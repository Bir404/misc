

[TOC]

## 1.简单易学

**更具现代性：** 

​    完全面向对象，跨平台，支持Unicode、多线程，垃圾自动回收，类型反射，静态编译，动态类型装载等等。

**更简单易学：**

​    繁琐、晦涩、不常用的语言特性被尽量精简，且补充了很多便于使用的语言特性。支持中英文双语关键字，在关键字和语法格式方面，尽量与现有类似编程语言相同，减少了学习量。

**对系统环境的适应和控制能力更强：**

​    定义有语言无关的“EF对象模型”，允许使用其它各种编程语言直接书写“易语言.飞扬”本地类，和用“易语言.飞扬”本身书写的类完全融合互补，可用作快速建立强大高效的本地应用环境，同时可充分利用现有代码资源。  

## 2.跨平台多线程

**跨平台**：

​    同一份源代码，不经过任何修改（或少量修改）即可在不同的操作系统下编译运行。
​       ![img](http://www.dywt.com.cn/eftx/eftximages/etx_1.jpg)

**支持Unicode、多线程**：

​    Unicode，支持全球各个国家的语言文字，便于开发国际化软件。 多线程，充分发挥多核CPU性能，提高程序执行效率。
​       ![img](http://www.dywt.com.cn/eftx/eftximages/etx_2.jpg)

## 3.垃圾自动回收：

​    自动判断并回收无效对象（不再被使用的对象）。
​    降低项目开发难度。
​    增强程序稳定性。 
​       ![img](http://www.dywt.com.cn/eftx/eftximages/etx_4.jpg)

## 4.类型反射：

​    编译后的类型具有“自省”性。
​    可以在运行时获取类型（或类库）的定义信息。
​    可以根据类名称动态创建类对象，并调用对象指定方法。
​    提供“反射”类库供程序员使用。 
​       ![img](http://www.dywt.com.cn/eftx/eftximages/etx_5.jpg)

## 5. 静态编译：

​    源代码将被直接编译为可执行代码。
​    没有中间字节码，没有解释执行环节。
​    编译时执行严格的语法和数据类型检查。
​    绝大多数非逻辑性错误都能在编译时发现。 
​       ![img](http://www.dywt.com.cn/eftx/eftximages/etx_6.jpg)

## 6.动态类型装载：

​    类型总是以类库的形式存在。
​    类库总是在第一次用到时被加载。
​    类型总是在第一次用到时被装载。
​    类型可以随时被卸载。
​    动态类型装载有助于提升程序的模块化、灵活性和可扩展性。 
​       ![img](http://www.dywt.com.cn/eftx/eftximages/etx_7.jpg)

## 7.属性表：

​    属性表采用易读易写的类XML格式，用于辅助定义“类库、类、接口、枚举、常量、变量、参数、友好名称……”等几乎所有程序实体。 
​    属性表的位置通常紧跟在实体名称的后面，且用户可以根据情况灵活设置扩展属性，并可通过反射机制读取。

​       ![img](http://www.dywt.com.cn/eftx/eftximages/etx_8.jpg)  

##  8.类自然语言编程：

​    属性表采用易读易写的类XML格式，通过引入“友好名称”，易语言实现了“类自然语言编程”。 
​    友好名称也有“参数”的概念，但它的参数可以出现在友好名称中间的任意位置，参数的顺序也不重要——而不象类方法那样：参数只能顺次放在方法名称的后面（还要用小括号括起来）。

​       ![img](http://www.dywt.com.cn/eftx/eftximages/etx_9.jpg)  

## 9.多层嵌套注释：

​    “/*”表示多行注释的开始，“*/”表示多行注释的结束。 
​    和其它语言不同的是，“易语言.飞扬”多行注释内部允许嵌套使用单行注释和多行注释。

​       ![img](http://www.dywt.com.cn/eftx/eftximages/etx_10.jpg)  

## 10.多返回值：

​    方法可以有多个返回值。 
​    多个返回值可以有不同的数据类型。
​    多返回值给编写程序提供了更大的灵活性。 
​       ![img](http://www.dywt.com.cn/eftx/eftximages/etx_11.jpg)

## 11.嵌入类型和匿名类：

​    允许在类型内部嵌套定义其它类型。 
​    嵌入类可以被允许访问其外层类的所有成员。
​    可以创建匿名类对象。 
​       ![img](http://www.dywt.com.cn/eftx/eftximages/etx_12.jpg)

## 12.嵌入方法：

​    允许在方法内部嵌套定义其它方法。 
​    嵌入方法可以使用其外层方法中的参数和局部变量。
​    通过嵌入方法可以实现更小范围内的代码重用。 
​       ![img](http://www.dywt.com.cn/eftx/eftximages/etx_13.jpg)

## 13.属性和事件

 **属性：** 
    支持“对象.属性”语法，如“按钮1.标题”。 
    当属性被读取或赋值时，对象将会得到通知。
    本特性用作更好地支持快速应用程序开发。 
       ![img](http://www.dywt.com.cn/eftx/eftximages/etx_14.jpg)
  **事件：** 
    当对象收到某个事件时，其对应的事件处理方法将被调用。 本特性用作更好地支持快速应用程序开发。 
       ![img](http://www.dywt.com.cn/eftx/eftximages/etx_15.jpg)



##  14.中英文双语关键字：

​    为了兼顾已有程序员的思维习惯，“易语言.飞扬”中所有关键字和系统属性，都同时具有中英文两种名称，可以同时混用。 
​    在语法格式和关键字方面，尽量与现有类似编程语言相同，减少了学习量 。 
​       ![img](http://www.dywt.com.cn/eftx/eftximages/etx_16.jpg)



## 15.参数默认值：

​    方法的参数可以有默认值。 
​    与其它语言不同的是，“易语言.飞扬”任何一个参数都可以有默认值，不限于最后面的参数 。 
​       ![img](http://www.dywt.com.cn/eftx/eftximages/etx_17.jpg)

## 16.参数扩展：

​    方法的参数可以被扩展。 
​    不仅允许扩展最后一个参数，还允许以“组”为单位扩展最后N个参数。 
​       ![img](http://www.dywt.com.cn/eftx/eftximages/etx_18.jpg)



## 17.数据类型自动转换：

​    可实现基本数据类型数据和对象之间的自动相互转换。 
​       ![img](http://www.dywt.com.cn/eftx/eftximages/etx_19.jpg)



 

##  18.“动态类型”和“弱类型”：

​    通过在系统库中引入“通用型”、“数值型”、“数组型”等类，“易语言.飞扬”在一定程度上做到了“动态类型”和“弱类型”。
​    以通用型为例，可以将其它类型数据赋值给通用型变量，也可以将通用型变量赋值给其它数据类型变量。
​       ![img](http://www.dywt.com.cn/eftx/eftximages/etx_20.jpg)     如果某方法的参数为“通用型”，则该参数可以接收各种类型的参数值。

​       ![img](http://www.dywt.com.cn/eftx/eftximages/etx_21.jpg)



##   19.三联判断表达式：

​    “0 < x < 10”等效于“x > 0 && x < 10”。
​    前者更符合人的思维习惯，代码可读性好。
​    与数学表达式相一致，便于初学者理解。

​       ![img](http://www.dywt.com.cn/eftx/eftximages/etx_22.jpg)  

##   20.交换操作符：

​    交换操作符用于交换两个变量的值。
​    “a <=> b;”等效于“int c = a; a = b; b = c;”。
​    前者更直观，更简捷，代码可读性好。

​       ![img](http://www.dywt.com.cn/eftx/eftximages/etx_23.jpg)  

##   21.交换操作符：

​    交换操作符用于交换两个变量的值。
​    “a <=> b;”等效于“int c = a; a = b; b = c;”。
​    前者更直观，更简捷，代码可读性好。

​       ![img](http://www.dywt.com.cn/eftx/eftximages/etx_23.jpg)  

## 22.与 JAVA、C#、C++ 的异同：
