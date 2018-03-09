# Makefile - 依赖关系

最后构建得到的二进制文件依赖于各种源代码和头文件是很常见的。依赖关系很重要，因为它们让 `make` 知道的任何执行**目标**的依赖。考虑下面的例子 -

```makefile
hello: main.o factorial.o hello.o
    $(CC) main.o factorial.o hello.o -o hello
```

其中，hello 为 make 的一个目标（target），而 hello 这个**目标**达成依赖于 main.o，factorial.o 和 hello.o 三个依赖（每个依赖都是另一个目标/文件）。因此，不论哪个依赖的目标发生了变化后， `make` 都会采取行动来重新（执行命令来）达到**目标**（即重新执行 `$(CC) main.o factorial.o hello.o -o hello`）。

同时，我们需要告诉 `make` 如何准备 .o 文件（即 hello 依赖的三个目标/文件）。因此我们需要定义这些依赖关系如下 -

```makefile
main.o: main.cpp functions.h
    $(CC) -c main.cpp

factorial.o: factorial.cpp functions.h
    $(CC) -c factorial.cpp

hello.o: hello.cpp functions.h
    $(CC) -c hello.cpp
```

以 main.o 为例，在依赖中写上如 functions.h 的意义在于。即使 main.cpp 文件没有修改，但是 functions.h 修改了，那么 `$(CC) -c main.cpp` 也会重新执行。如果目标依赖的文件都没有修改，那么执行 `make` 的时候就不会重新去运行其目标下的命令，即不会重复编译已经编译好的代码。

