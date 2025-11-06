---
title: MariaDB 数据库安装设置
abbrlink: aeb50ee
categories:
  - [笔记]
tags:
  - mariadb
date: 2020-02-11 12:14:57
---

## 安装

### Ubuntu18 系统

1. 设置 MariaDB 仓库

    默认情况下 MariaDB 的包没有在 Ubuntu 仓库中，请参考 MariaDB 官方进行设置 [**移步**](https://downloads.mariadb.org/mariadb/repositories/#mirror=neusoft)。  
    参考官方设置时请选择有效的仓库镜像地址，下面选择的是清华大学镜像，测试可用。

    ```sh
    sudo apt-get install software-properties-common
    sudo apt-key adv --fetch-keys 'https://mariadb.org/mariadb_release_signing_key.asc'
    sudo add-apt-repository 'deb [arch=amd64,arm64,ppc64el] http://mirror.its.dal.ca/mariadb/repo/10.4/ubuntu bionic main'
    ```

2. 安装

    安装过程中要输入 MariaDB 的 `root` 密码。

    ```sh
    sudo apt update
    sudo apt install mariadb-server
    ```

3. 基本操作

    ```sh
    # 链接
    $ mysql -u root -p
    # 停止
    $ sudo /etc/init.d/mysql stop
    # 启动
    $ sudo /etc/init.d/mysql start
    # 重启
    $ sudo /etc/init.d/mysql restart
    ```

## 设置

### 设置 MariaDB 开启远程访问

1. 检查有没有设置防火墙或者 iptables 规则

2. 检查 3306 端口是否打开

    检查端口状态，下面输出说明端口只在 `127.0.0.1` 上监听，所以无法通过其它 IP 访问。

    ```sh
    $ netstat -an | grep 3306
    tcp    0   0  127.0.0.1:3306    0.0.0.0:*     LISTEN
    ```

    解决办法：修改 `/etc/mysql/my.cnf` 文件，打开文件找到下面一行内容。  
    可以将 `127.0.0.1` 换成合适的 IP，建议直接将此行注释掉。

    ```ini
    # Instead of skip-networking the default is now to listen only on
    # localhost which is more compatible and is not less secure.
    bind-address  = 127.0.0.1
    ```

    重启 MariaDB 服务，再次检测：

    ```sh
    $ sudo /etc/init.d/mysql restart
    $ netstat -an | grep 3306
    tcp    0   0  0.0.0.0:3306    0.0.0.0:*     LISTEN
    ```

3. 给 MariaDB 用户 root 分配远程访问权限

    测试时如果输出如下内容，说明不允许链接服务器。

    ```sh
    $ mysql -h 192.168.0.101 -u root -p
    Enter password:
    ERROR 1130 (00000): Host 'Ubuntu-Fvlo.Server' is not allowed to connect to this MySQL server
    ```

    解决办法：给 root 用户分配远程访问的权限，密码设置为 `123456`。

    ```sh
    $ mysql -u root -p
    Enter password:
    Welcome to the MariaDB monitor.  Commands end with ; or \g.
    Your MariaDB connection id is 28
    Server version: 5.5.51-MariaDB-1~trusty mariadb.org binary distribution
    ......
    MariaDB [(none)]> grant all on *.* to root@'%' identified by '123456';
    Query OK, 0 rows affected (0.00 sec)
    ```

    再次测试，成功。

    ```sh
    $ mysql -h 192.168.0.101 -u root -p
    Enter password:
    Welcome to the MariaDB monitor.  Commands end with ; or \g.
    Your MariaDB connection id is 28
    Server version: 5.5.51-MariaDB-1~trusty mariadb.org binary distribution
    ......
    MariaDB [(none)]>
    ```

    > CentOS6.6 要关闭防火强  
    > `service iptables stop` # 停止  
    > `chkconfig iptables off` # 禁用  

## 参考

[MySQL 数据库无法远程连接的解决办法](http://www.cnblogs.com/beanmoon/p/3173924.html)
