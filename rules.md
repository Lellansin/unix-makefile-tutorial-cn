# 在Makefile中定义规则

Makefile 目标规则的一般语法形式 -

```
target [target...] : [dependent ....]
    [ command ...]
```

括号内的参数是可选的，省略号表示一个或多个。注意每个命令前面的标签是必需的。

下面给出一个简单的例子，您可以定义一个规则，使得 `make` 可以通过其他三个文件来达成目标 hello。

```
hello: main.o factorial.o hello.o
    $(CC) main.o factorial.o hello.o -o hello
```

**注 - **在这个例子中，你必须给出其他的规则来达成 main.o、factorial.o、hello.o 这三个目标。

依赖项比目标更新，则`make`执行一次一个命令（在宏替换之后）。如果任何依赖已经被创建，那么首先发生，所以你有一个递归根据当前目标，寻找目标的依赖以及递归查找依赖的依赖。

如果任何命令返回失败状态，则 `make`过程终止。这也是为什么你需要常备这种规则 -

```
clean:
    -rm *.o *~ core paper
```

`make`忽略以短划线开头的命令返回状态。Em，我想你可以把` - `当做是 Makefile 的行注释。

`make`会输出宏替换之后的命令，来向你展示正在执行的情况。有时你可能想关闭它。例如 -

```
install:
    @echo You must be root to install
```

在 Makefiles 中有些约定俗成的目标，每次碰到 Makefile 你都可以先查看一下其中有没有。这些约定的目标分别是：all（直接 make 调用的入口目标），install 和 clean。

* **make all** - \(或者直接运行 make\) 期望是：编译所有内容，以便在安装应用程序之前进行本地测试。

* **make install** - 这个目标通常期望为在适当的地方安装应用程序。但要注意事物安装在适合您系统的地方。

* **make clean **- 清理应用程序，摆脱可执行文件，任何临时文件，目标文件等。

## Makefile 隐藏规则

这个命令应该可以在所有情况下使用，适用于我们从源文件 x.cpp 中构建可执行文件 x 作为输出，所以可以说是一个隐含的规则 -

```
.cpp:
    $(CC) $(CFLAGS) $@.cpp $(LDFLAGS) -o $@
```

这个隐藏规则说明如何通过 x.c 生成 x - 对 x.c 运行 cc 并调用输出 x。该规则是隐含的，因为没有提到特定的目标。它可以用于所有情况。

另一个常见的隐含规则是用 .cpp（源文件）构建 .o（对象）文件。

```
.cpp.o:
        $(CC) $(CFLAGS) -c $<

或者 -

.cpp.o:
        $(CC) $(CFLAGS) -c $*.cpp
```



