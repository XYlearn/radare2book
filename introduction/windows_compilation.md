## 在Windows下编译

### Meson \(MSVC\)

**警告: 这种构建方式暂时没有被测试过。注意它并没有编译所有radare2目前可获得的插件。按照以下的 **[**Mingw32**](https://legacy.gitbook.com/book/xylearn/radare2book-chinese/edit#) **构建以更加稳定完整地进行构建**

最本地化的在Windows下编译radare2的方法是使用meson + msvc。首先你需要在电脑上安装**python3**。完成后，你可以通过`pip3 install meson`安装meson构建系统（使用管理员权限）。然后前往你的Python安装路径，并从`.\Scripts`子目录将`meson.py`拷贝到你的radare2目录下。

Meson也需要Ninja的支持。你可以从[这里](https://ninja-build.org)下载它。将`ninja.exe`文件拷贝到你的radare2目录下。并且运行`meson.bat`并等待编译完成。

#### 编译

* 一开始你需要从Visual Studio或VisualC++构建工具目录下运行`vsvarsall.bat。`  
  在VS2015的环境下，目录通常为`C:\\Program Files (x86)\\Microsoft Visual Studio 14.0\\VC\\` 或者是  
   `C:\\Program Files (x86)\\Microsoft Visual Studio\2017\Community\VC\Auxiliary\Build\vcvarsall.bat`

* 使用 **Ninja build system**：这是最简单的方式，只需要运行`meson.bat`并等待编译完成

* 使用 **VisualStudio**：如果你想要在Vuisual Studio中构建radare2，你只需要简单地运行`meson.bat -p`。这会生成一个Visual Studio 2015工程。之后你就能从`build\radare2.sln`载入它。

* 使用 **MSBuild**：调用 `meson.bat --msbuild`

在任何情况下，除了在sdb文件生成过程中，所有文件都默认在`build`目录下生成。

#### 安装
=
既然你已经成功编译了radare2，你可能会想将所有可执行文件和依赖放置在同一个地方。下面的命令会收集这些并将其放在`dist`目录下。

```bat
E:\radare2>sys\meson_install.bat dist
```

你现在可以很容易地测试你新生成的radare2程序

```bat
E:\radare2>cd dist
E:\radare2\dist>radare2.exe -v
```

### Mingw32

使用Mingw32是一种在Windows上编译文件的简单方式。在radare首页获取的w32构建发行版是在一个GNU/Linux容器中用Mingw32生成的，并且在Wine中被测试过。也请记住，使用Mingw-w64构建没有被测试过，因此使用Mingw-w64构建不会有任何保证。

请确认配置你的Mingw32使用**thread model: win32**而不是**posix**进行编译，并且target应该设置为**mingw32**。在你开始编译前你需要安装git，以自动获取capstone。

```sh
git config --global core.autocrlf true
git config --global core.filemode false
```

下面是使用MinGW32进行编译的一个示例 \(此前需安装Windows版本的**zip**\)：

```sh
CC=i486-mingw32-gcc ./configure
make
make w32dist
zip -r w32-build.zip w32-build
```

这回生成一个本地的，32位Windows控制台应用程序。
我的容器中安装的是'i486-mingw32-gcc'编译器，你可能需要更改这一设置。

radare2的代码下有一个在Windows/Mingw32下简化构建的脚本：`sys/mingw32.bat`。只需要在cmd.exe（或ConEmu/cmd.exe）下运行它。
这个脚本假定你在`C:\Mingw`下安装了Mingw32，在`C:\Program Files (x86)\Git`下安装了Git。如果你使用了别的安装路径，只需要正确设置`MINGW_PATH` 和 `GIT_PATH`环境变量：

```
set MINGW_PATH=D:\Mingw32
set "GIT_PATH=E:\Program and Stuff\Git"
sys\mingw32.bat
```

请注意，上述脚本应该在radare2目录下被运行。

有个脚本可自动检测交叉编译工具链配置并构建一个包含可在Windows或Wine上被部署的r2程序和库的zip文件。

```sh
sys/mingw32.sh
```

### Cygwin

Cygwin 是另一个可行方案。但是与Cygwin 库有关的issues可能会是调试非常困难。但是使用Ctgwin编译的程序可以使你在Windows控制台下使用Unicode并可以显示256种颜色。

注意，Cygwin构建需要不同的git配置，因此先安装git以正确自动获取capstone。

```sh
git config --global core.autocrlf false
```

请确认在你即将使用r2的环境下构建radare2。如果你想在MinGW32命令行或者cmd.exe下使用r2，你应该在MinGW32环境下构建它；如果你想在Cygwin下使用r2，你需要在Cygwin命令行构建。因为Cygwin比MinGW对UNIX兼容性更好，所以在Cygwin下构建的radare2支持更多颜色和Unicode符号。

### Mingw-W64

* 从官方网站下载下载MSYS2：[http://msys2.github.io/](http://msys2.github.io/)
* 安装代理\(如果需要的话\)：
  ```sh
  export http_proxy=<myusername>:<mypassword>@zz-wwwproxy-90-v:8080
  export https_proxy=$http_proxy
  export ftp_proxy=$http_proxy
  export rsync_proxy=$http_proxy
  export rsync_proxy=$http_proxy
  export no_proxy="localhost,127.0.0.1,localaddress,.localdomain.com"
  ```
* 更新软件包:
  ```sh
  pacman --needed -Sy bash pacman pacman-mirrors msys2-runtime mingw-w64-x86_64-toolchain
  ```
* 关闭MSYS2，从开始菜单再次运行它并且升级剩余软件包：
  ```sh
  pacman -Su
  ```
* 安装构建工具：
  ```sh
  pacman -S git make zip gcc patch
  ```
* 编译radare2：

  ```sh
  ./configure --with-ostype=windows ; make ; make w32dist
  ```

  ### 构建附带程序(bindings)

你需要安装[Windows版Vala \(valac\)](https://wiki.gnome.org/Projects/Vala/ValaOnWindows)以构建radare2捆绑工具。

然后下载[valabind](https://github.com/radare/valabind) 并构建它：

```sh
git clone https://github.com/radare/valabind.git valabind
cd valabind
make
make install
```

在你安装valabind后，你可以用Python和Perl构建radare2附带程序：

```sh
git clone https://github.com/radare/radare2-bindings.git radare2-bindings
cd radare2-bindings
./configure --enable=python,perl
make
make install
```



