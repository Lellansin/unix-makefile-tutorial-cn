# Makefile - 其他特性

## 递归使用 make

递归使用 `make` 的方式即在 Makefile 中将 `make` 用作命令。这个用法的主要用途是将一个大系统的拆成多个子系统i组合编译时。比如说，你有一个名为 \`subdir' 的子目录，它有自己的 Makefile，并且您希望在包含 Makefile 的子目录上运行 make。那么可以通过这个来做到这一点 -

```
subsystem:
        cd subdir && $(MAKE)

或者，等效的 −

subsystem:
        $(MAKE) -C subdir
```

你可以基于这个例子来编写递归`make`命令，但是你需要知道它们（每个子系统的 Makefile）是如何工作的、为什么它能工作，以及子 make 如何与顶层 make 相关联。

## 将变量传达给子 make

`make`通过明确的请求可以将顶层 make 的变量值传递给子 make 的环境。这些变量在子 make 中被定义为默认值。你不能重写被子 make 的 Makefile 所使用的变量（除非你执行的时候使用\`-e' 开关）。

为了传递或导出变量，`make`会将该变量和它的值添加到每个运行命令的环境中。而子 make 会顺序的根据环境来初始化它的变量表。

特殊变量 SHELL 和 MAKEFLAGS 总是被导出（除非你明确的阻止）。只要设置了 MAKEFILES 的内容，它就会被导出。

如果您想要将指定变量导出到子 make 中，请使用 **export** 指令，如图所示 -

```
export variable ...
```

如果您想阻止变量被导出，请使用 **unexport** 指令，如图所示 -

```
unexport variable ...
```

## 变量 MAKEFILES

如果定义了环境变量MAKEFILES，则将`make`其值视为其他makefile的名称列表（以空格分隔），以便在其他makefile之前读取。这与include指令非常相似：搜索各种目录以查找这些文件。

MAKEFILES的主要用途是在递归调用之间进行通信`make`。

## 引入不同目录的头文件

如果你将头文件放在不同的目录中并且在在不同的目录中运行`make`，则需要提供头文件的路径。这可以在 Makefile 中使用 **-I **选项完成。假设 functions.h 文件在 `/home/tutorialspoint/`  头文件夹中可用，其余文件在 `/home/tutorialspoint/src/` 文件夹中可用，则 Makefile 可以按如下方式编写：

```
INCLUDES = -I "/home/tutorialspoint/header"
CC = gcc
LIBS =  -lm
CFLAGS = -g -Wall
OBJ =  main.o factorial.o hello.o

hello: ${OBJ}
   ${CC} ${CFLAGS} ${INCLUDES} -o $@ ${OBJS} ${LIBS}
.cpp.o:
   ${CC} ${CFLAGS} ${INCLUDES} -c $<
```

## 将更多文本附加到变量

对已经定义的变量的值添加更多文本通常很有用。你可以用 \`+='  来做到这一点，如下所示：

```
objects += another.o
```

它采用变量对象的值，并向其添加文本 “another.o”（前面有一个空格）。因此 -

```
objects = main.o hello.o factorial.o
objects += another.o
```

将对象设置为\`main.o hello.o factorial.o another.o'。

使用 '+=' 类似于：

```
objects = main.o hello.o factorial.o
objects := $(objects) another.o
```

## Makefile 中的多行写法

如果你觉得 Makefile 中某一行太长，那么你可以使用反斜线 “\” 来分隔你的行，如下所示 -

```
OBJ =  main.o factorial.o \
    hello.o


等同于


OBJ =  main.o factorial.o hello.o
```

## Makefile 别名

如果本地目录中已经有了名称为“Makefile” 的 Makefile，那么只需在命令行下输入 make 回车它就会运行 Makefile 文件。但是，如果你的 Makefile 是其他名字，可以使用以下命令来运行 -

```
make -f your-makefile-name
```



