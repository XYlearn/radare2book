## 总览

The Radare2 project is a set of small command-line utilities that can be used together or independently.

Radare2项目由一系列小的命令行工具组成，他们可以被一起使用也可以独立使用。

### radare2

The core of the hexadecimal editor and debugger. radare2 allows you to open a number of input/output sources as if they were simple, plain files, including disks, network connections, kernel drivers, processes under debugging, and so on.

十六进制编辑器和调试器的核心。radare2允许你像打开简单普通文件那样打开大量输入/输出文件，如磁盘、网络连接、内核驱动、调试中的进程等。

It implements an advanced command line interface for moving around a file, analyzing data, disassembling, binary patching, data comparison, searching, replacing, visualizing. It can be scripted with a variety of languages, including Ruby, Python, Lua, and Perl.

它实现了一个高级的命令行借口，可以浏览文件、分析数据、反汇编、打二进制补丁、做数据比较、搜索、替换、可视化。它提供了不同语言脚本功能，包括Ruby，Python，Lua和Perl。

### rabin2

A program to extract information from executable binaries, such as ELF, PE, Java CLASS, and Mach-O. rabin2 is used by the core to get exported symbols, imports, file information, cross references \(xrefs\), library dependencies, sections, etc.

一个从可执行二进制文件中提取信息的程序，可以解析如ELF，PE，Java class文件和Mach-O文件。rabin2被radare2内核用于获取导入导出符号、文件信息、交叉引用、库依赖、节信息等。

### rasm2

A command line assembler and disassembler for multiple architectures \(including Intel x86 and x86-64, MIPS, ARM, PowerPC and Java\).

一个支持大量架构（包括 Intel x86和x86-64、MIPS、ARM、PowerPC和Java）的命令行汇编和反汇编工具

#### 例子

```
$ rasm2 -a java 'nop'
00

$ rasm2 -a x86 -d '90'
nop

$ rasm2 -a x86 -b 32 'mov eax, 33'
b821000000

$ echo 'push eax;nop;nop' | rasm2 -f -
509090
```

### rahash2

An implementation of a block-based hash tool. From small text strings to large disks, rahash2 supports multiple algorithms, including MD4, MD5, CRC16, CRC32, SHA1, SHA256, SHA384, SHA512, par, xor, xorpair, mod255, hamdist, or entropy.  
rahash2 can be used to check the integrity of, or track changes to, big files, memory dumps, and disks.

一个基于块的哈希工具。小到文本大到磁盘，rahash2 都支持多种算法，包括MD4, MD5, CRC16, CRC32, SHA1, SHA256, SHA384, SHA512, par, xor, xorpair, mod255, hamdist或entropy。rahash2可用于对大文件、内存转储文件和磁盘进行完整性校验和修改追踪。

### 例子

```
$ rahash2 file
file: 0x00000000-0x00000007 sha256: 887cfbd0d44aaff69f7bdbedebd282ec96191cce9d7fa7336298a18efc3c7a5a

$ rahash2 file -a md5
file: 0x00000000-0x00000007 md5: d1833805515fc34b46c2b9de553f599d
```

### radiff2

A binary diffing utility that implements multiple algorithms. It supports byte-level or delta diffing for binary files, and code-analysis diffing to find changes in basic code blocks obtained from the radare code analysis, or from the IDA analysis using the rsc idc2rdb  script.

一个实现了多种算法的文件比较工具。它支持对二进制文件的字节级或增量对比。也支持对代码进行对比找到基本代码块的改变。基本代码块可从radare code analysis获取或使用rsc idc2rdb脚本从IDA analysis获取。

### rafind2

A program to find byte patterns in files.

一个在文件中查找匹配字节的程序

### ragg2

A frontend for r\_egg. ragg2 compiles programs written in a simple high-level language into tiny binaries for x86, x86-64, and ARM.

一个r\_egg的前段，用于将用简单高级语言编写的代码编译成小型二进制程序。支持x86, x86-64和ARM。

#### Examples

```
   $ cat hi.r
   /* hello world in r_egg */
   write@syscall(4); //x64 write@syscall(1);
   exit@syscall(1); //x64 exit@syscall(60);

   main@global(128) {
     .var0 = "hi!\n";
     write(1,.var0, 4);
     exit(0);
   }
   $ ragg2 -O -F hi.r
   $ ./hi
   hi!

   $ cat hi.c
   main@global(0,6) {
     write(1, "Hello0", 6);
     exit(0);
   }
   $ ragg2 hi.c
   $ ./hi.c.bin
   Hello
```

### rarun2

A launcher for running programs within different environments, with different arguments, permissions, directories, and overridden default file descriptors. rarun2 is useful for:

* Crackmes
* Fuzzing
* Test suites

一个用于在不同环境，不同参数，权限，目录和重定向文件描述符条件下运行程序的程序运行器。rarun2可以用于：

* 破解
* 模糊测试
* 测试

#### rarun2脚本示例

```
   $ cat foo.rr2
   #!/usr/bin/rarun2
   program=./pp400
   arg0=10
   stdin=foo.txt
   chdir=/tmp
   #chroot=.
   ./foo.rr2
```

#### 将一个程序连接到套接字

```
   $ nc -l 9999
   $ rarun2 program=/bin/ls connect=localhost:9999
```

#### 调试程序时将IO重定向到另一个终端

1 - open a new terminal and type 'tty' to get a terminal name:

1- 打开一个新的终端并输入 'tty' 以获取终端名：

```
$ tty ; clear ; sleep 999999
/dev/ttyS010
```

2 - Create a new file containing the following rarun2 profile named foo.rr2:

2 - 创建一个包含下列rarun2配置的文件，命名为foo.rr2：

```
#!/usr/bin/rarun2
program=/bin/ls
stdio=/dev/ttys010
```

3 - Launch the following radare2 command: r2 -r foo.rr2 -d /bin/ls

3 - 运行radare2命令：r2 -r foo.rr2 -d /bin/ls

### rax2

A minimalistic mathematical expression evaluator for the shell that is useful for making base conversions between floating point values, hexadecimal representations, hexpair strings to ASCII, octal to integer, etc. It also supports endianness settings and can be used as an interactive shell if no arguments are given.

一个简约的命令行数学表达式运算器，用于转换浮点数和十六进制数、转换ascii字符串和十六进制字符串，八进制转十进制等等。它还提供了端序设置。如果不提供参数则提供一个交互式终端。

#### 例子

```
$ rax2 1337
0x539

$ rax2 0x400000
4194304

$ rax2 -b 01111001
y

$ rax2 -S radare2
72616461726532

$ rax2 -s 617765736f6d65
awesome
```



