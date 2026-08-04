---
title: 局域网内 https 证书部署指南
categories:
  - - 笔记
tags:
  - '开发, 运维'
published: true
abbrlink: 40419f77
date: 2026-08-04 12:11:23
series:
---

针对 **局域网** 场景，公网 CA（如Let's Encrypt）通常无法验证内网域名或IP地址，因此 **最实用的方案是自签名证书或搭建私有CA**。
下面给出两种完整路径，并重点说明如何避免浏览器 **不安全** 警告。

## 一、方案对比

| 方案 | 适用场景 | 客户端需要额外操作 | 维护复杂度 |
|---|---|---|---|
| **自签名证书** | 单台服务器，少量客户端 | 每台客户端导入该证书并信任 | 低 |
| **私有CA + 签发服务器证书** | 多台服务器，企业内网 | 每台客户端仅需导入一次根证书，后续所有由该 CA 签发的证书自动受信 | 中（需安全保管 CA 私钥） |

> ✅ 推荐采用 **私有 CA** 方案，尤其当内网有多个 HTTPS 服务时，只需部署一次根证书。

## 二、搭建私有 CA 并签发内网证书（以 OpenSSL 为例）

以下操作在 **一台安全的机器**（如管理机、堡垒机）上执行。

假设你的域名为 `v4coder.cn`, 组织名称为 `V4Coder`。

### 1. 生成根 CA 私钥和自签名证书

```bash
# 生成根 CA 私钥（妥善保管，不要泄露）
openssl genrsa -out ca.key 4096

# 生成根 CA 证书（有效期 10 年）
openssl req -x509 -new -nodes -key ca.key -sha256 -days 3650 \
  -subj "/C=CN/ST=YN/L=Kunming/O=V4Coder/CN=v4coder.cn" \
  -out ca.crt

# Windows 环境下报错, 可以尝试使用以下命令生成
MSYS_NO_PATHCONV=1 openssl req -x509 -new -nodes -key ca.key -sha256 -days 3650 \
  -subj "/C=CN/ST=YN/L=Kunming/O=V4Coder/CN=v4coder.cn" \
  -out ca.crt
```

`ca.crt` → 需要分发到所有客户端并信任。  
`ca.key` → 仅用于签发证书，可离线保存。

在这条 OpenSSL 命令中，`-subj` 参数用于指定证书的主题（Subject）信息。
您提到的 `C`, `ST`, `L`, `O`, `CN` 是 X.509 证书标准中常用的字段，它们的具体含义如下：

- **C (Country Name)**：国家/地区代码。通常使用两个字母的缩写，例如 `CN` 代表中国。
- **ST (State or Province Name)**：州或省份名称。例如 `YN` 代表云南省。
- **L (Locality Name)**：城市或地区名称。例如 `Kunming` 代表昆明市。
- **O (Organization Name)**：组织或公司名称。例如 `kmnt3b` 代表您所在的机构或公司名称。
- **CN (Common Name)**：公用名称。对于 SSL 证书，这通常是网站的完全合格域名（FQDN）；而对于 CA 根证书，则通常是该 CA 的名称

这些字段组合在一起，构成了证书持有者的唯一标识信息（Distinguished Name），有助于确认证书主体的身份合法性。

### 2. 生成服务器证书请求（包含SAN扩展）

**创建 OpenSSL 配置文件** `server.conf`：

```ini
[req]
default_bits = 2048
prompt = no
default_md = sha256
distinguished_name = dn
req_extensions = v3_req

[dn]
C = CN
ST = YN
L = Kunming
O = V4Coder
# 通用名称（CN），现代浏览器主要参考 SAN，此字段可随意填写但建议与第一个域名保持一致
CN = *.v4coder.cn

[v3_req]
keyUsage = digitalSignature, keyEncipherment
extendedKeyUsage = serverAuth
subjectAltName = @alt_names

[alt_names]
# 通配符域名，匹配所有一级子域，如 hello.v4coder.cn、api.v4coder.cn 等
DNS.1 = *.v4coder.cn
# 可选：若需要同时保护主域名 v4coder.cn 本身（不包含通配符），请取消下一行的注释
# DNS.2 = v4coder.cn
```

