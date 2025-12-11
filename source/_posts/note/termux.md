---
title: 安卓机上安装 linux 系统打造成生产力工具
abbrlink: 28558022
date: 2025-12-09 15:21:11
series: termux
categories:
  - - 笔记
tags:
  - android
  - termux
  - linux
  - avnc
  - vnc
---


安卓手机上安装 Linux 系统，安装要求很低，手机无需刷机、无需 root，本次所需要用到的软件有：

- [Termux](https://github.com/termux/termux-app)
- [AVNC](https://github.com/gujjwal00/avnc)
- [VNC Viewer](https://help.realvnc.com/hc/en-us/articles/360002762697-Where-can-I-find-the-APK-files-for-RealVNC-Viewer-and-RealVNC-Server-for-Android)
- [VNC Viewer 中文版](https://github.com/w311ang/VNC-Viewer_Android_zh-CN)

下载并安装以上软件，完成后再继续后面的操作：

![Termux-AVNC-VNC-Viewer](../../images/termux/20251209160220.png)

![Termux-AVNC-VNC-Viewer](../../images/termux/20251209162636.png)

安装步骤相对较多，跟着操作基本上是没有问题的，遇到终端交互的地方根据提示该输入 `y` 就输入 `y` 遇事不决就 `回车`。

## 更新 Termux

我们首次打开安装的 `Termux` 应用，会弹窗提示获取发送通知权限，将需要的权限全给足，然后 `Termux` 会处于 `Installing bootstrap packages...` 状态，等待完成后会如图所示：

![图片](../../images/termux/20251209162744.png)

接下来我们要执行下方的命令更新软件包：

```bash
pkg update
pkg upgrade
pkg install -y curl
```

如果在更新的过程中遇到提示输入例如 `y/n` 这种交互操作统一输入 `y` 回车继续。

![图片](../../images/termux/20251209163124.png)

## 部署容器和系统

手动部署非常的麻烦，使用大佬提供了一键安装脚本能够快速的安装需要的服务：

[Tome 一键安装脚本](https://github.com/2moe/tmoe)

执行下面的脚本进行安装：

```bash
bash -c "$(curl -L l.tmoe.me)"
```

![图片](../../images/termux/20251209163929.png)

![图片](../../images/termux/20251209164022.png)

在这一步操作中，遇到所有的交互都给予 `y` 并回车即可，如果遇到获取读写手机上的照片、视频、音频、文件等权限，都给予授权并回车。

当看到下面的界面时保持选择 `简体中文` 直接回车：

![图片](../../images/termux/20251209164111.png)

进入到下面的界面后，同样在第一项进行回车，遇到提示回车即可，然后还会回到下方的这个界面：

![图片](../../images/termux/20251209164147.png)

在上面的截图中保持选中 `proot 容器` 回车，之后的操作按照下面的一组截图选择即可：

![图片](../../images/termux/20251209164353.png)

1 Fonts

![图片](../../images/termux/20251209164437.png)

2  termux.properties

![图片](../../images/termux/20251209164503.png)

3 NDS

![图片](../../images/termux/20251209164533.png)

4 一言

![图片](../../images/termux/20251209164614.png)

5 Timezone

![图片](../../images/termux/20251209164637.png)

6 当遇到 `共享 sd 目录` 时选择 `自定义路径`：

![图片](../../images/termux/20251209164712.png)

我们可以在文件管理器中的根目录下新建 `Linux` 目录，然后填写路径回车确定：

![图片](../../images/termux/20251209165033.png)

然后在遇到 `共享 HOME 目录` 保持选中第一项回车：

![图片](../../images/termux/20251209165105.png)

当出现下方的界面：

![图片](../../images/termux/20251209165141.png)

在这一步我们选择 `arm64发行版列表` 回车然后进入到：

![图片](../../images/termux/20251209165229.png)

选择 `ubuntu`，然后回车选择 `24.04(LTS)` 发行版本：

![图片](../../images/termux/20251209165318.png)

继续回车，如下图所示，选择第一项回车 `启动proot ubuntu-noble_arm64`：

![图片](../../images/termux/20251209165445.png)

![图片](../../images/termux/20251209165702.png)

![图片](../../images/termux/20251209170111.png)

等待几分钟后，会让我们创建一个 `sudo` 用户，根据提示创建用户即可：

![图片](../../images/termux/20251209170125.png)

接下来根据下面的一组截图对应的选项进行回车操作即可：

![图片](../../images/termux/20251209170302.png)

1 zsh

![图片](../../images/termux/20251209170447.png)

2 `zsh.sh & zsh-i.sh`

![图片](../../images/termux/20251209170515.png)

3 tmoe-linux-tool

![图片](../../images/termux/20251209170622.png)

4 回车继续后，等待几分钟之后大概会进入到询问安装桌面的界面中：

![图片](../../images/termux/20251209172055.png)

在这个界面选择第一项进行回车，然后按照下面一组截图进行回车操作：

![图片](../../images/termux/20251209172346.png)

1 xfce

![图片](../../images/termux/20251209172405.png)

![图片](../../images/termux/20251209172500.png)

![图片](../../images/termux/20251209172539.png)

5 然后这里选择 `xfce` 后回车又是漫长的等待。

![图片](../../images/termux/20251209172742.png)

等待执行完成

大约等待二十多分钟，会提示 `开启 vnc` 服务，由于 `Termux` 本身无法输出图形界面，所以我们需要开启，这里选择 `tiger` 回车：

![图片](../../images/termux/20251209181527.png)

然后设置 vnc 的密码：

![图片](../../images/termux/20251209181559.png)

然后设置端口默认即可：

![图片](../../images/termux/20251209181621.png)

设置分辨率，1080p 的屏幕选择 `Yes`，其他的就选择 `No`：

![图片](../../images/termux/20251209181659.png)

然后根据提示一路回车等待结束进入新的界面：

![图片](../../images/termux/20251209181728.png)

之后的操作按照下面的一组截图进行：

![图片](../../images/termux/20251209182605.png)

![图片](../../images/termux/20251209182656.png)

![图片](../../images/termux/20251209182742.png)

退出后，会进入到终端界面，接着按下 `回车` 等待代码执行完成后会进入到如下的状态：

![图片](../../images/termux/20251209183009.png)

到这步后我们退出 `Termux` 软件杀掉软件后台，并重新启动 `Termux`。

## 启动 VNC 服务

重新启动 `Termux` 后，我们输入下方的命令启动 vnc 服务：

```bash
startvnc
```

![图片](../../images/termux/20251209183339.png)

然后我们打开安装好的 `VNC Viewer` 或者 `AVNC` 软件，按照如下进行添加远程：

![图片](../../images/termux/20251210102053.png)

![图片](../../images/termux/20251209183552.png)

![图片](../../images/termux/20251209183756.png)

添加完成后进行连接即可：

> 电脑端安装 VNC Viewer 也可以进行远程连接

![图片](../../images/termux/20251209183941.png)

连接成功

桌面上的 `主文件夹` 中的 `sd` 目录则是跟我们之前映射的 `Linux` 目录是互通的，可以通过这个文件夹来传递文件：

![图片](../../images/termux/20251209184042.png)
