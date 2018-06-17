## 颜色

Console access is wrapped in API that permits to show output of any command as ANSI, w32 console or HTML formats (more to come: ncurses, Pango etc.) This allows radare's core to run inside environments with limited displaying capabilities, like kernels or embedded devices. It is still possible to receive data from it in your favorite format.
To enable colors support by default, add a corresponding configuration option to the .radare2 configuration file:
从控制台显示所有ANSI，32控制台或是HTML格式（将来更多的会有：ncurses，Pango等）已经被纳入API中。这让radare core可以在有限显示兼容的环境下运行，例如内核或嵌入式设备。你仍可以获得用你喜欢的格式打印的数据。在.radare2配置文件添加符合的配置选项来启用默认的颜色支持：

    $ echo 'e scr.color=true' >> ~/.radare2rc

It is possible to configure color of almost any element of disassembly output. For \*NIX terminals, r2 accepts color specification in RGB format. To change the console color palette use `ec` command. 
Type `ec` to get a list of all currently used colors. Type `ecs` to show a color palette to pick colors from:
可以配置反编译输出的几乎每一个元素。对于\*NIX终端，r2接受RGB格式的颜色。可以使用`ec`命令给终端调色。
输入`ec`获取所有当前使用的颜色列表。输入`ecs`显示可选取颜色的调色板：

![img](r2pal.png)

#### xvilka主题

    ec fname rgb:0cf
    ec label rgb:0f3
    ec math rgb:660
    ec bin rgb:f90
    ec call rgb:f00
    ec jmp rgb:03f
    ec cjmp rgb:33c
    ec offset rgb:366
    ec comment rgb:0cf
    ec push rgb:0c0
    ec pop rgb:0c0
    ec cmp rgb:060
    ec nop rgb:000
    ec b0x00 rgb:444
    ec b0x7f rgb:555
    ec b0xff rgb:666
    ec btext rgb:777
    ec other rgb:bbb
    ec num rgb:f03
    ec reg rgb:6f0
    ec fline rgb:fc0
    ec flow rgb:0f0

![img](r2-rainbow.png)
