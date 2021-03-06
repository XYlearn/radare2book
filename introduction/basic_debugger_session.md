## 基本调试

用`-d`选项启动radare以调试程序。你可以通过指定进程号(PID)附加(attach)到一个正在运行的进程，或是通过指定一个程序的名字和参数启动它。

    $ pidof mc
    32220
    $ r2 -d 32220

    $ r2 -d /bin/ls

在第二种情况下，调试器会fork并将ls程序装载到内存中。调试器会早在在程序执行到ld.so动态连接器后暂停。因此，在此时不要期待能看到程序入口点或是共享库。你可以通过为入口点设置其他名字以改变这一行为。具体为，在初始脚本中添加一条指令`e dbg.bep=entry`或`e dbg.bep=main`，通常初始脚本为`~/.radare2rc`。
（译者注：默认情况下dbp.bep=loader）

下面是一系列调试器常用命令：

    > d?          ; 获取调试器命令帮助
    > ds 3        ; 运行三步
    > db 0x8048920  ; 下断点
    > db -0x8048920 ; 移除断点
    > dc          ; 继续执行进程
    > dcs        ; 继续执行直到系统调用(syscall)
    > dd            ; 操作文件描述符
    > dm          ; 显示进程maps
    > dmp A S rwx  ; 改变A处大小为S的页的保护权限
    > dr eax=33 ; 设置寄存器值。 eax = 33

在radare中切换到可视模式使用调试器可能会更简单。在这种方式下你不需要记住非常多指令，也不同记住程序运行状态。用`V`进入可视模式：

    [0xB7F0C8C0]> V

在进入可视模式的初始界面为十六进制界面，指向当前目标程序程序指针（比方说 x86下为EIP）。按`p`可以让你在在剩余的可视界面间切换。你可以按`p`和`P`来往返于最常用的显示模式。
使用F7或是`s`来步入(step into)，用F8或`S`来步过(step over)当前指令。你可以使用`c`来切换光标模式来标记选择范围内字节（例如，在之后用nop覆盖它们）。按F2设置断点。

在可视模式下你可以先输入`:`然后输入普通的radare命令。例如，打印一块在ESI的内存内容：
    <按 ':'>
    x @ esi

按`?`获取可视模式下的帮助。可以使用方向键滚动帮助界面。按`q`可以退出帮助界面。

一个频繁使用的命令是dr，用来读写目标通用寄存器的值。你也可以操作硬件和扩张浮点寄存器。
