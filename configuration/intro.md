# 配置

radare2内核在开始时会读取`~/.radare2rc`。你可以在这个文件添加`e`命令按你的口味调整radare的配置

为防止radare在开始时解析这个文件，传入`-N`选项。

所有radare的配置使用`eval`命令完成。一个典型的配置文件类似这样：

    $ cat ~/.radare2rc
    e scr.color = true
    e dbg.bep   = loader

位置可以用`-e` <config=value>命令行参数改变。你可以通过这种方式在命令行调整参数而不用修改.radare2rc文件。比方说，用空配置启动r2并调整`src.color`和`asm.syntax`可以使用如下命令完成：

    $ radare2 -N -e scr.color=true -e asm.syntax=intel -d /bin/ls

在内部，配置被存放在哈希表中。变量被组织在名字空间：`cfg.`，`file.`，`dbg.`，`scr.`等中。

只需要在命令行提示符下输入`e`就可以打印所有配置变量的列表。将输出限制在一个选定的名字空间中，给`e`传入一个以点结尾的参数。例如`e file`会显示所有在"file"名字空间中定义的变量。

输入`e?`以获得`e`命令的帮助：

    Usage: e[?] [var[=value]]
    e?              显示这个帮助
    e?asm.bytes     显示描述
    e??             列出带描述的配置变量
    e               列出配置变量
    e-              重置配置变量
    e*              显示以命令形式输出的配置变量
    e!a             反转a变量的bool值
    er [key]        设置配置变量为只读。设置后无法回退
    ec [k] [color]  为指定的键设置颜色（提示符，偏移，……）
    e a             获取变量'a'的值
    e a=b           将'a'变量设置为'b'的值
    env [k[=v]]     获取/设置环境变量


一个更简单的替代`e`的方式可以在可视化模式实现。输入`Ve`进入，使用方向键（上下左右）浏览配置，输入`q`以退出。初始的可视化配置界面类似这样：

    Eval spaces:                                                                   

    >  anal
       asm
       scr
       asm
       bin
       cfg
       diff
       dir
       dbg
       cmd
       fs
       hex
       http
       graph
       hud
       scr
       search
       io


对一些可以有很多值的配置变量，你可以使用`=?`操作符获取有效值的列表：

    [0x00000000]> e scr.nkey =?
    scr.nkey = fun, hit, flag
