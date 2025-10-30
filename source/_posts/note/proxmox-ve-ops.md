---
title: Proxmox VE 日常维护
series: Proxmox-VE
categories:
  - - 笔记
tags:
  - PVE Kernel Cleaner
  - Proxmox VE
abbrlink: f1e76699
date: 2025-10-30 11:06:30
---

## 移除未使用的 Linux 内核

若不存在 `pvekclean`，请先安装：

```sh
git clone https://github.com/jordanhillis/pvekclean.git
cd pvekclean
chmod +x pvekclean.sh

```

安装完成后执行 `pvekclean` 即可：

```sh
./pvekclean.sh
```

## 软件更新

```sh
apt update -y && apt dist-upgrade -y
```

## 安装常用软件

```sh
apt-get update
apt-get install vim lrzsz unzip net-tools curl screen tmux uuid-runtime git -y
apt dist-upgrade -y
```

## 去掉未订阅的提示

适用 Proxmox VE 6.3/6.4/7.0/7.1/7.2/7.3/7.4/8.0/8.1/8.2/8.3/8.4/9.0 等版本。

```sh
sed -i_orig "s/data.status === 'Active'/true/g" /usr/share/pve-manager/js/pvemanagerlib.js
sed -i_orig "s/if (res === null || res === undefined || \!res || res/if(/g" /usr/share/javascript/proxmox-widget-toolkit/proxmoxlib.js
sed -i_orig "s/.data.status.toLowerCase() !== 'active'/false/g" /usr/share/javascript/proxmox-widget-toolkit/proxmoxlib.js
systemctl restart pveproxy
```

确认无误后，重新启动服务器：

```sh
reboot
```

## 增加硬件概要信息显示

```sh
# 稳定版
wget -q -O /root/pve_source.tar.gz \
'https://bbs.x86pi.cn/file/topic/2023-11-28/file/01ac88d7d2b840cb88c15cb5e19d4305b2.gz' \
&& tar zxvf /root/pve_source.tar.gz && /root/./pve_source
```

> 根据提示选择 `7，1，o，<enter>`

## 免密登录

```sh
cd ~
mkdir .ssh
echo ssh-rsa AAAAB3NzaC1yc2EAAAABIwAAAQEAyq1pB5aF0w6ps4OzwQl1C8uP41Iq7J+gqylLMXkoESrTUVhH1+irHuImxi2At886sO7x9s+b4jhRZoJZpZURPU4UmzUEBHKoXlqOf9eO//GtUita2AaPFw5tc0YgLPrgnO+z5MKfjo20aoJtVBvleRA/0YJcWy1a6ufXa8944D8a1Dirc9uVNR5QjKVFRbQt/twLkLdFB6t16HCwISKCVI56DcJOoY2g7mXI8clKaESeB+ANIhSKJclPwjoC6P0pHFfgqNauxC+0xugx3W2ZSIkVhdZu1L7iKvzXXPiETjPQA6qMjp/1dY2WU49Lf+wDOQplCy4HLq7QqNNVSzIBGw== Administrator@PCOS-1407251925 >> ~/.ssh/authorized_keys
```

> 请将密钥更换为您自己的公钥

## 解决无法上网的问题

配置 DNS，解决无法上网的问题：

```sh
vi /etc/resolv.conf
# 添加阿里云 DNS 服务器
nameserver 223.5.5.5
nameserver 223.6.6.6
```

重启网络服务：

```sh
service networking restart
```

## UI 增加显示脚本

```sh
# 稳定版
wget -q -O /root/pve_source.tar.gz \
'https://bbs.x86pi.cn/file/topic/2023-11-28/file/01ac88d7d2b840cb88c15cb5e19d4305b2.gz' \
&& tar zxvf /root/pve_source.tar.gz && /root/./pve_source
```

## 设置国内源

### PVE 6.x

设置 debian 阿里云源：

```sh
cat > /etc/apt/sources.list <<EOF
deb http://mirrors.huaweicloud.com/debian/ buster main non-free contrib
deb http://mirrors.huaweicloud.com/debian/ buster-updates main non-free contrib
deb http://mirrors.huaweicloud.com/debian/ buster-backports main non-free contrib
deb-src http://mirrors.huaweicloud.com/debian/ buster main non-free contrib
deb-src http://mirrors.huaweicloud.com/debian/ buster-updates main non-free contrib
deb-src http://mirrors.huaweicloud.com/debian/ buster-backports main non-free contrib
deb http://mirrors.huaweicloud.com/debian-security/ buster/updates main non-free contrib
deb-src http://mirrors.huaweicloud.com/debian-security/ buster/updates main non-free contrib
EOF
```

