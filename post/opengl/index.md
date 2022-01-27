
> OpenGL (Open Graphics Library)

A cross-language, cross-platform application programming interface (API) for rendering 2D and 3D vector graphics. The API is typically used to interact with a graphics processing unit (GPU), to achieve hardware-accelerated rendering.

"However, OpenGL by itself is not an API, but merely a **specification**, developed and maintained by the Khronos Group." -- from `learnopengl.com`

学习 OpenGL 最好的教程：
[LearnOpenGL](https://learnopengl.com/)
[LearnOpenGL-CN](https://learnopengl-cn.github.io)

## OpenGL 环境搭建

### Windows 操作系统搭建 VS Code/Visual Studio + OpenGL 环境

- 操作系统：Windows 10
- 系统架构：x64-based PC
- 工程文件生成工具：CMake
- OpenGL 应用框架：GLFW
- 访问 OpenGL 规范接口的第三方库：GLAD
- [Option 1]
  - 编译器：MinGW-W64
  - 编辑器：Visual Studio Code
- [Option 2]
  - 编译器：MSVC
  - 集成开发环境：Visual Studio 2022

💡 **NOTE**:

需要提前安装好 CMake，另外：

- 选择 [Option 1] 需要提前安装好 MinGW-W64 和 VS Code
- 选择 [Option 2] 需要提前安装好 Visual Studio

安装 MinGW-w64 与 CMake 可以参考上一篇博客 [CMake 的安装与使用](/post/cmake)

#### 构建 GLFW

> GLFW 提供创建 OpenGL Context 和用于显示的窗口、鼠标键盘等操作等接口，否则你需要自己写大量操作系统相关的代码来完成这些工作，因为在不同操作系统上这些操作不同，OpenGL 将它们抽象出去。

访问 [GLFW GitHub Releases 页面](https://github.com/glfw/glfw/releases)，选择最新版本 **源代码** 压缩包 `Source code (zip)` 下载，或者 [GLFW 官方下载页面](https://www.glfw.org/download.html) 点击 `Source package` 直接下载最新版本 **源代码** 压缩包 `glfw-<latest-version>.zip`。

💡 **NOTE**: 从源代码编译库可以保证生成的库是兼容你的操作系统和 CPU 的，而预编译的二进制文件可能会出现兼容问题（甚至有时候没提供支持你系统的文件）。

下载源码包之后，将其解压并打开。我们只需要里面的这些内容：

- 编译生成的库
- include 文件夹

0. 创建一个单独的文件夹，里面包含 Libs 和 Include 文件夹，在这里要存放 OpenGL 工程用到的所有第三方库和头文件。

##### [Option 1] MinGW-W64 + VS Code 解决方案

1. 在解压后的源代码目录新建一个 build 目录，打开 CMake-GUI：

   - Where is the source code: 选择解压后的源代码目录
   - Where to build the bianries: 选择刚刚创建的 build 目录

   然后点击 `Configure` 按钮，让 CMake 读取设置和源代码
2. 然后选择工程生成器：Generator 选择 `MinGW Makefile` 和 `Use default native compilers`，点击 `Finish` 开始 **配置**
3. 配置完成后 CMake 会显示可选的编译选项用来配置最终生成的库，勾选 BUILD_SHARED_LIBS，创建动态库
4. 点击 `Generate` 等待文件 **生成**
5. 打开 PowerShell 进入刚才创建的 build 目录，执行 `make` 命令等待 **编译** 完成

⚠️️ **NOTICE**：将 **build/src** 目录中的 **glfw3.dll**、**libglfw3dll.a** 添加到你的第三方库文件目录中，并将 **include/GLFW** 目录添加到你的第三方包含目录中

##### [Option 2] MSVC + Visual Studio 解决方案

1. 在解压后的源代码目录新建一个 build 目录，打开 CMake-GUI：

   - Where is the source code: 选择解压后的源代码目录
   - Where to build the bianries: 选择刚刚创建的 build 目录

   然后点击 `Configure` 按钮，让 CMake 读取设置和源代码
2. 然后选择工程生成器：Generator 选择你的 `Visual Studio <msvc-version> <visual-studio-version>` 和 `Use default native compilers`，点击 `Finish` 开始 **配置**
3. CMake 会显示可选的编译选项用来配置最终生成的库，这里我使用默认设置
4. 再次点击 `Configure` 按钮保存设置，然后点击 `Generate` 按钮，**生成** 的工程文件会在你的 build 文件夹中
5. 点击 `Open Project` 或者进入到刚才创建的 build 目录中，找到 `GLFW.sln` 文件打开，点击 `生成解决方案（Build Solution）` 开始 **编译**

编译的库 **glfw3.lib** 会出现在 **build/src/Debug** 目录内。

库生成完毕之后，我们需要让 IDE 知道库和头文件的位置。有两种方法：

- 找到 IDE 或者编译器的 /lib 和 /include 文件夹，添加 GLFW 的 include 文件夹里的文件到 IDE 的 /include 文件夹里去。用类似的方法，将 glfw3.lib 添加到 /lib 文件夹里去。虽然这样能工作，但这不是推荐的方式，因为这样会让你很难去管理库和 include 文件，而且重新安装 IDE 或编译器可能会导致这些文件丢失。
- 推荐的方式是建立一个新的目录包含所有的第三方库文件和头文件，并且在你的 IDE 或编译器中指定这些文件夹。我个人会使用一个单独的文件夹，里面包含 Libs 和 Include 文件夹，在这里存放 OpenGL 工程用到的所有第三方库和头文件。这样我的所有第三方库都在同一个位置（并且可以共享至多台电脑）。然而这要求你每次新建一个工程时都需要告诉 IDE / 编译器在哪能找到这些目录。

#### 配置 GLAD

GLAD 是一个开源库：[GitHub 项目地址](https://github.com/Dav1dde/glad)

> WHY GLAD?

因为 OpenGL 只是一个标准/规范，具体的实现是由驱动开发商针对特定显卡实现的。由于 OpenGL 驱动版本众多，它大多数函数的位置都无法在编译时确定下来，需要在 **运行时** 查询。所以任务就落在了开发者身上，开发者需要在运行时获取函数地址并将其保存在一个函数指针中供以后使用。取得地址的方法因平台而异。

GLAD 能解决这个繁琐的问题，下面是配置 GLAD 的步骤：

1. 访问 [GLAD在线服务](https://glad.dav1d.de/)，在这里我们能够告诉 GLAD 需要定义的 OpenGL 版本，并且根据这个版本加载所有相关的 OpenGL 函数，参考配置如下：

   - Language: C/C++
   - Specification: OpenGL
   - API:
     - gl: 版本建议选择 3.3 以上
   - Profile: Core（ 如果选择 Compatibility 将兼容 OpenGL 旧标准，文件也更大）
   - Extensions: 可以暂时忽略
   - Options: 选中 Generate a loader

   然后点击 `GENERATE` 按钮生成文件。
2. GLAD 现在提供了一个 zip 压缩文件，解压后文件夹包含一个 include 目录（其中有两个头文件目录）以及一个 src 目录（其中是一个 glad.c 文件）。将两个头文件目录 **glad** 和 **KHR** 复制到你的第三方包含文件夹中
   😶 如果你使用 **[Option 2] MSVC + Visual Studio** 搭建环境，**跳过** 下面的步骤
3. 在 glad 目录下将 src 文件夹中的 glad.c 编译为静态库：

    ```powershell
    gcc .\src\glad.c -c -I .\include\
    ar -rc libglad.a glad.o
    ```

    得到 glad.o 和 libglad.a 文件，下面的测试工程需要 **libglad.a**，将它复制到你的第三方库文件目录中

#### 测试工程

##### [Option 1] MinGW-W64 + VS Code 测试工程

1. 新建一个工程目录，在此目录下打开 VS Code，安装 C/C++ Project Generator 插件，`Ctrl + Shift + P` 搜索 `Create C++ Project` 创建一个 C++ 工程
2. `Ctrl + Shift + P` 搜索 `C/C++: Edit Configurations`，修改配置（c_cpp_properties.json）：

   - 编译器路径（compilerPath）：`<path-to-your-mingw-w64>/bin/g++.exe`
   - intelliSense 模式（intalliSenseMode）：linux-gcc-x64
   - 修改 c_cpp_properties.json 中的包含路径（includePath）：添加 `<path-to-your-3rd-party-include-directory>`

    🦘 详细的的配置可以参考我的 GitHub 仓库中 [OpenGL-Demo/Test_VSC/.vscode](https://github.com/exitance/OpenGL-Demo/Test_VSC/.vscode) 目录中的内容
3. 按 Test 目录中的配置修改 Makefile 文件，添加你的第三方包含目录和库目录路径至 INCLUDE 和 LIB，并修改 LIBS 添加链接静态库选项 `-lglad` 和 `-lglfw3dll`，分别对应于上面生成的静态库文件 libglad.a 和 libglfw3dll.a，Makefile 中链接库文件需要“掐头去尾”——去掉开头的 lib 和尾部的扩展名 .a
    🐧 详细的的配置可以参考我的 GitHub 仓库中 [OpenGL-Demo/Test_VSC/Makefile](https://github.com/exitance/OpenGL-Demo/Test_VSC/Makefile) 文件中的内容
4. 把 glfw3.dll 复制到此工程的根目录中
5. 修改源文件 main.cpp，添加你的 OpenGL 渲染代码
    🐻 测试代码可以参考我的 GitHub 仓库中 [OpenGL-Demo/Test_VSC/src/main.cpp](https://github.com/exitance/OpenGL-Demo/Test_VSC/src/main.cpp) 文件中的内容
6. 在 VS Code 中工程目录路径下打开 PowerShell，输入 `make run` 可以看到运行结果

##### [Option 2] MSVC + Visual Studio 测试工程

1. 打开 Visual Studio，创建一个新的项目 Test，选择：
   - 语言： C++
   - 项目模板：空项目（Empty Project）
2. 在解决方案（Solution Explorer）窗口里点击工程名 **`Test`**，右键选择 `属性（Properties）`，在弹出的窗口选择 `配置属性（Configuration Properties）` 选项卡
3. 选择 `VC++ 目录（VC++ Directories）` 选项卡，在 `常规（General）` 中的 `包含目录（Include Directories）` 和 `库目录（Library Directories）` 字段中加入你的第三方包含目录和库目录路径 💡 **NOTE**: 记得在每个路径末尾带上 `;`
4. 选择 `链接器（Linker）` 选项卡里的 `输入（Input）` 选项卡，在 `附加依赖项` 字段中最后一个字符串 `%(AdditionalDependencies)` 的前面添加字符串 `glfw3.lib;`
5. 注意 `Configuration` 要选择 `所有配置（All Configurations）`，`Platform` 要和你主菜单选择的 `Solution Platform` 保持一致，然后点击 `确定（confirm）` 保存更新
6. 将 glad.c 复制到你的工程目录下，解决方案中选择 `源文件（Source Files）` 右键选择 `Add` -> `Existing Item` 选择 glad.c 添加到工程中
7. 解决方案中选择 `源文件（Source Files）` 右键选择 `Add` -> `New Item` 添加文件 main.cpp，添加你的 OpenGL 渲染代码
   🐼 测试代码可以参考我的 GitHub 仓库中 [OpenGL-Demo/Test_VS/Test_VS/main.cpp](https://github.com/exitance/OpenGL-Demo/Test_VS/Test_VS/main.cpp) 文件中的内容
8. 现在可以在 Visual Studio 中调试运行了

> 可能遇到的问题：cpp 源文件包含 `GLFW/glfw3.h` 头文件时提示找不到 `stddef.h` 和 `GL/gl.h` 等头文件
> > 可能是 SDK 版本的问题。
> > **Slove**：
> > 打开 Visual Studio Installer，选择你使用的 Visual Studio 版本，点击 `修改` -> `单个组件`，找到 `Visual Studio SDK` 和相应的 `Windows <windows-version> SDK <kernel-version>` 安装

### Ubuntu 操作系统

## OpenGL 使用
