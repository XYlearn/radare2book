## 指令格式

通常的radare指令格式如下：

    [.][times][cmd][~grep][@[@iter]addr!size][|>pipe] ;

指令由一个大小写敏感的字母区分[a-zA-Z]。在指令前增加数字来重复执行一条指令：

    px    # run px
    3px   # run px 3 times

`!`前缀用来在shell环境下执行一条指令。如果指令以感叹号开头，命令将会被传给在当前加载的I/O插件中的system()钩子函数。例如，这被用于在ptrace I/O插件中从radare命令行接受执行调试指令。

一些例子：

    ds                    ; 调用调试器的 'step' 指令
    px 200 @ esp          ; 显示esp指向位置的200个字节
    pc > file.c           ; 将缓冲区以C语言的字节数组的形式保存到file.c中
    wx 90 @@ sym.*        ; 在每一个符号上写入一个nop
    pd 2000 | grep eax    ; 匹配用到'eax'寄存器的opcodes
    px 20 ; pd 3 ; px 40  ; 在一行中写多个变量

`@`字符制定了一个一个临时偏移，在其左边的指令会在这个偏移上执行。原来的文件偏移位置在执行命令后会被还原。例如，`pd 5 @ 0x100000fce`在地址0x100000fce处反汇编5条指令。

`~`是一个内部的类似于grep的字符指令，用于筛选任何指令的输出。例如：

    pd 20~call            ; 反汇编20条指令并筛选其中输出含'call'的

此外，你也可以筛选列或行：

    pd 20~call:0          ; 获取第一行
    pd 20~call:1          ; 获取第二行
    pd 20~call[0]         ; 获取第一列
    pd 20~call[1]         ; 获取第二列

甚至结合它们：

    pd 20~call:0[0]       ; 筛选出第一行第一列为'call'的输出

这一内部功能是编写radare脚本的关键，因为他能被用于遍历一个由反汇编器、ranges或是其他指令生成的偏移或是数据列表。你可以参考宏这一节的迭代(iterator)部分来获取更多信息。

using `!~...` command - it offers a visual mode to scroll through the radare2 command history.
大多数命令支持`TAB`自动补全，例如`s`eek或是`f`lags命令。自动补全提供了所有可能的取值，在这个例子中包括了flag名字。注意，可以使用`!~...`命令查看命令的历史，这条命令提供了一个可滚动的可视界面。
