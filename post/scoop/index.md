
> 包 package

或称“软件包”，可以理解为广义上的软件，通常指的是一个应用程序，它可以是常见的基于图形用户界面（ GUI ）的软件、基于命令行界面（ CLI ）的开发工具或（其他软件程序需要的）软件库。

包本质上是一个存档文件，包含二进制可执行文件、配置文件，有时还包含依赖关系的信息。

> 包管理器 package manager

开发人员常用的生产力工具，如 Ubuntu 上的 Apt-Get 和 MacOS 上的 Homebrew。

简单说，包管理器就是一个 *软件自动化管理工具* ，允许用户在操作系统上安装、删除、升级、配置和管理软件包。软件包管理器可以是像“软件中心”这样的图形化应用，也可以是像 apt-get 或 pacman 这样的命令行工具。。

Windows 下常用的包管理工具有：

- Scoop
- Chocolatey

## WHY Scoop ?

Scoop 由澳洲程序员 Luke Sampson 于 2015 年创建，其特色之一就是其安装管理不依赖“管理员权限”，这对使用有权限限制的公共计算机的使用者是一大利好。

Chocolatey 整个社区发布的安装脚本有3000多个，而 Scoop 官方仓库发布的安装脚本有2000多个。脚本数量上不如 Chocolatey，但是 Scoop *自定义程度高*，*扩展性强*，可以非常方便的自己定制安装脚本。最关键的是，Scoop 的*维护*完胜前者，前者貌似脚本很多，但其中不少已经没人维护或者不再更新了。

