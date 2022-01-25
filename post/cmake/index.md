
> CMake

An **open-source**, **cross-platform** family of tools designed to build, test and package software.
CMake is used to control the software compilation process using simple platform and compiler independent configuration files, and **generate native makefiles** and workspaces that can be used in the compiler environment of your choice. The suite of CMake tools were created by Kitware in response to the need for a powerful, cross-platform build environment for open-source projects such as ITK and VTK.

CMake is part of Kitware’s collection of commercially supported open-source platforms for software development.

一个跨平台的安装（编译）工具，可以用简单的语句来描述所有平台的安装（编译过程）。

CMake 允许开发者编写一种平台无关的 CMakeLists.txt 文件来定制整个编译流程，然后再根据目标用户的平台进一步生成所需的本地化 Makefile 和工程文件，如 Unix 的 Makefile 或 Windows 的 Visual Studio 工程。从而做到 "Write once, run everywhere" 。

[CMake 入门实战](https://www.hahack.com/codes/cmake/)

## 安装 CMake

### Windows 操作系统安装 CMake

- 操作系统：Windows 10
- 系统架构：x64-based PC

x64 是 x86 架构的 64 位拓展——x86-64: 64-bit extended。

打开 PowerShell，使用命令：

```powershell
systeminfo
```

可以查看操作系统与系统架构等信息。

#### 预先安装 MinGW-W64

如果你已经安装 MinGW-W64，可以跳过下面的步骤。

> MinGW：Minimalist GNU on Windows
> MinGW-w64 与 MinGW 的区别： MinGW 只能编译生成 32 位可执行程序，而 MinGW-W64 则可以编译生成 64 位或 32 位可执行程序。

1. 下载：

    [SourceForge: MinGW-W64 下载地址](https://sourceforge.net/projects/mingw-w64/)

    💡 **NOTE**: 更新到 v9.0.0 后此通过此地址下载的是 `mingw-w64-v9.0.0.zip` ，需要自行进行构建，现成的可以在 [winlibs](https://winlibs.com/) 找到，往下翻到 `Download-Release versions`，选择合适的版本下载后解压得到文件夹，移动到安装位置即可，然后直接转到 **步骤 3** 👉

2. 运行下载器：

   - Version: gcc 版本。一般选择最高版本
   - Architecture: 计算机架构。64 位系统选择 x86_64 ，32 位系统选择 i686
   - Threads: 操作系统接口协议。开发 Windows 程序选择 win32，开发其他操作系统程序选择 posix
   - Exception: 异常处理模型。seh 比 sjlj 的性能好，但不支持 32 位， sjlj 稳定性好，支持 32 位。64 位操作系统建议选择 seh
   - 选择自定义安装路径

   之后一路 Next 即可，等待安装完成。

3. 配置环境变量：

    高级系统设置–>环境变量–>系统变量->编辑 `Path` 变量->添加 MinGW-W64 可执行文件所在目录 `<your-path-to-mingw-w64>/bin`

    ⚠️️ **NOTICE**：修改后需要点击所有的确定。

4. 验证安装：

    打开 PowerShell，使用命令：

    ```powershell
    gcc -v
    ```

    查看版本信息，若显示 MinGW-W64 的组件列表以及版本信息说明安装成功。

#### 开始安装 CMake

1. 下载

    访问 [CMake 官网下载地址](https://cmake.org/download/)，下载与你的计算机 Platform （操作系统、系统架构）对应版本的 **预先编译的二进制文件（Binary distributions）** 的（其中之一即可）:

    - **安装器（Installer）**：`cmake-<version>-<operating-system>-<cpu-architechture>.msi`
    - **压缩包（ZIP）**：`cmake-<version>-<operating-system>-<cpu-architechture>.zip`

    **如果你下载的是 Installer**，转 **步骤 2** 👉
    **如果你下载的是 ZIP**，将解压得到的文件夹移动到（如果有历史版本，请覆盖）你的 CMake 安装目录，然后转 **步骤 3** 👉

2. 运行下载器

   - 是否添加环境变量根据自己需要选择，建议勾选添加，否则后面仍然要自行添加环境变量。这里我选择 "Add CMake to the system PATH for the current user"；
   - 是否创建桌面快捷图标根据自己需要选择；
   - 自定义安装路径。

   之后一路 Next 即可，等待安装完成。

   如果前面勾选 **不** 添加环境变量的选项，转 **步骤 3**
   否则，转 **步骤 4**

3. 配置环境变量

    高级系统设置–>环境变量–>系统变量->编辑 `Path` 变量->添加 CMake 可执行文件所在目录 `<your-path-to-cmake>/bin`

    ⚠️️ **NOTICE**：修改后需要点击所有的确定。

4. 验证安装

    打开 powershell ，使用命令：

    ```powershell
    cmake
    ```

    若显示 Usage 等信息说明安装成功。

#### 测试工程

新建工程文件夹 Test，创建 C 源代码文件 hello.c 与 CMakeLists.txt （CMake 用于生成 makefile 的文件），具体代码请参考博主的 GitHub 仓库 [CMake-Demo](https://github.com/exitance/CMake-Demo) 中的 Test 文件夹。

在刚刚创建的工程目录下打开 PowerShell ，创建并进入 build 目录，使用 `cmake` 与 `make` 命令生成可执行文件，并测试运行。详细命令如下：

```powershell
mkdir build
cd build
cmake -G"Unix Makefiles" ..
make
./Hello
```

### Ubuntu 操作系统安装 CMake

## CMake 与 MinGW-w64 使用问题

### Windows 操作系统使用 `cmake ..` 命令非预期编译器

在 Windows 系统上，如果直接使用 `cmake ..` 命令，（如果你安装了）很有可能选择到了 Visual Studio 的编译器 MSVC* ，输出类似如下内容：

```powershell
-- Building for: Visual Studio <version>
-- Selecting Windows SDK version <version> to target Windows <version>.
-- The C compiler identification is MSVC <version>
-- The CXX compiler identification is MSVC <version>
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Check for working C compiler: <your-path-to-visual-studio>/<version>/Community/VC/Tools/MSVC/<version>/bin/Hostx64/x64/cl.exe - skipped
-- Detecting C compile features
-- Detecting C compile features - done
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Check for working CXX compiler: <your-path-to-visual-studio>/<version>/Community/VC/Tools/MSVC/<version>/bin/Hostx64/x64/cl.exe - skipped
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Configuring done
-- Generating done
-- Build files have been written to: <your-path-to-project>/build
```

可以看到使用的编译器是 MSVC

**Slove**: 改用命令 `cmake -G"Unix Makefiles" ..` 指定编译器为 GNU

### Windows 操作系统使用 `cmake` 命令或者 `make` 命令出现错误信息

原因是缺少 make 程序。

**Slove**:

如果没安装 MinGW-W64，安装上面的步骤进行安装，检查是否添加环境变量，否则选择下面一种解决方法：

1. 在 `<your-path-to-mingw-w64>/bin` 找到 mingw32-make.exe 拷贝一份并重命名为 make.exe（建议使用）
2. make 命令改为 mingw32-make（`cmake` 命令的问题无法解决）

## CMake 使用

Windows 系统上 `cmake --help` 命令的输出（定时更新）：

```powershell
Usage

  cmake [options] <path-to-source>
  cmake [options] <path-to-existing-build>
  cmake [options] -S <path-to-source> -B <path-to-build>

Specify a source directory to (re-)generate a build system for it in the
current working directory.  Specify an existing build directory to
re-generate its build system.

Options
  -S <path-to-source>          = Explicitly specify a source directory.
  -B <path-to-build>           = Explicitly specify a build directory.
  -C <initial-cache>           = Pre-load a script to populate the cache.
  -D <var>[:<type>]=<value>    = Create or update a cmake cache entry.
  -U <globbing_expr>           = Remove matching entries from CMake cache.
  -G <generator-name>          = Specify a build system generator.
  -T <toolset-name>            = Specify toolset name if supported by
                                 generator.
  -A <platform-name>           = Specify platform name if supported by
                                 generator.
  --toolchain <file>           = Specify toolchain file
                                 [CMAKE_TOOLCHAIN_FILE].
  --install-prefix <directory> = Specify install directory
                                 [CMAKE_INSTALL_PREFIX].
  -Wdev                        = Enable developer warnings.
  -Wno-dev                     = Suppress developer warnings.
  -Werror=dev                  = Make developer warnings errors.
  -Wno-error=dev               = Make developer warnings not errors.
  -Wdeprecated                 = Enable deprecation warnings.
  -Wno-deprecated              = Suppress deprecation warnings.
  -Werror=deprecated           = Make deprecated macro and function warnings
                                 errors.
  -Wno-error=deprecated        = Make deprecated macro and function warnings
                                 not errors.
  --preset <preset>,--preset=<preset>
                               = Specify a configure preset.
  --list-presets               = List available presets.
  -E                           = CMake command mode.
  -L[A][H]                     = List non-advanced cached variables.
  --build <dir>                = Build a CMake-generated project binary tree.
  --install <dir>              = Install a CMake-generated project binary
                                 tree.
  --open <dir>                 = Open generated project in the associated
                                 application.
  -N                           = View mode only.
  -P <file>                    = Process script mode.
  --find-package               = Legacy pkg-config like mode.  Do not use.
  --graphviz=[file]            = Generate graphviz of dependencies, see
                                 CMakeGraphVizOptions.cmake for more.
  --system-information [file]  = Dump information about this system.
  --log-level=<ERROR|WARNING|NOTICE|STATUS|VERBOSE|DEBUG|TRACE>
                               = Set the verbosity of messages from CMake
                                 files.  --loglevel is also accepted for
                                 backward compatibility reasons.
  --log-context                = Prepend log messages with context, if given
  --debug-trycompile           = Do not delete the try_compile build tree.
                                 Only useful on one try_compile at a time.
  --debug-output               = Put cmake in a debug mode.
  --debug-find                 = Put cmake find in a debug mode.
  --trace                      = Put cmake in trace mode.
  --trace-expand               = Put cmake in trace mode with variable
                                 expansion.
  --trace-format=<human|json-v1>
                               = Set the output format of the trace.
  --trace-source=<file>        = Trace only this CMake file/module.  Multiple
                                 options allowed.
  --trace-redirect=<file>      = Redirect trace output to a file instead of
                                 stderr.
  --warn-uninitialized         = Warn about uninitialized values.
  --no-warn-unused-cli         = Don't warn about command line options.
  --check-system-vars          = Find problems with variable usage in system
                                 files.
  --profiling-format=<fmt>     = Output data for profiling CMake scripts.
                                 Supported formats: google-trace
  --profiling-output=<file>    = Select an output path for the profiling data
                                 enabled through --profiling-format.
  --help,-help,-usage,-h,-H,/? = Print usage information and exit.
  --version,-version,/V [<f>]  = Print version number and exit.
  --help-full [<f>]            = Print all help manuals and exit.
  --help-manual <man> [<f>]    = Print one help manual and exit.
  --help-manual-list [<f>]     = List help manuals available and exit.
  --help-command <cmd> [<f>]   = Print help for one command and exit.
  --help-command-list [<f>]    = List commands with help available and exit.
  --help-commands [<f>]        = Print cmake-commands manual and exit.
  --help-module <mod> [<f>]    = Print help for one module and exit.
  --help-module-list [<f>]     = List modules with help available and exit.
  --help-modules [<f>]         = Print cmake-modules manual and exit.
  --help-policy <cmp> [<f>]    = Print help for one policy and exit.
  --help-policy-list [<f>]     = List policies with help available and exit.
  --help-policies [<f>]        = Print cmake-policies manual and exit.
  --help-property <prop> [<f>] = Print help for one property and exit.
  --help-property-list [<f>]   = List properties with help available and
                                 exit.
  --help-properties [<f>]      = Print cmake-properties manual and exit.
  --help-variable var [<f>]    = Print help for one variable and exit.
  --help-variable-list [<f>]   = List variables with help available and exit.
  --help-variables [<f>]       = Print cmake-variables manual and exit.

Generators

The following generators are available on this platform (* marks default):
* Visual Studio 17 2022        = Generates Visual Studio 2022 project files.
                                 Use -A option to specify architecture.
  Visual Studio 16 2019        = Generates Visual Studio 2019 project files.
                                 Use -A option to specify architecture.
  Visual Studio 15 2017 [arch] = Generates Visual Studio 2017 project files.
                                 Optional [arch] can be "Win64" or "ARM".
  Visual Studio 14 2015 [arch] = Generates Visual Studio 2015 project files.
                                 Optional [arch] can be "Win64" or "ARM".
  Visual Studio 12 2013 [arch] = Generates Visual Studio 2013 project files.
                                 Optional [arch] can be "Win64" or "ARM".
  Visual Studio 11 2012 [arch] = Generates Visual Studio 2012 project files.
                                 Optional [arch] can be "Win64" or "ARM".
  Visual Studio 10 2010 [arch] = Deprecated.  Generates Visual Studio 2010
                                 project files.  Optional [arch] can be
                                 "Win64" or "IA64".
  Visual Studio 9 2008 [arch]  = Generates Visual Studio 2008 project files.
                                 Optional [arch] can be "Win64" or "IA64".
  Borland Makefiles            = Generates Borland makefiles.
  NMake Makefiles              = Generates NMake makefiles.
  NMake Makefiles JOM          = Generates JOM makefiles.
  MSYS Makefiles               = Generates MSYS makefiles.
  MinGW Makefiles              = Generates a make file for use with
                                 mingw32-make.
  Green Hills MULTI            = Generates Green Hills MULTI files
                                 (experimental, work-in-progress).
  Unix Makefiles               = Generates standard UNIX makefiles.
  Ninja                        = Generates build.ninja files.
  Ninja Multi-Config           = Generates build-<Config>.ninja files.
  Watcom WMake                 = Generates Watcom WMake makefiles.
  CodeBlocks - MinGW Makefiles = Generates CodeBlocks project files.
  CodeBlocks - NMake Makefiles = Generates CodeBlocks project files.
  CodeBlocks - NMake Makefiles JOM
                               = Generates CodeBlocks project files.
  CodeBlocks - Ninja           = Generates CodeBlocks project files.
  CodeBlocks - Unix Makefiles  = Generates CodeBlocks project files.
  CodeLite - MinGW Makefiles   = Generates CodeLite project files.
  CodeLite - NMake Makefiles   = Generates CodeLite project files.
  CodeLite - Ninja             = Generates CodeLite project files.
  CodeLite - Unix Makefiles    = Generates CodeLite project files.
  Eclipse CDT4 - NMake Makefiles
                               = Generates Eclipse CDT 4.0 project files.
  Eclipse CDT4 - MinGW Makefiles
                               = Generates Eclipse CDT 4.0 project files.
  Eclipse CDT4 - Ninja         = Generates Eclipse CDT 4.0 project files.
  Eclipse CDT4 - Unix Makefiles= Generates Eclipse CDT 4.0 project files.
  Kate - MinGW Makefiles       = Generates Kate project files.
  Kate - NMake Makefiles       = Generates Kate project files.
  Kate - Ninja                 = Generates Kate project files.
  Kate - Unix Makefiles        = Generates Kate project files.
  Sublime Text 2 - MinGW Makefiles
                               = Generates Sublime Text 2 project files.
  Sublime Text 2 - NMake Makefiles
                               = Generates Sublime Text 2 project files.
  Sublime Text 2 - Ninja       = Generates Sublime Text 2 project files.
  Sublime Text 2 - Unix Makefiles
                               = Generates Sublime Text 2 project files.


```

Ubuntu 系统上 `cmake --help` 命令的输出（定时更新）：
