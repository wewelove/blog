---
title: 国产 ngrok 穿透内网, 开发测试神器 - sunny
date: 2017-08-25 14:44:42
categories:
  - 开发
tags:
  - ngrok
  - sunny
---

## ngrok 是什么

[百度百科](https://baike.baidu.com/item/ngrok/13986278?fr=aladdin)  
[官网](https://ngrok.com/)

> `ngrok` 是一个反向代理，通过在公共的端点和本地运行的 Web 服务器之间建立一个安全的通道。ngrok 可捕获和分析所有通道上的流量，便于后期分析和重放。  

官方版本 `ngrok` 有太多的限制，免费用户最多启动4个节点且不能自定义域名，所以我们跳过它直接介绍 `sunny` 的使用。

## sunny 才是福利

1. 注册账号

    先到 `sunny` 官网 [注册账号](https://www.ngrok.cc/login/register)。

2. 开通隧道

    在后台管理界面选择：隧道管理->开通隧道->选择免费产品，点击立即购买。

    ![ngrok-sunny-01](/images/ngrok-sunny-01.png)

    填写表单：隧道协议、隧道名称、前置域名、本地端口，然后点击确定添加。

    ![ngrok-sunny-02](/images/ngrok-sunny-02.png)

    返回隧道列表或点击隧道管理菜单，可以看到刚刚申请的隧道信息。

    ![ngrok-sunny-03](/images/ngrok-sunny-03.png)

3. 运行客户端

    下载 `sunny` [客户端](https://www.ngrok.cc/#down-client)，根据需要下载不同版本。

    命令行启动客户端：

    ```
    # windows
    sunny clientid d3fdc171be3a94a0,e3fc6b1431a51e89
    # linux
    ./sunny clientid d3fdc171be3a94a0,e3fc6b1431a51e89
    ```

    启动成功后就可以通过 `ngrok.cc` 二级域名访问自己的站点了。

## 自定义域名

1. 准备域名

    首先要有自己的域名，最好是顶级的，如果没有可以到 [freenom](http://www.freenom.com) 免费领取一枚。

2. 配置隧道自定义域名

    进入隧道编辑页面，修改域名类型、自定义域名，最后确认修改。

    ![ngrok-sunny-04](/images/ngrok-sunny-04.png)

3. 根据提示，修改域名的 `CNAME` 参数

    我的域名是从 [freenom](http://www.freenom.com) 申请的，配置方法如下图，其它域名请根据提供商文档操作。

    ![ngrok-sunny-05](/images/ngrok-sunny-05.png)

    > `Target` 项填入的内容要与 sunny 提供的一致，请参考上图

## 开机运行 sunny

这是在 Ubuntu 16.04 上做的尝试。

1. 新建 `sunny.sh` 文件

    ```
    #!/bin/bash
    count=`ps -ef | grep "sunny" | wc -l`

    if [ $count -gt 0 ]; then
    killall sunny;
    fi

    /root/sunny/sunny clientid de9e30c769c73e02,0fa3f0385e227f14
    ```

    `/root/sunny/sunny` 为 sunny 客户端所在路径，可以将多个隧道全部写在 `clientid` 后面。

2. 添加开机运行

    在 `/ect/rc.local` 文件最后增加如下命令：

    ```
    setsid /root/sunny/sunny.sh
    exit 0
    ```

3. 定时重启

    新建 `sunny.cron` 文件，输入如下内容：

    ```
    0 */1 * * * setsid /root/sunny/sunny.sh
    ```

    然后执行如下命令：

    ```
    crontab sunny.cron
    ```

    > 系统将每小时重启一次 `sunny`
