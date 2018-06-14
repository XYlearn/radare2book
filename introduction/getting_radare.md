## Getting radare2

You can get radare from the website, [http://radare.org/](http://radare.org/), or the GitHub repository, [https://github.com/radare/radare2](https://github.com/radare/radare2).

你可以从网页获取radare,[http://radare.org/](http://radare.org/),或从GitHub仓库获取,[https://github.com/radare/radare2](https://github.com/radare/radare2)。

Binary packages are available for a number of operating systems \(Ubuntu, Maemo, Gentoo, Windows, iPhone, and so on\). Yet, you are highly encouraged to get the source and compile it yourself to better understand the dependencies, to make examples more accessible and of course to have the most recent version.

你可以获取支持不同操作系统的二进制程序包。但是，我们建议你下载源代码，并亲自编译它们以对依赖有一个更好的了解，更容易获取样例并且，当然，获得最新的版本。

A new stable release is typically published every month. Nightly tarballs are sometimes available at [http://bin.rada.re/](http://bin.rada.re/).

新的稳定版本通常在每一个月发布。有些晚上发布的tar包可以从 [http://bin.rada.re/](http://bin.rada.re/) 获取。

The radare development repository is often more stable than the 'stable' releases. To obtain the latest version:

radare的开发仓库通常比“稳定”发行版更加稳定。如下命令获取最新版本

```
$ git clone https://github.com/radare/radare2.git
```

This will probably take a while, so take a coffee break and continue reading this book.

这很可能会花费一段时间。所以请喝杯咖啡继续阅读这本书。

To update your local copy of the repository, use `git pull` anywhere in the radare2 source code tree:

在radare2的源代码目录树下的任意一处使用\`git pull\` 更新本地仓库

```
$ git pull
```

If you have local modifications of the source, you can revert them \(and lose them!\) with:

如果你修改了本地源代码，你可以回退（并删除更改）：

```
$ git reset --hard HEAD
```

Or send me a patch:

或给我发送一个补丁：

```
$ git diff > radare-foo.patch
```

The most common way to get r2 updated and installed system wide is by using:

最普通的更新并在整个系统上安装r2的方法为：

```
$ sys/install.sh
```

### 用meson + ninja构建

There is also a work-in-progress support for Meson.

目前也有对Meson的支持

Using clang and ld.gold makes the build faster:

使用clang和ld.gold使构建更迅速：

```bash
CC=clang LDFLAGS=-fuse-ld=gold meson . release --buildtype=release --prefix ~/.local/stow/radare2/release
ninja -C release
# ninja -C release install
```

### 帮助脚本

Take a look at the `sys/*` scripts, those are used to automate stuff related to syncing, building and installing r2 and its bindings.

看一看`sys/*`的脚本，这些是用于自动化同步、构建和安装r2和它的其他文件的

The most important one is `sys/install.sh`. It will pull, clean, build and symstall r2 system wide.

最重要的一个脚本是`sys/install.sh`。它会拉取，清空，构建并在系统上安装\(symstall\)r2。

Symstalling is the process of installing all the programs, libraries, documentation and data files using symlinks instead of copying the files.

Symstall使用符号链接而不是文件拷贝来安装所有程序，库，文档和数据文件。

By default it will be installed in /usr, but you can define a new prefix as argument.

默认情况下程序会被安装在/usr。但是你可以通过参数自定义前缀。

This is useful for developers, because it permits them to just run 'make' and try changes without having to run make install again.

### Cleaning Up

Cleaning up the source tree is important to avoid problems like linking to old objects files or not updating objects after an ABI change.

The following commands may help you to get your git clone up to date:

```
$ git clean -xdf
$ git reset --hard @~10
$ git pull
```

If you want to remove previous installations from your system, you must run the following commands:

```
$ ./configure --prefix=/usr/local
$ make purge
```



