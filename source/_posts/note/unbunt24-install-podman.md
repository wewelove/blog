---
title: Ubuntu 24.04 安装 Podman
abbrlink: 20f0cc91
series: Podman
categories:
  - - 笔记
tags:
  - Podman
  - 容器
date: 2025-10-30 11:49:22
---

Podman（POD Manager）是一个开源的无守护进程（daemonless）容器引擎，用于管理容器、容器镜像、容器卷和网络。

它兼容 OCI 标准，可以运行 Docker 镜像，并且设计上与 Docker CLI 命令高度兼容。

## 安装 Podman

```sh
# 更新软件包并安装
sudo apt update
sudo apt install podman podman-compose buildah slirp4netns fuse-overlayfs -y

# 验证安装
podman --version
podman-compose --version
```

## 配置 Rootless 模式

```sh
# 启用用户命名空间
echo 'user.max_user_namespaces=28633' | sudo tee -a /etc/sysctl.d/99-podman.conf
sudo sysctl -p /etc/sysctl.d/99-podman.conf

# 配置 subuid 和 subgid
sudo usermod --add-subuids 100000-165535 --add-subgids 100000-165535 $USER

# 重新登录或重启使配置生效
```

## 配置存储驱动

```sh
# 创建配置文件
mkdir -p ~/.config/containers

# 添加常用存储驱动
cat > ~/.config/containers/storage.conf << EOF
[storage]
driver = "overlay"
runroot = "/run/user/$(id -u)/containers"
graphroot = "/home/$USER/.local/share/containers/storage"

[storage.options.overlay]
ignore_chown_errors = "true"
mount_program = "/usr/bin/fuse-overlayfs"
EOF
```

## 启用 API 服务

```sh
# 安装 Podman API 服务
sudo apt install -y podman-docker

# 启动 socket 服务
systemctl --user enable podman.socket
systemctl --user start podman.socket

# 设置环境变量
echo 'export DOCKER_HOST=unix://$XDG_RUNTIME_DIR/podman/podman.sock' >> ~/.bashrc
```

## 添加国内镜像源

```sh
# 创建配置文件
mkdir -p ~/.config/containers

# 添加常用国内镜像源列表
cat > ~/.config/containers/registries.conf << EOF
unqualified-search-registries = ["docker.io"]

[[registry]]
location = "docker.io"

# 中国科学技术大学镜像源
[[registry.mirror]]
location = "docker.mirrors.ustc.edu.cn"

# 网易镜像源
[[registry.mirror]]
location = "hub-mirror.c.163.com"

# 百度镜像源
[[registry.mirror]]
location = "mirror.baidubce.com"

# 腾讯云镜像源
[[registry.mirror]]
location = "ccr.ccs.tencentyun.com"

# 上海交大镜像源
[[registry.mirror]]
location = "docker.mirrors.sjtug.sjtu.edu.cn"
EOF
```

## 配置 Docker 兼容

```sh
cat > ~/.bashrc << EOF
# Podman 替代 Docker 配置
docker() {
  if [ "$1" = "compose" ]; then
    shift
    podman-compose "$@"
  else
    podman "$@"
  fi
}
EOF
```

验证：

```sh
# 重新加载配置
source ~/.bashrc

# 启动 Podman socket 服务
systemctl --user start podman.socket

# 验证命令
docker --version
docker compose --version
docker compose version
docker ps
docker images

# 测试 Docker 兼容性
docker run --rm hello-world

# 检查 API 连接
curl -H "Content-Type: application/json" --unix-socket $XDG_RUNTIME_DIR/podman/podman.sock http://localhost/_ping
```
