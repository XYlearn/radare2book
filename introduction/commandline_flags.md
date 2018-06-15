## 命令行选项

radare core提供了非常多命令行选项

摘抄于用法帮助：
（译者注：现在的参数远不止这些）

    $ radare2 -h
    Usage: r2 [-dDwntLqv] [-P patch] [-p prj] [-a arch] [-b bits] [-i file] [-s addr] [-B blocksize] [-c cmd] [-e k=v] file|-

       -a [arch]    设置 asm.arch
       -A           运行 'aa' 命令以分析所有被引用的代码
       -b [bits]    设置 asm.bits
       -B [baddr]   设置PIE文件的基地址
       -c 'cmd..'   执行radare命令
       -C           将'file'作为 host:port (等价于 -c+=http://%s/cmd/)
       -d           将'file'作为调试程序
       -D [backend] 开启调试模式(e cfg.debug=true)
       -e k=v       执行变量配置
       -f           块大小 = 文件大小
       -h, -hh      显示磅数信息, -hh显示更多
       -i [file]    运行脚本文件
       -k [kernel]  为asm和anal设置set asm.os变量
       -l [lib]     载入插件
       -L           打印支持的IO插件
       -m [addr]    将文件映射(map)到指定地址
       -n           禁用分析
       -N           禁用用户设置
       -q           安静模式（没有提示符）并在-i后退出
       -p [prj]     设置工程文件
       -P [file]    应用rapatch文件并退出
       -s [addr]    初始seek(转移)
       -S           用沙盒模式启动r2
       -t           在线程中加载rabin2信息
       -v, -V       显示radare2版本 (-V 显示库版本)
       -w           以写模式打开文件

常用命令行选项。

* 以写模式打开文件且不解析文件的格式头

    $ r2 -nw file

* 迅速进入r2 shell不打开任何文件

    $ r2 -

打开fatbin文件时指定子文件：

    $ r2 -a ppc -b 32 ls.fat

在显示命令行交互提示符钱运行脚本：

    $ r2 -i patch.r2 target.bin

执行一条命令并退出，不进入交互模式：

    $ r2 -qc ij hi.bin > imports.json

配置变量：

    $ r2 -e scr.color=false blah.bin

调试程序：

    $ r2 -d ls

使用一个已存在的工程文件

    $ r2 -p test