然后生成私钥和 CSR：

```bash
openssl genrsa -out private.key 2048

openssl req -new -key private.key \
  -out server.csr \
  -config server.conf
```

### 3. 使用根 CA 签发服务器证书（有效期可设 1~3 年）

```bash
openssl x509 -req -in server.csr \
  -CA ca.crt -CAkey ca.key -CAcreateserial \
  -out server.crt -days 730 -sha256 \
  -extensions v3_req -extfile server.conf
```

```bash
cat server.crt ca.crt > fullchain.cer
```

### 4. 配置 Web 服务器

**Nginx 示例**（监听 443 端口）：

```nginx
server {
  listen 443 ssl;
  listen [::]:443 ssl;
  server_name hello.v4coder.cn;
  root /var/www/html;
  index index.html index.htm index.nginx-debian.html;
  ssl_certificate /etc/nginx/ssl/v4coder.cn/fullchain.cer;
  ssl_certificate_key /etc/nginx/ssl/v4coder.cn/private.key;
  location / {
    try_files $uri $uri/ =404;
  }
}
```

重启 Nginx 后，服务器即支持 HTTPS。

## 三、让局域网客户端信任该证书

不同的操作系统需要将 **根证书 `ca.crt`** 导入 **受信任的根证书颁发机构** 存储区。

### Windows

1. 双击 `ca.crt` → 选择 **安装证书** → 存储位置选 **本地计算机**
2. 选择 **将所有证书放入下列存储** → 浏览 → **受信任的根证书颁发机构**
3. 确认，重启浏览器即可。

### macOS

```bash
sudo security add-trusted-cert -d -r trustRoot \
  -k /Library/Keychains/System.keychain ca.crt
```

### Linux (Ubuntu/Debian)

```bash
sudo cp ca.crt /usr/local/share/ca-certificates/v4coder.cn.crt
sudo update-ca-certificates
```

### 在浏览器中（不推荐生产，仅临时测试）

访问 `https://<内网IP>` 出现警告时，点击“高级” → “继续前往（不安全）” —— 但这种方法每个页面都需手动确认，不推荐。

## 四、特殊情况：仅单台服务器 + 少数客户端

如果你只想快速测试，可以用之前的单条命令生成自签名证书：

```bash
openssl req -x509 -newkey rsa:2048 -nodes \
  -keyout server.key \
  -out server.crt \
  -days 365 -subj "/CN=192.168.1.100"
```

然后将 `server.crt` 安装到每台客户端（导入“受信任的根证书”），注意该证书不含SAN，Chrome可能仍报“缺少subjectAltName”错误。
要彻底解决必须**采用前面带SAN的配置**。

> Chrome 从 58 版本起要求证书必须包含 subjectAltName（SAN），否则即使手动信任也会显示 **NET::ERR_CERT_COMMON_NAME_INVALID**。
> 因此 **务必使用带 `alt_names` 的配置**。

## 五、常见问题

- **浏览器依然提示不安全**  

  检查证书是否包含正确的 SAN（使用 `openssl x509 -in server.crt -text -noout` 查看）  
  确认根证书已正确导入客户端的 “受信任的根证书颁发机构”  
  确认访问的 URL 与证书中的 CN 或 SAN 完全一致（包括端口号）

- **Let's Encrypt 能否用在局域网？**  

  如果局域网服务器拥有公网域名，且能在公网验证（如开放 80 端口或配置 DNS-01 挑战），则可以使用。但纯内网无公网域名或无法开放端口时不可行。

- **如何避免每台客户端手动安装证书？**  

  在企业域环境中，可通过组策略（Windows）或 MDM 批量推送根证书。

- **证书有效期已过**  

  重新执行签发步骤，使用旧CA即可（不必重新创建根证书）。

采用私有 CA 方案后，你可以在局域网内任意签发证书，客户端只需要 **信任一次根证书**，后续所有服务都会自动受信。
如有更多细节问题（如 Windows 组策略部署、反向代理场景），欢迎继续提问。
