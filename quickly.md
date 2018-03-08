# 为什么我们需要 Makefile?
手动编译源代码文件很麻烦，特别是当你要编译多个源文件，并切每次编译都要重复输入编译命令的时候。而 Makefiles 正是为了简化这个过程的一个工具。

首先你需要了解 Makefiles 是一种特殊格式的文件，可帮助自动构建和管理项目。

例如，我们假设我们有以下源文件。

* main.cpp
* hello.cpp
* factorial.cpp
* functions.h

*main.cpp中*
```cpp
#include <iostream>

using namespace std;

#include "functions.h"

int main(){
   print_hello();
   cout << endl;
   cout << "The factorial of 5 is " << factorial(5) << endl;
   return 0;
}
```

*hello.cpp*
```cpp
#include <iostream>

using namespace std;

#include "functions.h"

void print_hello(){
   cout << "Hello World!";
}
```

factorial.cpp
```cpp
#include "functions.h"

int factorial(int n){

   if(n!=1){
      return(n * factorial(n-1));
   }
   else return 1;
}
```

functions.h
```cpp
void print_hello();
int factorial(int n);
```

编译文件并获取可执行文件的简单方法是运行命令

```bash
gcc  main.cpp hello.cpp factorial.cpp -o hello
```

该命令生成 hello 二进制文件。在这个例子中，我们的项目里只有四个文件，同时我们也知道这些函数调用的顺序。因此，键入上述命令并准备最终二进制文件是可行的。但是，对于拥有数千个源代码文件的大型项目来说，想要维护这个二进制版本会变得非常困难。

不过没关系，make命令正是用来管理大型程序或程序组的。当你开始编写大型程序时，你会注意到重新编译大型程序比重新编译短程序需要更长的时间。此外，你也可能注意到通常情况下开发者只能在程序的一小部分（例如单个函数）上工作，其余大部分程序文件都没有变化。

在随后的部分中，我们看到如何为我们的项目准备一个 Makefile。

