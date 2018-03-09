# 在 Makefile 中定义自定义后缀规则

就其本身而言，`make`已知为了创建 .o文件，它必须在相应的 .c 文件上使用 cc -c。这些规则是内置的，你可以利用他们的优势来缩短你的 Makefile。如果只在 Makefile 的依赖行中指出当前目标所依赖的.h文件，将知道相应的.c文件已经是必需的。你甚至不需要包含编译器的命令。

这进一步减少了 Makefile，如图所示 -

```makefile
OBJECTS = main.o hello.o factorial.o
hello: $(OBJECTS)
        cc $(OBJECTS) -o hello
hellp.o: functions.h
main.o: functions.h 
factorial.o: functions.h 
```

`make` 使用名为 .SUFFIXES 的特殊目标来允许您定义自己的后缀。例如  -

```makefile
.SUFFIXES: .foo .bar
```

它通知`make`你将使用这些特殊的后缀来制定你自己的规则。

与`make`已知如何从 .c 文件创建 .o 文件类似，您可以按以下方式定义规则 -

```makefile
.foo.bar:
        tr '[A-Z][a-z]' '[N-Z][A-M][n-z][a-m]' < $< > $@
.c.o:
        $(CC) $(CFLAGS) -c $<
```

第一条规则允许你从 .foo 文件创建一个 .bar 文件。（它基本上是对文件进行加密）。

第二个规则是从 .c 文件创建 .o 文件时 `make` 使用的默认规则。



