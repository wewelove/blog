---
title: 内网部署 YApi
date: 2022-01-27 10:59:54
categories:
  - 工具
tags:
  - Yapi
  - pm2
---

Ubuntu 20.04 虚拟机下载手动部署 [YApi](https://hellosean1025.github.io/yapi/index.html) 服务。

- 适用 Yapi v1.10.x 版本

## 环境要求

- [nodejs](https://nodejs.org/zh-cn/) 版本 v12.x；
- [mongodb](https://www.mongodb.org.cn/) 版本 v4.4；

### 安装 nodejs

系统默认 nodejs 版本为 10.x ，直接安装可能失败，需要升级至 12.x 版本。

```sh
# 导入秘钥，注册源，更新
curl -fsSL https://deb.nodesource.com/setup_12.x | sudo -E bash -

# 安装
sudo apt install -y nodejs

# 替换 npm 源
npm config set registry https://registry.npmmirror.com
```

### 安装 mongodb

```sh
# 导入秘钥
wget -qO - https://www.mongodb.org/static/pgp/server-4.4.asc | sudo apt-key add -

# 注册源
echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu bionic/mongodb-org/4.4 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-4.4.list

# 更新并安装
sudo apt-get update && sudo apt-get install -y mongodb-org
```

如果以上操作不成功，请求参考官方 [安装过程](https://docs.mongodb.com/v4.4/tutorial/install-mongodb-on-ubuntu/)。

安装完成后将 mongodb 设置为系统服务，开机启动：

```sh
sudo systemctl enable mongod
```

## 安装部署

检查环境：

```sh
node -v # v12.22.8
npm -v # 6.14.15
```

使用官方推荐的可视化部署方式：

```sh
# 全局安装 YApi 命令行工具
sudo npm install -g yapi-cli

# 启动可视化安装界面
sudo yapi server # 在浏览器打开 http://0.0.0.0:9090 访问。非本地服务器，请将 0.0.0.0 替换成指定的域名或ip
```

根据提示在浏览器中打开安装页面，输入相关信息，点击 **开始部署**：

![部署](/images/yapi.png)

安装完成后注意结尾提示，注意保存管理员账号和密码：

```sh
初始化管理员账号成功,账号名："admin@admin.com"，密码："ymfe.org"
部署成功，请切换到部署目录，输入： "node vendors/server/app.js" 指令启动服务器, 然后在浏览器打开 http://127.0.0.1:3000 访问
```

## 服务管理

使用 pm2 管理应用进程:

```sh
# 安装 pm2
sudo npm install pm2 -g

# pm2 管理 yapi 服务，请求注意修改 Yapi 安装目录
sudo pm2 start "/<dir>/yapi/vendors/server/app.js" --name yapi

# 查看服务信息
sudo pm2 info yapi

# 停止服务
sudo pm2 stop yapi

# 重启服务
sudo pm2 restart yapi
```

将 pm2 设置为系统服务，开机启动 YApi：

```sh
# 设置 pm2 为系统服务
sudo pm2 upstart

# 保存已启动的应用
sudo pm2 save
```

更多请参考 pm2 [官方文档](https://pm2.keymetrics.io/docs/usage/quick-start/)。
