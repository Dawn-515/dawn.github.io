---
title: windows
date: 2024-06-24 02:30:20
tags:
---

# windows

### 1免密自动登录

#### 1.1 使用微软账户登录

win+r  运行   netplwiz  
取消勾选  “要使用本计算机，用户必须输入用户名密码”  
然后两遍输入**微软账户的密码**，无需修改用户名

#### 1.2 使用本地账户登录

### 2 windows开启SSH服务端

***服务端windows***打开**设置**，依次选择**应用**，**应用和功能**，**可选功能**，**添加功能**，搜索**ssh**,然后安装**OpenSSH服务器**。

安装完成后，以管理员身份启动cmd或powershell，输入

```powershell
net start sshd
```

接下来就可以通过另一台***客户端电脑***连接**服务端windows**了，但是默认是连接到**服务端**的cmd，

以管理员身份启动powshell，输入以下代码可以使SSH连接Windows时默认使用Powershell

```powershell
New-ItemProperty -Path "HKLM:\SOFTWARE\OpenSSH" -Name DefaultShell -Value "C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe" -PropertyType String -Force
```

### 3 windwos服务常用端口

> | 服务              | 协议  | 端口   |
> | --------------- | --- | ---- |
> | FTP             | TCP | 21   |
> | SSH(包括ssh和sftp) | TCP | 22   |
> | telnet          | TCP | 23   |
> | SMTP            | TCP | 25   |
> | http            | TCP | 80   |
> | P0P3            | TCP | 110  |
> | https           | TCP | 443  |
> | smb             | TCP | 445  |
> | mysql           | TCP | 3306 |
> | 远程桌面连接          | TCP | 3389 |
> | DNS             | UDP | 53   |
> | DHCP            | UDP | 68   |
> | TFTP(简单文件传输系统)  | UDP | 69   |
> | SNMP            | UDP | 161  |
> | NFS(网络文件系统)     | UDP | 2049 |
> |                 |     |      |

一般情况下，为了安全起见，运行商会封禁以下端口

80(http) 443(https) 445(samba) 25(SMTP) 465(SMTPS)



### 4 公网smb

#### 1 ipv6 + ddns-go + 开放netlogon

#### 2 ipv4 + ddns-go + 端口转发

###### 2.1方法1 在服务端与客户端分别进行端口转发（失败）

服务端(家里台式机)，

```powershell
netsh interface portproxy add v4tov4 listenport=希望被修改成的服务器端口 listenaddress=0.0.0.0 connectport=445 connectaddress=127.0.0.1
```

比如,将smb默认的*127.0.0.1:445*端口转发给*127.0.0.1:4455*端口

```powershell
netsh interface portproxy add v4tov4 listenport=4455 listenaddress=127.0.0.1 connectport=445 connectaddress=127.0.0.1
```

客户端(学校笔记本)

```powershell
netsh interface portproxy add v4tov4 listenport=445 listenaddress=127.0.0.1 connectport=服务器端口 connectaddress=服务器IP
```

比如，将*180.108.102.103:4455*转发给*127.0.0.2:445*

```powershell
netsh interface portproxy add v4tov4 listenport=445 listenaddress=127.0.0.2 connectport=4455 connectaddress=ipv6.chenxiaoijiajia.top
```

查看全部端口转发：

```powershell
netsh interface portproxy show all
```

删除端口转发：

```powershell
netsh interface portproxy delete v4tov4 listenaddress=欲删除项目的监听IP listenport=欲删除项目的监听端口
```

比如 删除 *127.0.0.2:445*

```powershell
netsh interface portproxy delete v4tov4 listenaddress=127.0.0.2 listenport=445
```

###### 方法2 路由器设置端口转发（成功）



### 5 字体

windows terminal  