Mike Zick 在 [对 Cygwin 和 MSYS 的描述](https://sourceforge.net/p/mingw/mailman/mingw-msys/thread/200506130821.11185.mszick@morethan.org/) 中提到他认为 Scoop 并不是一个包管理器，而是通过读取 JSON 描述文件来安装程序及其依赖。

Scoop 专注于开源和命令行开发工具，不符合其标准的不可能进入 main bucket（Scoop 安装后便自带的），因而虽然通过 scoop install skype 也能安装 Skype，但是只能放在 extra bucket 中。

使用 Scoop 安装的应用程序通常称为“便携式”用程序，需要的权限更少，对系统产生的副作用也更少。Scoop 将软件直接安装到我们的用户目录下，安装过程不需要申请管理员权限（UAC）也不会污染系统环境变量，相比包管理器和应用仓库更简单。使用 Scoop 最简单的形式只需 Git + JSON。

⚠️️ **NOTICE**：对于像 VirtualBox、Docker for Windows，输入法等这些需要高权限的软件仍然要通过在官网下载安装包进行安装。

## Scoop 安装与卸载

### Scoop 安装

参考[官方教程](https://scoop.sh/)

**环境要求**：

- PowerShell 5 or later, include PowerShell Core（版本信息可在 PowerShell 中使用命令 $psversiontable 查看 PSVersion）
- NET Framework 4.5 or later

**安装步骤**：

打开 PowerShell

1. 打开远程权限

    ```powershell
    Set-ExecutionPolicy RemoteSigned -scope CurrentUser;
    ```

    出现提示`是否要更改执行策略?`，输入 Y 回车

2. 自定义 Scoop 安装目录

    ```powershell
    $env:SCOOP='Your_Scoop_Path'
    [Environment]::SetEnvironmentVariable('SCOOP', $env:SCOOP, 'User')
    ```

    💡 **NOTE**: 如果跳过该步骤，Scoop 将默认把所有用户安装的 App 和 Scoop 本身置于 `C:\Users\<user_name>\scoop`

3. Scoop 下载安装与更新

    使用官网给出的命令

    ```powershell
    iwr -useb get.scoop.sh | iex
    scoop update
    ```

    或者使用国内镜像加速：

    ```powershell
    iwr -useb https://gitee.com/glsnames/scoop-installer/raw/master/bin/install.ps1 | iex
    scoop config SCOOP_REPO 'https://gitee.com/glsnames/Scoop-Core'
    scoop update
    ```

4. 检查安装

    通过 scoop 或 scoop help 命令查看使用简介，若输出帮助信息，说明安装成功

    ```powershell
    Usage: scoop [<COMMAND>] [<OPTIONS>]

    Windows command line installer

    General exit codes:
    0 - Everything OK
    1 - No parameter provided or usage shown
    2 - Argument parsing error
    3 - General execution error
    4 - Permission/Privileges related issue
    10+ - Number of failed actions (installations, updates, ...)

    Type 'scoop help <COMMAND>' to get help for a specific command.

    Available commands are:

    alias       Manage scoop aliases.
    bucket      Manage local scoop buckets.
    cache       Show or clear the download cache.
    cat         Show content of specified manifest(s).
    checkup     Check system for pontential problems.
    cleanup     Perform cleanup on specified installed application(s) by removing old/not actively used versions.
    config      Get or set configuration values into scoop configuration file.
    depends     List dependencies for an application.
    download    Download manifest files into cache folder.
    export      Export (an importable) list of installed applications.
    help        Show help for specific scoop command or scoop itself.
    hold        Hold an application(s) to disable updates.
    home        Open the application's homepage in default browser.
    info        Display information about an application.
    install     Install specific application(s).
    list        List installed applications.
    prefix      Return the location/path of installed application.
    reset       Force binaries/shims, shortcuts, environment variables and persisted data to be re-linked.
    search      Search for applications, which are available for installation.
    status      Show status and check for available updates for all installed applications.
    unhold      Unhold an installed application(s) to enable updates.
    uninstall   Uninstall specified application(s).
    update      Update installed application(s), or scoop itself.
    utils       Wrapper around utilities for maintaining buckets and manifests.
    virustotal  Search virustotal database for potential viruses in files provided in manifest(s).
    which       Locate the path to a shim/executable of application that was installed with scoop (similar to 'which' on Li
                nux).
    ```

### Scoop 卸载

```powershell
scoop uninstall scoop
```

## Scoop 包管理

### 软件仓库 bucket （安装 GUI 程序等）

Scoop 默认软件仓库（main bucket）软件数量是有限的。同时由于 Scoop 的设计初衷是为了方便 Windows 开发者安装和配置 *开发工具*，其默认软件仓库的收录条件也就很苛刻

It MUST be:

- 主流的开发者工具
- 维护中的最新版本
- 完整版本（非 Trial 版本）

It CANNNOT be:

- 有复杂的安装前与安装后处理步骤
- 有 GUI

这个最后一点恐怕是最严格的条件了，直接否决了一系列用户向软件（Firefox、Typora 等等）的收录。

但是我们一样可以安装我们常用的图形界面软件，这就涉及到接下来我要介绍的，与 Homebrew cask 的设计理念类似的 Scoop bucket。

> bucket

在 Scoop 中，bucket 就是一个软件仓库。

Scoop 将仓库缓存至本地，当我们想要安装一个软件的时候，Scoop 就从本地的仓库中挑选出我们想要安装的软件的 **安装配置文件**，并依照这个配置文件进行软件的安装工作，包括：

- 软件源（从哪里下载软件）
- 安装步骤（如何安装软件）
- 安装路径（安装到哪里）
- 环境配置（需要修改更新什么环境变量）
- 安装之前（之后）要做什么准备（善后）工作
    ……

这就是 Scoop 安装软件的一个大体过程。

比如我们希望安装 aria2 这个软件，可以使用 info 命令查看它的详细配置

使用命令

```powershell
scoop info aria2
```

输出：

```powershell
Name: aria2
Version: 1.36.0-1
Description: Lightweight multi-protocol & multi-source command-line download utility
Website: https://aria2.github.io/
License: GPL-2.0-or-later (https://spdx.org/licenses/GPL-2.0-or-later.html)
Manifest:
  <Your-path-to-Scoop>\buckets\main\bucket\aria2.json
Installed:
  <Your-path-to-Scoop>\apps\aria2\1.36.0-1
Binaries:
 aria2c.exe

```

安装 Scoop 后默认添加了主仓库（main bucket）软件安装配置文件的集合。可以在 GitHub 上查看 [Main bucket 的软件列表](https://github.com/ScoopInstaller/Main/tree/master/bucket) 。

可以通过添加 bucket 来添加其他软件仓库，从而下载我们想要的软件的安装配置文件，来安装不在默认仓库中的软件。

Scoop 给我们提供了很多可以直接使用的 bucket，就是为了方便我们安装更为常见的带有 GUI 的软件。一个最为常见的 bucket 是 extras，这里面基本涵盖了大部分不符合 main bucket 收录条件的常用软件。

```powershell
scoop bucket add extras
scoop update
```

#### 官方维护的 bucket

查看 Scoop *直接识别* 的 bucket 列表：

```powershell
scoop bucket known
```

添加相应的 bucket：

```powershell
scoop bucket add <bucket-name>
```

与开发环境安装没有太大关系的仓库：

- extras：Scoop 官方维护的一个仓库，涵盖了大部分因为种种原因不能被收录进主仓库的常用软件
    [Bucket 地址](https://github.com/ScoopInstaller/Extras)
- nirsoft：一个 NirSoft 开发的小工具的安装合集。NirSoft 制作了大量的小工具，包括系统工具、网络工具、密码恢复等
    [Bucket 地址](https://github.com/kodybrown/scoop-nirsoft)
    [NirSoft 官网地址](http://www.nirsoft.net/)
- games：游戏与游戏相关的工具合集。包含了大量免费/开源的小游戏
    [Bucket 地址](https://github.com/Calinou/scoop-games)

其余 bucket 都是和开发环境相关的，比如 java 这个 bucket 就是为了安装 JDK 用的 bucket

#### 社区提供的 bucket

将社区维护的 bucket 添加至本机的 Scoop bucket 列表：

```powershell
scoop bucket add <bucket-name> <bucket-origin-url>
```

添加仓库后可以通过如下命令安装在对应仓库中的 App ：

```powershell
scoop install <bucket-name>/<app-name>
```

#### 进阶操作

学习以下内容前你需要了解：

- Git 与 GitHub
- JSON

> 这些 bucket 中都找不到我想要安装的软件？

可以自己维护一个 bucket，加入所需软件的安装步骤配置文件（.json）

**在 GitHub 上托管自定义 bucket**：

步骤参考 [官方文档中的 Demo](https://github.com/ScoopInstaller/Scoop/wiki/Buckets)

1. 新建一个 GitHub 仓库名为 my-bucket
2. 向 my-bucket 中添加一个名字叫 hello 的 App：

    ```bash
    git clone https://github.com/<your-username>/my-bucket
    cd my-bucket
    '{ version: "1.0", url: "https://gist.github.com/lukesampson/6446238/raw/hello.ps1", bin: "hello.ps1" }' > hello.json
    git add .
    git commit -m "add hello app"
    git push
    ```

3. 在 Scoop 中将 my-bucket 添加至你的 Scoop bucket 列表：

   ```powershell
   scoop bucket add my-bucket https://github.com/<your-username>/my-bucket
   ```

4. 测试是否正常工作：

    ```powershell
    scoop bucket list # -> you should see 'my-bucket'
    scoop saerch hello # -> you should see hello listed under, 'my-bucket bucket:'
    scoop install hello
    hello # -> you should see 'Hello, <windows-username>!'
    ```

继续对 hello.json 进行配置，包括下载地址、安装步骤等一系列配置方法请参考 [App Manifests](https://github.com/ScoopInstaller/Scoop/wiki/App-Manifests)

### 安装 App

```powershell
# 使用 scoop search 命令在本地 bucket 搜索 App 的具体名称
scoop search <incomplete-app-name>
scoop install <app-name>
```

## 实用仓库与工具推荐

### scoop-completion：命令补全

**安装与启用**：

```powershell
# add extras bucket if you haven't
scoop bucket add extras

# install
scoop install scoop-completion
```

在 **当前** 终端中启用 scoop-completion

```powershell
# enable completion in current shell, use absolute path because PowerShell Core not respect $env:PSModulePath
Import-Module "$($(Get-Item $(Get-Command scoop.ps1).Path).Directory.Parent.FullName)\modules\scoop-completion"
```

若需要每次打开终端自动启动 scoop-completion，需要修改用户配置文件 $profile

💡 **NOTE**：如果需要为所有用户/主机启用 scoop-completion，请参考[微软 $PROFILE 官方文档](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_profiles?view=powershell-7.2&viewFallbackFrom=powershell-6#the-profile-variable)

```powershell
# create profile if not exist
if (!(Test-Path $profile)) { New-Item -Path $profile -ItemType "file" -Force }

# print $profile path
$profile
```

用文本编辑器打开输出的 $profile 路径，将上面启用 scoop-completion 的代码复制到该文件中，重启 PowerShell 即可

**用法**：

```powershell
scoop ins[Press Tab]
scoop install py[Press Ctrl+Space]
scoop uninstall [Press Ctrl+Space]
```

**卸载**：

```powershell
scoop uninstall scoop-completion
```

⚠️️ **NOTICE**：如果前面设置了自动启动，卸载后需要将 $profile 中的启用代码删除

### Aria2：加速下载

一款自由、跨平台命令行界面的下载管理器，该软件根据GPLv2许可证进行分发。
支持的下载协议有：HTTP、HTTPS、FTP、Bittorrent和Metalink。

**安装**：

```powershell
scoop install aria2
```

使 Aria2 在 Scoop 中默认开启

```powershell
# 
scoop config aria2-enabled true
```

若使用代理，有时需要通过如下命令关闭 aria2

```powershell
scoop config aria2-enabled false
```

其他参数可以参考 [Aria2 官网](https://aria2.github.io/) 的文档

### gow：Linux 命令行

```powershell
scoop install gow
```

### sudo：调用管理员权限

```powershell
scoop install sudo
```

### cmder：Windows 终端模拟器

```powershell
scoop install cmder
```

### versions：安装特定版本与版本切换

```powershell
scoop bucket add versions
```
