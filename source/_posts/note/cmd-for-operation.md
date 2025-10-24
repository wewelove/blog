---
title: CMD 运行命令大全
date: 2025-10-20 20:40:57
categories:
  - 笔记
tags:
  - CMD
  - 运维
---

## 系统管理与维护

1. **systeminfo**：显示系统详细信息（安装日期/补丁/内存等）
2. **sfc /scannow**：扫描并修复系统文件损坏 **[管理员]**
3. **chkdsk /f**：检查磁盘错误并修复（需重启）**[管理员]**
4. **cleanmgr**：启动磁盘清理工具
5. **defrag C: /O**：优化机械硬盘碎片（SSD无需使用）
6. **msinfo32**：打开系统信息面板
7. **winver**：显示Windows版本
8. **services.msc**：服务管理控制台
9. **compmgmt.msc**：计算机综合管理
10. **diskmgmt.msc**：磁盘分区管理
11. **devmgmt.msc**：设备管理器
12. **eventvwr**：事件查看器
13. **perfmon**：性能监视器
14. **taskschd.msc**：任务计划程序
15. **lusrmgr.msc**：本地用户和组管理
16. **control**：打开控制面板
17. **appwiz.cpl**：程序和功能面板
18. **sysdm.cpl**：系统属性设置
19. **secpol.msc**：本地安全策略
20. **rsop.msc**：组策略结果集
21. **slmgr.vbs -xpr**：检查系统激活状态
22. **wmic qfe list**：查看已安装补丁
23. **wmic bios get serialnumber**：获取BIOS序列号
24. **powercfg /energy**：生成电源效率报告
25. **ver**：显示操作系统版本
26. **hostname**：显示计算机名称
27. **time**：显示或设置系统时间
28. **date**：显示或设置系统日期
29. **shutdown /r /t 0**：立即重启系统
30. **shutdown /s /t 0**：立即关机

## 网络诊断与配置

31. **ipconfig /all**：显示完整网络配置
32. **ping 8.8.8.8 -t**：持续测试网络连通性
33. **tracert www\.baidu.com**：路由追踪
34. **netstat -ano**：查看所有网络连接
35. **nslookup www\.qq.com**：DNS解析验证
36. **arp -a**：显示ARP缓存表
37. **netsh interface ip show config**：显示网卡配置
38. **netsh interface ip set dns "以太网" static 8.8.8.8**：设置静态DNS
39. **netsh winsock reset**：重置网络协议栈
40. **netsh advfirewall set allprofiles state off**：临时关闭防火墙
41. **route print**：查看路由表
42. **net use K: \192.168.1.100\share**：映射网络驱动器
43. **netsh wlan show profiles**：查看保存的WiFi配置
44. **netsh wlan show profile name="Home" key=clear**：查看WiFi密码
45. **netsh wlan connect ssid="Office"**：连接指定WiFi
46. **pathping www\.taobao.com**：综合路由和丢包测试
47. **getmac /v**：显示MAC地址详情
48. **net view**：查看局域网共享资源
49. **net share**：管理共享文件夹
50. **ftp**：启动FTP客户端
51. **telnet**：启动Telnet客户端
52. **netsh trace start capture=yes**：启动网络抓包
53. **netsh trace stop**：停止网络抓包
54. **ipconfig /flushdns**：清除DNS缓存
55. **ipconfig /registerdns**：刷新DNS注册

## 文件与磁盘操作

56. **dir /s /ah**：递归显示目录及隐藏文件
57. **cd /d D:\logs**：跨盘符切换目录
58. **robocopy C:\src D:\backup /MIR /MT:8**：多线程镜像备份
59. **del /F /Q .tmp**：强制删除临时文件
60. **rd /S /Q "D:\old"**：无提示删除非空目录
61. **fsutil file createnew test.txt 1048576**：生成1MB测试文件
62. **type filename.txt**：显示文本文件内容
63. **copy file1.txt+file2.txt merged.txt**：合并文件
64. **xcopy C:\data D:\backup /E /H /C**：复制目录树
65. **move file.txt D:\newfolder**：移动文件
66. **ren old.txt new.txt**：重命名文件
67. **attrib +h secret.txt**：设置隐藏属性
68. **fc file1.txt file2.txt**：文件内容比较
69. **find "error" log.txt**：文件中搜索字符串
70. **tree /F**：显示目录树结构
71. **md newfolder**：创建新目录
72. **cipher /W:D:**：彻底擦除磁盘剩余空间
73. **compact /c /s**：启用NTFS压缩
74. **diskpart**：启动磁盘分区工具
75. **format E: /FS:NTFS /Q**：快速格式化磁盘

## 进程与用户管理

76. **tasklist /svc**：查看进程及关联服务
77. **taskkill /F /IM chrome.exe**：强制结束进程
78. **taskkill /PID 1234 /T**：结束进程树
79. **start notepad**：启动应用程序
80. **net user Tech P\@ssw0rd /add**：创建用户
81. **net localgroup administrators Tech /add**：提权为管理员
82. **net user Tech /delete**：删除用户
83. **query session**：查看远程桌面会话
84. **whoami**：显示当前用户
85. **runas /user:admin cmd**：以其他用户身份运行
86. **qwinsta**：显示终端服务会话（同query session）
87. **sc query**：查看服务状态
88. **sc stop WinDefend**：停止服务
89. **sc config DiagTrack start= disabled**：禁用服务
90. **wmic process get name,processid**：获取进程列表

- - -

## 运维实战命令

91. **for /L %i in (1,1,100) do ping -n 1 192.168.1.%i**：批量Ping扫描
92. **forfiles /p "C:\logs" /s /m *.log /d -7 /c "cmd /c del @path"**：自动清理日志
93. **auditpol /set /category:"Account Logon" /success:enable**：启用登录审核
94. **wmic product get name,version > software.csv**：导出软件清单
95. **wmic memorychip get capacity,speed**：获取内存信息
96. **wmic diskdrive get model,size**：获取硬盘信息
97. **reg query "HKLM\Software\Microsoft\Windows\CurrentVersion\Run"**：查看启动项
98. **schtasks /query /fo LIST /v**：查看计划任务详情
99. **vssadmin list shadows**：查看卷影副本
100. **powercfg /batteryreport**：生成电池健康报告

## 高危操作预警

```bat
:: 永久删除命令（不可恢复）  
format C: /FS:NTFS  
diskpart → clean  

:: 谨慎使用的命令  
del /F /S /Q *.*  
rd /S /Q C:\Windows  
```

## 运维黄金法则

1. **测试环境验证**：危险命令先在测试机执行
2. **权限最小化**：日常操作避免使用管理员权限
3. **双重确认**：执行删除/格式化前确认路径
4. **日志记录**：关键操作用 `> log.txt` 保存记录
