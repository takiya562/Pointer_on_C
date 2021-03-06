<center><font size='6'>Chapter three</font></center>

# 3.1 基本数据类型

关键词`signed`针对整型数来说看起来有些可有可无，毕竟编译器在缺省`符号`时会把整型数定义为有符号（**signed**）的。

然而，对于字符类型而言，不同的编译器可能得到的结果是不一样的。所以，对于一个可移植的程序，需要将使用的`char`型变量的值限制在**signed char**和**unsigned char**的交集中。

其次，不同的机器在处理**signed char**时更加的得心应手。

## 3.1.3

<font color='red'>对一个字符串常量进行修改，其效果是未定义的</font>

通常编译器会将字符串常量存储在一个地方，即使它在程序中多次出现。

用代码来说的话，`char s[10] = "whatever"`与`char *s = "whatever"`是不一样的。

前者程序会在stack分配10个字节给数组`s`，然后把`“whatever”`拷贝进去。stack是可读可写的，所以我们可以修改字符数组的值。

后者会在stack分配`sizeof(char*)`字节的空间给指针`s`，然后将`s`的值修改为字符串常量`"whatever"`的地址，而这段地址通常在只读数据段中。

# 3.4 常量

有趣的指针变量：

* `int *pi;` 

  > `pi`是一个普通的指向整型的指针

* `int const *pci`

  > `pci`是一个指向整型常量的指针，可以修改指针的值，但是不能修改指针所指向的值

* `int * const cpi`

  > `cpi`是一个指向整型的常量指针。此时不能修改指针的值，但是可以修改所指向的值

* `int const  * const cpci`

  > `cpci`是一个指向整型常量的常量指针，此时无论是指针本身还是指向的值都无法修改

# 3.6 链接属性

* 首先明确概念：**链接属性**决定如何处理不同文件中出现的标识符

  >  标识符的作用域与它的链接属性油管，但是这两个是两个不同的概念

* 种类：

  * external:

    >  不论被声明多少次、位于几个不同的源文件都表示同一个实体

  * internal:

    > 在同一个源文件内所有声明中都指同一个实体，但是位于不同的源文件的多个声明则分属不同的实体

  * none:

    > 这种标识符被当作单独的个体，换句话说标识符的多个声明被当作独立不同的实体

* 关键字：

  > 总体而言，这两个关键字用于在声明中修改标识符的链接属性

  * [static](#3.8 static关键字):

    > 只对缺省链接属性为`external`的声明才有改变链接属性的效果

  * extern:

    > 一般而言，为一个标识符指定`external`链接属性
    >
    > 需要注意的是，上述的效果只针对于源文件中一个标识符的第一次声明。但是，如果它用于该标识符的第2次或者以后的声明，它并不会改变第一次声明指定的链接属性

# 3.7 存储类型

变量的存储类型是指变量值的内存类型。变量的存储类型决定变量何时创建、何时销毁以及它的值将保持多久。

有三个地方可以用于存储变量：

1. 普通内存
2. 运行时栈
3. 硬件寄存器

* 静态变量：
  * 在代码块之外声明的变量总时存储于静态内存，这类变量被称为静态变量。
  * 它们的存储类型是无法被指定的。
  * 静态变量在程序运行之前创建，在程序的整个执行期间始终存在。它始终保持原先的值，除非给他赋一个不同的值或者结束程序。
* 自动变量：
  * 在代码块内部声明的变量，存储与堆栈中。
  * 可以用关键字`auto`对它进行修饰
  * 在运行时被创建，离开代码块时销毁
  * 如果加上`static`，存储类型就变为静态（不改变作用域）
* 寄存器变量：
  * 使用`register`进行修饰
  * 存储于寄存器中，比内存访问效率高
  * 不便声明太多，编译器有可能只取前几个，其余作为自动变量

# 3.8 static关键字

* 用于函数定义，或用于代码块之外的变量声明是，static关键字用于修改标识符的链接属性，不影响标识符的存储类型和作用域
* 用于代码块内部的变量声明时，static关键字用于修改变量的存储类型，但是不改变链接属性和作用域

