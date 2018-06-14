## 清理老版本清理编译和移植

目前来说radare2的内核可以在很多系统和架构上编译，但是主要的开发工作在GNU/Linux用GCC，和在MacOS X上用clang完成。众所周知，Radare也可以在很多其他系统和架构上编译（包括 TCC和SunStudio）。

人们通常想用radare作为逆向工程的调试器。目前，调试层可以在Windows, GNU/Linux\(Intel x86 and x86\_64, MIPS 和 ARM\), FreeBSD, NetBSD 和 OpenBSD \(Intel x86 and x86\_64\)下使用。我们有支持Solaris和MacOS X的打算。

与核心相比，调试器的特性在移植上有更多限制。如果调试器没有被一直到你喜爱的平台，你可以通过--without-debugger这个\`configure\` 参数关闭调试器。

注意有I/O插件使用了GDB，GDB Remote或者Wine作为后台，并因此依赖对应的第三方工具

在一些系统上\(例如在\*BSD系统上\)用以下命令构建：

```
$ ./configure --prefix=/usr
$ gmake
$ sudo gmake install
```

也有一个简单的脚本可以自动完成：

```
$ sys/install.sh
```

### 静态构建

你可以用如下命令静态地构建radare2和所有的工具：

```
$ sys/static.sh
```

### Docker

Radare2仓库里有一个[Dockerfile](https://legacy.gitbook.com/book/xylearn/radare2book-chinese/edit#)，你可以用Docker运行它。

这个dockerfile也被来自SANS的Remnux发行版使用，并可以从docker [registryhub](https://registry.hub.docker.com/u/remnux/radare2/)获取

## 清理已安装的Radare2老版本

```
./configure --prefix=/old/r2/prefix/installation
make purge
```



