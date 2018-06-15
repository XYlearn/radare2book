## Radare基本用法

The learning curve for radare is usually somewhat steep at the beginning. Although after an hour of using it you should easily understand how most things work, and how to combine various tools radare offers, you are encouraged to read the rest of this book to understand how some non-trivial things work, and to ultimately improve your skills with radare.
radare的学习曲线在开始通常很陡峭。尽管在一小时的使用后你应该能简单地理解大多数东西是如何工作的，并且如何结合使用radare提供的众多工具。你最好继续阅读这本书的剩余部分以理解一些不简单的运作原理，并最终提升你的radare使用技术。

Navigation, inspection and modification of a loaded binary file is performed using three simple actions: seek (to position), print (buffer), and alternate (write, append).
浏览，检视和修改一个已加载的文件可使用三个简单操作完成：seek(to position)，print (buffer)和alternate(write, append)。

The 'seek' command is abbreviated as `s` and accepts an expression as its argument. The expression can be something like `10`, `+0x25`, or `[0x100+ptr_table]`. If you are working with block-based files, you may prefer to set the block size to a required value with `b` command, and seek forward or backwards with positions aligned to it. Use `s++` and `s--` commands to navigate this way.
'seek'命令简写为`s`并接受一个表达式作为参数。表达式可以是像`10`, `+0x25`, 或 `[0x100+ptr_table]`这样的东西。如果你打开的是基于块的文件，你可以用`b`将块大小设置成需要的大小，并且使用`s++`和`s--`向前或向后跳转到块对其的位置。

If r2 opens an executable file, by default it will open the file in VA mode and the sections will be mapped to their virtual addresses. In VA mode, seeking is based on the virtual address and the starting position is set to the entry point of the executable. Using `-n` option you can suppress this default behavior and ask r2 to open the file in non-VA mode for you. In non-VA mode, seeking is based on the offset from the beginning of the file.
如果r2打开了一个可执行文件，它默认会以虚拟地址模式打开并且程序节会被映射到对应的虚拟地址上。在虚拟地址模式，跳转基于虚拟地址并且开始位置被设置为可执行程序的入口点。你可以使用`-n`选项禁用这一默认行为并且让r2以非虚拟地址模式打开文件。

The 'print' command is abbreviated as `p` and has a number of submodes — the second letter specifying a desired print mode. Frequent variants include `px` to print in hexadecimal, and `pd` for disassembling.
'print'命令简写为`p`并且有大量子模式 —— 第二个字母表示打印的模式。常用的字母包括 `px`——以十六进制形式答应和`pd`——打印反汇编指令。

To be allowed to write files, specify the `-w` option to radare when opening a file. The `w` command can be used to write strings, hexpairs (`x` subcommand), or even assembly opcodes (`a` subcommand). Examples:
打开文件时向radare传入`-w`参数后可以修改文件。`w`命令可以用于写如字符串，十六进制数，甚至是汇编指令('a' 子模式)。例如：

    > w hello world         ; 字符串(string)
    > wx 90 90 90 90        ; 十六进制数(hexpairs)
    > wa jmp 0x8048140      ; 汇编指令(assemble)
    > wf inline.bin         ; 写入文件

Appending a `?` to a command will show its help message, for example, `p?`.
在一个指令后使用?可以显示帮助信息，例如`p?`。
Appending `?*` will show commands starting with the given string, e.g. `p?*`.
在指令后使用`?*`可以显示以给出字符串开头的所用命令，例如`p?*`。

To enter visual mode, press `V<enter>`. Use `q` to quit visual mode and return to the prompt.
In visual mode you can use HJKL keys to navigate (left, down, up, and right, respectively). You can use these keys in cursor mode toggled by `c` key. To select a byte range in cursor mode, hold down `SHIFT` key, and press navigation keys HJKL to mark your selection.
While in visual mode, you can also overwrite bytes by pressing `i`. You can press `TAB` to switch between the hex (middle) and string (right) columns. Pressing `q` inside the hex panel returns you to visual mode.
输入`V<enter>`进入可视模式，使用`q`退出可是模式并返回命令行提示符界面。
在可是模式下你可以使用HJKL键移动（左下上右）。你可以在光标模式下按`c`以使用这些按键。按住`SHIFT`键可以选择一系列字节，并且通过HIJK移动来界定你的选择。
在可是模式下，你也可以按`i`修改字节。你可以按`TAB`来在十六进制(中央)和字符串列表（右边）间进行切换。在十六进制面板按`q`返回到可视模式（注：应该是退出）。
