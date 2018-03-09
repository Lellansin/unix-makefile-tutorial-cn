# Makefile - 例子

这是用于编译 hello 程序的 Makefile 的一个完整例子。该程序由三个文件 main.cpp、factorial.cpp 和 hello.cpp 组成。

```
# 定义需要的宏
SHELL = /bin/sh

OBJS =  main.o factorial.o hello.o
CFLAG = -Wall -g
CC = gcc
INCLUDE =
LIBS = -lm

hello:${OBJ}
   ${CC} ${CFLAGS} ${INCLUDES} -o $@ ${OBJS} ${LIBS}

clean:
   -rm -f *.o core *.core

.cpp.o:
   ${CC} ${CFLAGS} ${INCLUDES} -c $<
```

现在，你可以使用`make`来构建的你的程序 **hello** 。如果运行`make clean`则会删除当前目录中所有的 .o 文件和 .core 文件。



