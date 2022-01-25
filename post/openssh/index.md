
> SSH(Secure Shell Protocol)

一种加密的网络传输协议，可在不安全的网络中为网络服务提供安全的传输环境。它通过在网络中建立安全隧道来实现 SSH 客户端与服务器之间的连接，最常见的用途是远程登录系统，通常被用于传输命令行界面和远程执行命令。

> OpenSSH(OpenBSD Secure Shell)

使用 SSH 通过计算机网络加密通信的 **实现**。它是取代由 SSH Communications Security 所提供的商用版本的 **开放源代码** 方案。目前 OpenSSH 是 OpenBSD 的子项目。

OpenSSH 常常被误认以为与 OpenSSL 有关系，但实际上这两个项目有不同的目的，不同的发展团队，名称相近只是因为两者有同样的软件发展目标──提供开放源代码的加密通信软件。

## 安装 OpenSSH

### Windows 操作系统

#### 通过包管理器 Scoop 安装

最简单的方法是使用 Scoop 进行安装，打开 PowerShell：

```Powershell
scoop install openssh
```

如果你没有安装 Scoop，可以参考我的另一篇文章 [Windows 下统一开发环境 Scoop 的安装与使用](/post/scoop)

#### 通过 Windows 提供的功能安装

操作系统版本：Windows 10

开始->设置->应用->应用和功能->可选功能->添加功能

搜索框输入 ssh ，找到并安装：

- OpenSSH 服务器
- OpenSSH 客户端

#### 检查安装

打开 PowerShell，输入：

```powershell
ssh
```

如果输出帮助信息说明安装成功

💡 **NOTE**: Windows 下若提示系统错误 5 ，请用管理员方式打开命令行

### Ubuntu 操作系统

## 使用 OpenSSH

全部命令请查询 [OpenSSH Manual](https://www.openssh.com/manual.html)。