## 获取radare2

你可以从网页获取radare,[http://radare.org/](http://radare.org/),或从GitHub仓库获取,[https://github.com/radare/radare2](https://github.com/radare/radare2)。

你可以获取支持不同操作系统的二进制程序包。但是，我们建议你下载源代码，并亲自编译它们以对依赖有一个更好的了解，更容易获取样例并且，当然，获得最新的版本。

新的稳定版本通常在每一个月发布一次。有些晚上发布的tar包可以从 [http://bin.rada.re/](http://bin.rada.re/) 获取。

radare的开发仓库通常比“稳定”发行版更加稳定。如下命令获取最新版本

```
$ git clone https://github.com/radare/radare2.git
```

这很可能会花费一段时间。所以请喝杯咖啡继续阅读这本书。

在radare2的源代码目录树下的任意一处使用\`git pull\` 更新本地仓库：

```
$ git pull
```

如果你修改了本地源代码，你可以回退（并删除更改）：

```
$ git reset --hard HEAD
```

或给我发送一个补丁：

```
$ git diff > radare-foo.patch
```

最简单的更新并在整个系统上安装r2的方法为：

```
$ sys/install.sh
```

### 用meson + ninja构建

目前也有对Meson的支持

使用clang和ld.gold使构建更迅速：

```bash
CC=clang LDFLAGS=-fuse-ld=gold meson . release --buildtype=release --prefix ~/.local/stow/radare2/release
ninja -C release
# ninja -C release install
```

### 帮助脚本

看一看`sys/*`下的脚本，这些是用于自动化同步、构建和安装r2和它的其他文件的

最重要的一个脚本是`sys/install.sh`。它会拉取，清空，构建并在系统上安装\(symstall\)r2。

Symstall使用符号链接而不是文件拷贝来安装所有程序，库，文档和数据文件。

默认情况下程序会被安装在/usr。但是你可以通过参数自定义前缀。

这对开发者很有帮助，因为这可以让他们通过运行make并且测试更改而不用重新运行make install。

### 清理

清理源代码目录树能有效避免诸如链接到旧的object文件或ABI改变后object文件等不到更新等问题。

下面的命令能帮助你让你的git clone仓库更至最新：

```
$ git clean -xdf
$ git reset --hard @~10
$ git pull
```

如果你想要从你的系统中移除之前的安装，你需要运行以下的命令：

```
$ ./configure --prefix=/usr/local
$ make purge
```



