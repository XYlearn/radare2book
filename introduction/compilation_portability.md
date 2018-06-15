## 清理老版本清理编译和移植

Currently the core of radare2 can be compiled on many systems and architectures, but the main development is done on GNU/Linux with GCC, and on MacOS X with clang. Radare is also known to compile on many different systems and architectures \(including TCC and SunStudio\).

目前来说radare2的内核可以在很多系统和架构上编译，但是主要的开发工作在GNU/Linux用GCC，和在MacOS X上用clang完成。众所周知，Radare也可以在很多其他系统和架构上编译（包括 TCC和SunStudio）。

People often want to use radare as a debugger for reverse engineering. Currently, the debugger layer can be used on Windows, GNU/Linux \(Intel x86 and x86\_64, MIPS, and ARM\), FreeBSD, NetBSD, and OpenBSD \(Intel x86 and x86\_64\). There are plans to support Solaris and MacOS X.

人们通常想用radare作为逆向工程的调试器。目前，调试层可以在Windows, GNU/Linux\(Intel x86 and x86\_64, MIPS 和 ARM\), FreeBSD, NetBSD 和 OpenBSD \(Intel x86 and x86\_64\)下使用。我们有支持Solaris和MacOS X的打算。

Compared to core, the debugger feature is more restrictive portability-wise. If the debugger has not been ported to your favorite platform, you can disable the debugger layer with the --without-debugger `configure` script option when compiling radare2.

与核心相比，调试器的特性在移植上有更多限制。如果调试器没有被一直到你喜爱的平台，你可以通过--without-debugger这个\`configure\` 参数关闭调试器。

Note that there are I/O plugins that use GDB, GDB Remote, or Wine as back-ends, and therefore rely on presence of corresponding third-party tools.

注意有I/O插件使用了GDB，GDB Remote或者Wine作为后台，并因此依赖对应的第三方工具

To build on a system using ACR/GMAKE \(e.g. on \*BSD systems\):

在一些系统上\(例如在\*BSD系统上\)用以下命令构建：

```
$ ./configure --prefix=/usr
$ gmake
$ sudo gmake install
```

There is also a simple script to do this automatically:

也有一个简单的脚本可以自动完成：

```
$ sys/install.sh
```

### 静态构建

You can build statically radare2 and all the tools with the command:

你可以用如下命令静态地构建radare2和所有的工具：

```
$ sys/static.sh
```

### Docker

Radare2 repository ships a [Dockerfile](https://github.com/radare/radare2/blob/master/Dockerfile) that you can use with Docker.

Radare2仓库里有一个[Dockerfile](https://legacy.gitbook.com/book/xylearn/radare2book-chinese/edit#)，你可以用Docker运行它。

This dockerfile is also used by Remnux distribution from SANS, and is available on the docker [registryhub](https://registry.hub.docker.com/u/remnux/radare2/).

这个dockerfile也被来自SANS的Remnux发行版使用，并可以从docker [registryhub](https://registry.hub.docker.com/u/remnux/radare2/)获取

## 清理已安装的老版本Radare2

```
./configure --prefix=/old/r2/prefix/installation
make purge
```