删除企业源：

```sh
rm -rf /etc/apt/sources.list.d/pve-enterprise.list
```

下载秘钥：

```sh
wget http://mirrors.ustc.edu.cn/proxmox/debian/proxmox-ve-release-6.x.gpg -O /etc/apt/trusted.gpg.d/proxmox-ve-release-6.x.gpg
```

添加国内源：

```sh
echo "deb http://mirrors.ustc.edu.cn/proxmox/debian/pve buster pve-no-subscription" >/etc/apt/sources.list.d/pve-install-repo.list
apt update -y && apt dist-upgrade -y
```

### PVE 7.x

设置 debian 阿里云源：

```sh
cat > /etc/apt/sources.list <<EOF
deb https://mirrors.huaweicloud.com/debian/ bullseye main non-free contrib
deb-src https://mirrors.huaweicloud.com/debian/ bullseye main non-free contrib
deb https://mirrors.huaweicloud.com/debian-security/ bullseye-security main
deb-src https://mirrors.huaweicloud.com/debian-security/ bullseye-security main
deb https://mirrors.huaweicloud.com/debian/ bullseye-updates main non-free contrib
deb-src https://mirrors.huaweicloud.com/debian/ bullseye-updates main non-free contrib
deb https://mirrors.huaweicloud.com/debian/ bullseye-backports main non-free contrib
deb-src https://mirrors.huaweicloud.com/debian/ bullseye-backports main non-free contrib
EOF
```

删除企业源：

```sh
rm -rf /etc/apt/sources.list.d/pve-enterprise.list
```

下载秘钥：

```sh
wget http://mirrors.ustc.edu.cn/proxmox/debian/proxmox-release-bullseye.gpg -O /etc/apt/trusted.gpg.d/proxmox-release-bullseye.gpg
```

添加国内源：

```sh
echo "deb http://mirrors.ustc.edu.cn/proxmox/debian/pve bullseye pve-no-subscription" > /etc/apt/sources.list.d/pve-install-repo.list
apt update -y && apt dist-upgrade -y
```

### PVE 8.x

设置 debian 阿里云源：

```sh
cat > /etc/apt/sources.list <<EOF
deb https://mirrors.huaweicloud.com/debian/ bookworm main non-free contrib
deb-src https://mirrors.huaweicloud.com/debian/ bookworm main non-free contrib
deb https://mirrors.huaweicloud.com/debian-security/ bookworm-security main
deb-src https://mirrors.huaweicloud.com/debian-security/ bookworm-security main
deb https://mirrors.huaweicloud.com/debian/ bookworm-updates main non-free contrib
deb-src https://mirrors.huaweicloud.com/debian/ bookworm-updates main non-free contrib
deb https://mirrors.huaweicloud.com/debian/ bookworm-backports main non-free contrib
deb-src https://mirrors.huaweicloud.com/debian/ bookworm-backports main non-free contrib
EOF
```

删除企业源

```sh
rm -rf /etc/apt/sources.list.d/pve-enterprise.list
```

下载秘钥：

```sh
wget http://mirrors.ustc.edu.cn/proxmox/debian/proxmox-release-bookworm.gpg -O /etc/apt/trusted.gpg.d/proxmox-release-bookworm.gpg
```

添加国内源：

```sh
echo "deb http://mirrors.ustc.edu.cn/proxmox/debian/pve bookworm pve-no-subscription" > /etc/apt/sources.list.d/pve-install-repo.list
apt update -y && apt dist-upgrade -y
```

### PVE 9.x

设置 debian 阿里云源：

```sh
cat > /etc/apt/sources.list <<EOF
deb https://mirrors.huaweicloud.com/debian/ trixie main non-free contrib
deb-src https://mirrors.huaweicloud.com/debian/ trixie main non-free contrib
deb https://mirrors.huaweicloud.com/debian-security/ trixie-security main
deb-src https://mirrors.huaweicloud.com/debian-security/ trixie-security main
deb https://mirrors.huaweicloud.com/debian/ trixie-updates main non-free contrib
deb-src https://mirrors.huaweicloud.com/debian/ trixie-updates main non-free contrib
deb https://mirrors.huaweicloud.com/debian/ trixie-backports main non-free contrib
deb-src https://mirrors.huaweicloud.com/debian/ trixie-backports main non-free contrib
EOF
```

