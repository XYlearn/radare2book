## 基本调试

To debug a program, start radare with the `-d` option. You can attach to a running process by specifying its PID, or you can start a new program by specifying its name and parameters:
用`-d`选项启动radare以调试程序。你可以通过指定进程号(PID)附加(attach)到一个正在运行的进程，或是通过指定一个程序的名字和参数启动它。

    $ pidof mc
    32220
    $ r2 -d 32220

    $ r2 -d /bin/ls

In the second case, the debugger will fork and load the debuggee `ls` program in memory. It will pause its execution early in `ld.so` dynamic linker. Therefore, do not expect to see an entrypoint or shared libraries at this point. You can override this behavior by setting another name for and entry breakpoint. To do this, add a radare command `e dbg.bep=entry` or `e dbg.bep=main` to your startup script, usually it is `~/.radare2rc`.
Be warned though that certain malware or other tricky programs can actually execute code before `main()` and thus you'll be unable to control them.
在第二种情况下，调试器会fork并将ls程序装载到内存中。调试器会早在在程序执行到ld.so动态连接器后暂停。因此，在此时不要期待能看到程序入口点或是共享库。你可以通过为入口点设置其他名字以改变这一行为。具体说，在初始脚本中添加一条指令`e dbg.bep=entry`或`e dbg.bep=main`，通常初始脚本为`~/.radare2rc`。
（译者注：默认情况下dbp.bep=loader）

Below is a list of most common commands used with debugger:
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

Maybe a simpler method to use debugger in radare is to switch it to visual mode. That way you will not have to remember many commands nor to keep program state in your mind. To enter visual mode use `V`:
在radare中切换到可视模式使用调试器可能会更简单。在这种方式下你不需要记住非常多指令，也不同记住程序运行状态。用`V`进入可视模式：

    [0xB7F0C8C0]> V

The initial view after entering visual mode is a hexdump view of current target program counter (e.g., EIP for x86). Pressing `p` will allow you to cycle through the rest of visual mode views. You can press `p` and `P` to rotate through the most commonly used print modes.
Use F7 or `s` to step into and F8 or `S` to step over current instruction. With the `c` key you can toggle the cursor mode to mark a byte range selection (for example, to later overwrite them with nop). You can set breakpoints with `F2` key.
在进入可视模式的初始界面为十六进制界面，指向当前目标程序程序指针（比方说 x86下为EIP）。按`p`可以让你在在剩余的可视界面间切换。你可以按`p`和`P`来往返于最常用的显示模式。
使用F7或是`s`来步入(step into)，用F8或`S`来步过(step over)当前指令。你可以使用`c`来切换光标模式来标记选择范围内字节（例如，在之后用nop覆盖它们）F2设置断点。

In visual mode you can enter regular radare commands by prepending them with `:`. For example, to dump a one block of memory contents at ESI:
在可视模式下你可以先输入`:`然后输入普通的radare命令。例如，打印一块在ESI的内存内容：
    <按 ':'>
    x @ esi

To get help on visual mode, press `?`. To scroll help screen, use arrows. To exit help view, press `q`.

A frequently used command is `dr`, to read or write values of target's general purpose registers. You can also manipulate the hardware and extended/floating point registers.