| 工具                          | 字体                        | 字号  | 行高  | 第二字体            |     |
| --------------------------- | ------------------------- | --- | --- | --------------- | --- |
| windows terminal            | MesloLGSDZ Nerd Font Mono | 14  |     |                 |     |
|                             |                           |     |     |                 |     |
| tabby                       | MesloLGSDZ Nerd Font Mono | 18  | 2   | 微软雅黑            |     |
| idea 编辑器字体<br />即配色方案字体     | MiSans                    | 20  | 1.2 | Microsoft YaHei |     |
| idea 编辑器字体<br />即配色方案字体 方案2 | UbuntuMono Nerd Font Mono | 20  | 1.2 | Microsoft YaHei |     |
| ideaUI                      | Microsoft YaHei           | 16  |     |                 |     |
| idea 控制台                    | MesloLGSDZ Nerd Font Mono | 18  | 1.2 |                 |     |
|                             |                           |     |     |                 |     |



#### 术语

在下载某些字体时，可能会遇到相关术语，如下：

|                     |                                                    |
| ------------------- | -------------------------------------------------- |
| mono                | 即 Monospaced，等宽字体（一般指英文等宽）                         |
| Proportional        | 比例字体                                               |
| Serif               | 衬线体<br />在字的笔画开始、结束的地方有额外的装饰，而且笔画的粗细会有所不同。<br />毛边 |
| sans                | 即 Sans-Serif，无衬线体<br />没有这些额外的装饰，而且笔画的粗细差不多。       |
| gothic              | 哥特体，即无衬线体                                          |
| 上述术语都是针对英文字体的       | 对于中文梯子                                             |
| hinted              | 显示效果微调的字体                                          |
| unhinted            | 无微调字体                                              |
| VF (Variable fonts) | :可变字体，可以自由的调整字体的粗细                                 |
| slab                | 一般较为厚重，适用于强调性文本，标题等，不适用于长文本                        |

微调(hinting)分为两种：内嵌微调与自动微调。内嵌微调是字体设计者耗费大量的时间和精力、精心制作的、内嵌在字体文件中的额外指令，本质上是人工微调；而自动微调(autohint)是 FreeType 内嵌的一套微调算法，通用于所有矢量字体，本质上是机器微调。一般说来，内嵌微调比自动微调的显示效果更佳。

上述术语都是针对英文字体的，对于中文字体，习惯性称衬线字体为宋体，非衬线字体为黑体

| 字重（字体大小）   |                                 |
| ---------- | ------------------------------- |
| Thin       | 极细                              |
| Light      | 细体                              |
| Regular    | 常规                              |
| Medium     | 适中                              |
| Blod       | 粗体                              |
| Black      | 粗体                              |
| Heavy      | 特粗                              |
| italic     | 斜体                              |
| bolditalic | 加粗斜体                            |
| regular    | 常规体                             |
| sc         | 即 Simplified Chinese，简体中文       |
| tc         | 即 Traditional Chinese，繁体中文      |
| cl         | 即 Classical Literature，《康熙字典》字形 |
| ligature   | 连体字符，举例来说，会把 `!=` 变成 `≠`        |



#### 字体文件类型后缀：

1. `.otf` 是指基于 PostScript 开发的 OTF 格式 （windows 对其支持不佳，例如：word 无法将这类字体嵌入到 pdf 中）
2. `.ttf`包含两类字体：
   1. 一类是古老的 `TTF` 格式；
   2. 另一类是 基于 TTF 开发的 `OTF` 格式
      `.ttc (TrueType Collection)` 是 `ttf (TrueType Collection)` 的集合。

https://blog.csdn.net/weixin_39550940/article/details/111739766



Sarasa Gothic / 更纱黑体：基于Noto Sans，全宽引号 Quotes (“”) are full width —— Gothic

Sarasa UI / 更纱黑体 UI：基于Noto Sans窄引号 Quotes (“”) are narrow —— UI

Sarasa Mono T / 等距更纱黑体 T：基于Iosevka，有连字，全宽破折号 Have ligature, Em dashes (——) are full width —— MonoT

Sarasa Mono / 等距更纱黑体：基于Iosevka，有连字，半宽破折号 Have ligature, Em dashes (——) are half width —— Mono

Sarasa Term：基于Iosevka，无连字，半宽破折号 No ligature, Em dashes (——) are half width —— Term



适合代码 的   

mono 

