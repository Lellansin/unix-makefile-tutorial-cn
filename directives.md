# Makefile - 指令

有许多可用于各种形式的指令（Directives）。你系统上的`make`程序可能不支持所有的指令。所以请检查你的`make`是否支持我们在这里解释的指令。本教程中的指令均源自 GNU`make`。

## 条件指令

条件指令是 -

* **ifeq **（if eqaul）指令。它包含两个参数，用逗号分隔并用圆括号包围。变量替换在两个参数上执行，然后进行比较。如果两个参数匹配，则遵循 ifeq 后面的命令行；否则会被忽略。

* **ifneq **（if not eqaul）指令。它包含两个参数，用逗号分隔并用圆括号包围。变量替换在两个参数上执行，然后进行比较。如果两个参数不匹配，则遵循ifneq后面的makefile行;否则会被忽略。

* **ifdef **（if defined）指令。它包含单个参数。如果给定的参数为真，则条件成立。

* **ifndef **（if not defined）指令。它包含单个参数。如果给定的参数为假，则条件成立。

* **else **指令导致如果之前的条件没有被遵守以下行。在上面的例子中，这意味着每当没有使用第一个替代方案时，就会使用第二个替代链接命令。在条件中有其他选项是可选的。

* **endif **指令结束的语句，每个 if 条件必须以 endif 结尾。

### 条件指令的语法

最简单条件的语法如下 -

```
conditional-directive
   text-if-true
endif
```

text-if-true 可以是任何文本行，如果条件为真，则被视为 Makefile 的一部分。如果条件假，则不使用该内容。

复杂一点条件的语法如下 -

```
conditional-directive
   text-if-true
else
   text-if-false
endif
```

如果条件为真，则使用 text-if-true; 否则，使用 text-if-false。text-if-false 可以是任意数量的文本行。

无论条件简单还是复杂，条件指令的语法都是相同的。有四种不同的指令来测试各种条件。他们如同给出的 -

```
ifeq (arg1, arg2)
ifeq 'arg1' 'arg2'
ifeq "arg1" "arg2"
ifeq "arg1" 'arg2'
ifeq 'arg1' "arg2"
```

以上条件的相反指令如下 -

```
ifneq (arg1, arg2)
ifneq 'arg1' 'arg2'
ifneq "arg1" "arg2"
ifneq "arg1" 'arg2'
ifneq 'arg1' "arg2"
```

### 条件指令示例

```
libs_for_gcc = -lgnu
normal_libs =

foo: $(objects)
ifeq ($(CC) ,gcc)
        $(CC) -o foo $(objects) $(libs_for_gcc)
else
        $(CC) -o foo $(objects) $(normal_libs)
endif
```

## include 指令

include 指令指示`make`在执行的过程中读取一个或多个其他 Makefile 到当前 Makefile。该指令是 Makefile 中的一行，看起来如下所示 -

```
include filenames...
```

文件名可以包含 shell 格式的文件名匹配。额外的空格是允许的，并且在行的开始处被忽略，但不允许使用制表符（\t）。例如，如果你有三个 \`.mk' 文件，即 \`a.mk'，\`b.mk' 和 \`c.mk'，以及 $\(bar\)，那么它将扩展到 bish bash，然后如下所示表达。

```
include foo *.mk $(bar)

等同于 −

include foo a.mk b.mk c.mk bish bash
```

当`make`处理一个 include 指令时，它会暂停并依次读取每个列出的文件到当前 Makefile。完成后，`make`继续读取执行当前的Makefile。

## override 指令

如果一个变量已经设置了一个命令参数，那么 makefile 中的普通赋值就会被忽略。如果你想在 makefile 中继续修改变量，即使它是用一个命令参数设置的。那么你可以使用一个 override 指令，它是一条看起来如下所示的行：

```
override variable = value

或者

override variable := value
```