删除企业源：

```sh
rm -rf /etc/apt/sources.list.d/pve-enterprise.list
```

下载秘钥：

```sh
wget http://mirrors.ustc.edu.cn/proxmox/debian/proxmox-release-trixie.gpg -O /etc/apt/trusted.gpg.d/proxmox-release-trixie.gpg
```

添加国内源：

```sh
echo "deb http://mirrors.ustc.edu.cn/proxmox/debian/pve trixie pve-no-subscription" > /etc/apt/sources.list.d/pve-install-repo.list
apt update -y && apt dist-upgrade -y
```

## 安装并设置 NTP 服务

### PVE 6.x

> 参考[https://pve.proxmox.com/wiki/Time\_Synchronization](https://pve.proxmox.com/wiki/Time_Synchronization)

新增阿里云的公共 NTP 地址：

```sh
mv /etc/systemd/timesyncd.conf /etc/systemd/timesyncd.conf_bak
echo [Time] >> /etc/systemd/timesyncd.conf
echo NTP=ntp1.aliyun.com ntp2.aliyun.com ntp3.aliyun.com ntp4.aliyun.com ntp5.aliyun.com ntp6.aliyun.com ntp7.aliyun.com >>    /etc/systemd/timesyncd.conf
cat /etc/systemd/timesyncd.conf
timedatectl set-ntp true
timedatectl status
```

### PVE 7.x/8.x/9.x

> 参考[https://help.aliyun.com/document\_detail/187016.html?utm\_content=g\_1000230851&spm=5176.20966629.toubu.3.f2991ddcpxxvD1#title-ik2-31x-dso](https://help.aliyun.com/document_detail/187016.html?utm_content=g_1000230851&spm=5176.20966629.toubu.3.f2991ddcpxxvD1#title-ik2-31x-dso)

新增阿里云的公共 NTP 地址：

```sh
vim /etc/chrony/chrony.conf

# 添加以下服务
server ntp1.aliyun.com minpoll 4 maxpoll 10 iburst
server ntp2.aliyun.com minpoll 4 maxpoll 10 iburst
server ntp3.aliyun.com minpoll 4 maxpoll 10 iburst
server ntp4.aliyun.com minpoll 4 maxpoll 10 iburst
server ntp5.aliyun.com minpoll 4 maxpoll 10 iburst
server ntp6.aliyun.com minpoll 4 maxpoll 10 iburst
server ntp7.aliyun.com minpoll 4 maxpoll 10 iburst
```

## 修改 PVE 默认的 IP 地址

```sh
vim /etc/network/interfaces
```

会出现类似下面的配置文件：

```sh
auto lo
iface lo inet loopback

iface enp2s0 inet manual

auto vmbr0
iface vmbr0 inet static
    address 192.168.1.2
    netmask 255.255.255.0
    gateway 192.168.1.1
    bridge_ports enp2s0
    bridge_stp off
    bridge_fd 0
```

建议只修改 `address`，`netmask` 和 `gateway` 这 3 个配置值即可，含义分别是 `IP地址`，`子网掩码` 和 `网关地址`。

## 异常处理

如果更新时出现错误 `E: Sub-process /usr/bin/dpkg returned an error code`

[参考文章](https://blog.csdn.net/yusiguyuan/article/details/24269129)

```sh
apt-get update --fix-missing
apt-get autoremove && sudo apt-get clean && sudo apt-get install -f
```

如果更新时出现错误 `You are attempting to remove the meta-package 'proxmox-ve'`

[参考文章](https://forum.proxmox.com/threads/apt-get-dist-upgrade-wants-to-remove-proxmox-ve-pve-firmware.39360/)

```sh
# Yes, I've tested it. I can remove any kernels listed with this command:
# 列出当前系统的Linux镜像
dpkg --list | egrep -i --color 'linux-image|linux-headers'
# Then:
# 删除旧的Linux镜像
apt-get --purge remove linux-image-4.9.0-4-amd64 linux-image-4.9.0-5-amd64
# 更新 grub
update-grub

```

解决 `locale: Cannot set LC_CTYPE to default locale: No such file or directory` 报错

[参考文章](https://www.cyberciti.biz/faq/os-x-terminal-bash-warning-setlocale-lc_ctype-cannot-change-locale/)

```sh
# localedef -i en_US -f UTF-8 en_US.UTF-8
```
