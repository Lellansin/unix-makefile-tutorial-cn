# 宏

`make` 允许你使用类似变量的宏。宏在 Makefile 中使用 名称 = 值 的形式来定义。例如

```makefile
MACROS  = -me
PSROFF  = groff -Tps
DITROFF = groff -Tdvi
CFLAGS  = -O -systype bsd43
LIBS    = "-lncurses -lm -lsdl"
MYFACE  = ":*)"
```

## 特殊的宏

在运行任何目标集（target rule set）中的指令之前，有一些预定义的特殊宏 -

* **$@** 要创建的文件的名称。

* **$?** 是被更改的对应文件名。

例如，在下列场景中我们可以使用如下规则：

```makefile
hello: main.cpp hello.cpp factorial.cpp
    $(CC) $(CFLAGS) $? $(LDFLAGS) -o $@

或者 −

hello: main.cpp hello.cpp factorial.cpp
    $(CC) $(CFLAGS) $@.cpp $(LDFLAGS) -o $@
```

在这个例子中，像 $\(CC\) $\(CFLAGS\) $? $\(LDFLAGS\) -o $@ 这样的操作行应该在行首输入一个制表符 \(\t\) ，否则 make 会报错。 其中的 $@ 代表 hello 同时 $? 或者 $@.cpp 则代表所有已修改源文件。

同时默认规则中还有另外两个特殊的宏。他们分别是 -

* **$&lt;** 触发操作的相关文件的名称。

* **$\*** 由目标文件和依赖文件共享的前缀。

常见的隐式规则是用于构建 .cpp（源文件）之外的 .o（对象）文件。

```makefile
.cpp.o:
    $(CC) $(CFLAGS) -c $<

或者 −

.cpp.o:
    $(CC) $(CFLAGS) -c $*.c
```

## 常规宏

有各种默认的宏。你可以通过 “make -p” 命令看到他们。大多数的常规宏都是见名知意。

这些预定义变量（即隐式规则中使用的宏）分为两类 -

* 作为程序名称的宏（如CC）

* 包含程序参数的宏（如CFLAGS）。

以下是一些用作 Makefile 内置规则中程序名称的常用变量表。

| AR | 档案保存程序;默认是\`ar'。 |
| :--- | :--- |
| AS | 编译汇编文件的程序；默认是 \`as'。 |
| CC | 编译 C 语言文件的程序；默认是 \`cc'. |
| CO | Program for checking out files from RCS; default is \`co'. |
| CXX | 编译 C++ 文件的程序; 默认是 \`g++'. |
| CPP | 运行 C 预处理器并输出到当前输出流的程序; 默认是 \`$\(CC\) -E'. |
| FC | Program for compiling or preprocessing Fortran and Ratfor programs; default is \`f77'. |
| GET | Program for extracting a file from SCCS; default is \`get'. |
| LEX | Program to use to turn Lex grammars into source code; default is \`lex'. |
| YACC | Program to use to turn Yacc grammars into source code; default is \`yacc'. |
| LINT | Program to use to run lint on source code; default is \`lint'. |
| M2C | Program to use to compile Modula-2 source code; default is \`m2c'. |
| PC | 编译 Pascal 的程序；默认值是 \`pc'。 |
| MAKEINFO | Program to convert a Texinfo source file into an Info file; default is \`makeinfo'. |
| TEX | Program to make TeX dvi files from TeX source; default is \`tex'. |
| TEXI2DVI | Program to make TeX dvi files from Texinfo source; default is \`texi2dvi'. |
| WEAVE | 将 Web 转化为 TeX 的程序；默认值是 \`weave'。 |
| CWEAVE | 将 C Web 转换为 TeX 的程序；默认值是 \`cweave'。 |
| TANGLE | 将 Web 转化为 Pascal 的程序；默认值是 \`tangle'。 |
| CTANGLE | 将 C Web 转换为 C 的程序；默认值是 \`ctangle'。 |
| RM | 删除文件的命令；默认是 \`rm -f'. |

这是一个变量表，其值是上述程序的附加参数。除非另有说明，否则所有这些变量的默认值都是空字符串。

| ARFLAGS | Flags to give the archive-maintaining program; default is \`rv'. |
| :--- | :--- |
| ASFLAGS | Extra flags to give to the assembler when explicitly invoked on a \`.s' or \`.S' file. |
| CFLAGS | 传递给 C 编译器的额外 flag。 |
| CXXFLAGS | 传递给 C 编译器 的额外 flag。 |
| COFLAGS | Extra flags to give to the RCS co program. |
| CPPFLAGS | Extra flags to give to the C preprocessor and programs, which use it \(such as C and Fortran compilers\). |
| FFLAGS | Extra flags to give to the Fortran compiler. |
| GFLAGS | Extra flags to give to the SCCS get program. |
| LDFLAGS | Extra flags to give to compilers when they are supposed to invoke the linker, \`ld'. |
| LFLAGS | Extra flags to give to Lex. |
| YFLAGS | Extra flags to give to Yacc. |
| PFLAGS | Extra flags to give to the Pascal compiler. |
| RFLAGS | Extra flags to give to the Fortran compiler for Ratfor programs. |
| LINTFLAGS | Extra flags to give to lint. |

**注 **- 你可以使用 '-R' 或 '--no-builtin-variables' 选项取消隐式规则使用的所有变量。

你也可以在命令行中定义宏，如下所示 -

```
make CPP = /home/courses/cop4530/spring02
```