sans 

Nerd fonts

unhinted

中英文2:1的字体

* 1 Dejavu Sans Mono

* 2 Sarasa mono SC(等距更纱黑体)
  
  * [Sarasa Term SC Nerd](https://github.com/laishulu/Sarasa-Term-SC-Nerd)
  * [Sarasa Mono SC Nerd 旧版](https://github.com/XuanXiaoming/Sarasa-Mono-SC-Nerd)
  * [Sarasa Mono SC Nerd & Sarasa Mono SC Wide Nerd 新版](https://github.com/seekdoor/Sarasa-Mono-SC-Nerd)

* 3 思源黑体HW

​    https://zhuanlan.zhihu.com/p/623573927



### 6 vscode

vscode 远程运行R

* 远程服务器
  * 1 R环境 和R包
    
    ```r
    install.packages("languageserver")
    install.packages("httpgd")
    ```
  * 2 python环境 和radian
    pipx install radian
  * 
* 本地电脑
  * 1  vscode   remote-SSH插件

### 7 winget

查询参数   winget install -q

winget install --id packageid --scope machine

```powershell
winget install --id packageid --scope machine

# --scope machine   ==> C:\Program Files\WinGet\Packages
#### zip格式 ####
## --scope machine 全局安装 
##  
## 用户安装
##  C:\Users\24970\AppData\Local\Microsoft\WinGet\Packages\sharkdp.fd_Microsoft.Winget.Source_8wekyb3d8bbwe\fd-v8.7.1-x86_64-pc-windows-msvc

# bat
winget install --id sharkdp.bat --scope machine
# fd-find
winget install sharkdp.fd
winget install --id sharkdp.fd --scope machine

# hexyl  16进制查看器
winget install --id sharkdp.hexyl --scope machine

# dust
winget install --id bootandy.dust --scope machine

# duf
winget install --id muesli.duf --scope machine

# zoxide
winget install --id ajeetdsouza.zoxide --scope machine
# fzf
winget install --id junegunn.fzf --scope machine

#### msi格式 ####
# bottom 默认安装在 C:\Program Files\bottom
winget install --id Clement.bottom
btm

#### exe格式 ####
winget install wingetui

### 失败
# procs 尚未进winget源

# lsd 只能用choco

```

### 8 出现ms-gamingoverlay问题

#### 1 实测 只需修改1

HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\CurrentVersion\GameDVR

找到其中AppCaptureEnabled这个键值，把值改为0。

#### 2

HKEY_CURRENT_USER\System\GameConfigStore

找打其中GameDVR_Enabled这个键值，把值改为0

### 9 系统文件夹与用户文件夹

#### 1 启动文件夹

##### 1.1 系统启动文件夹

```powershell
shell:CommonStartup
%programdata%\Microsoft\Windows\Start Menu\Programs\Startup
C:\ProgramData\Microsoft\Windows\Start Menu\Programs\StartUp
```

##### 1.2 用户启动文件夹

```powershell
shell:startup
%appdata%\Microsoft\Windows\Start Menu\Programs\Startup
C:\Users\用户名\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup
```

#### 2 [powershell 配置文件](https://learn.microsoft.com/zh-cn/powershell/module/microsoft.powershell.core/about/about_profiles?view=powershell-7.4)

##### 2.1当前用户，当前主机(用得较多)

```powershell
$PROFILE
$PROFILE.CurrentUserCurrentHost
$HOME\Documents\PowerShell\Microsoft.PowerShell_profile.ps1
C:\Users\用户名\Documents\PowerShell\Microsoft.PowerShell_profile.ps1
```

##### 2.2 当前用户，所有主机

```powershell
$PROFILE.CurrentUserAllHosts
$HOME\Documents\PowerShell\Profile.ps1
```

##### 2.3 所有用户，当前主机

```powershell
$PROFILE.AllUsersCurrentHost
$PSHOME\Microsoft.PowerShell_profile.ps1
```

##### 2.2 所有用户，所有主机

```pwsh
$PROFILE.AllUsersAllHosts
$PSHOME\Profile.ps1
```
