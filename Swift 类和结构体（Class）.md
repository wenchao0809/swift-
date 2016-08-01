# Swift 类和结构体（Class）
类定义语法

~~~swift
class ClassName {
	//class body
}
~~~

结构体定义语法

~~~swift
struct StructName {
	//struct bodt
}
~~~

**命名风格**

swift的类和结构体的命名要使用*UpperCamelCase*风格，字母均以大写字母开头。

**初始化一个类的实例**

~~~swift
Class Person {
	var name:string?
	var age = 20
	var weight = 60
	var height = 175
}
//生成一个实例
let somePerson = Person()
~~~

**属性访问**

通过使用点语法（dot syntax），访问实例的属性，实例名后面紧跟着属性名，两者通过点符号(.)链接:，另外你也可以使用点语法为变量属性赋值。

~~~swift
let age = somePerson.age
let name = somePerson.name!
//为变量属性赋值
somePerson += 2    //年龄增加了2岁
~~~

**！！！**

*学过objc的人都知道在objc中是不允许直接设置结构体属性的子属性的，但是swift支持之一操作。*

### 结构体类型的成员逐一构造器 （Memberwise Initializers for Structure Types）
所有结构体都会自动生成一个成员逐一构造器，用于初始化新结构体实例中的成员的属性。新实例中各个属性的初始值可以通过属性的名称产地到成员逐一构造器之中：

~~~swift
Struct Computer {
	var cpu: String
	var mem: Int
	var graph: String
	var mainBoard: String
}
//用逐一构造器初始化实例
let cmpt = Computer(cpu: "i5", mem: 8, graph: "GTX 960", mainBoard: "Z170")
~~~

**结构体和枚举是值类型**

>值类型：值类型被赋予给一个变量、常量或者被出传递给一个函数的时候，其值会被拷贝
>
>swift 中所有的基本类型都是值类型，并且在底层都是以结构体的形式所实现。

~~~swift
let lowCmp = Computer(cpu: "i5", mem: 8, graph: "GTX 960", mainBoard: "Z170")

//升级下cpu

var superCmp = lowCmp

superCmp.cpu = "i7"

print("The superCmp of cpu is \(superCmp.cpu)")
print("The superCmp of cpu is \(lowCmp.cpu)")
~~~
输出结果如下图

![](http://i2.piimg.com/4851/83b776bb446da599.png)

**类是引用类型**

~~~swift
class Person {
    var age: Int
    var name: String
    init (ag: Int, na: String) {
        age = ag
        name = na
    }
}

var p = Person(ag: 20, na: "Tom")
var p2 = p
p2.age = 22

print("The p2's age is \(p2.age)")
print("The p's age is \(p.age)")
~~~

输出如下图
![](http://i4.piimg.com/524586/18172522b40add3es.png)

**可以看到改变p2的age属性，p的age属性也会改变因为它俩本来就是指向同一个对象**

### 恒等运算符比较两个对象
为了判断两个变量是否引用同一个类的实例将会很有帮助。为此swift内建两个恒等预案算符

~~~swift
print(p1 === p) //true
~~~

### 类和结构体的选择

在你的代码中，你可以使用类和结构体来定义你的自定义数据类型。

然而，结构体实例总是通过值传递，类实例总是通过引用传递。这意味两者适用不同的任务。当你在考虑一个工程项目的数据结构和功能的时候，你需要决定每个数据结构是定义成类还是结构体。

按照通用的准则，当符合一条或多条以下条件时，请考虑构建结构体：

* 该数据结构的主要目的是用来封装少量相关简单数据值。
* 有理由预计该数据结构的实例在被赋值或传递时，封装的数据将会被拷贝而不是被引用。
* 该数据结构中储存的值类型属性，也应该被拷贝，而不是被引用。
* 该数据结构不需要去继承另一个既有类型的属性或者行为。


**！！**
>以上是对字符串、数组、字典的“拷贝”行为的描述。在你的代码中，拷贝行为看起来似乎总会发生。然而，Swift 在幕后只在绝对必要时才执行实际的拷贝。Swift 管理所有的值拷贝以确保性能最优化，所以你没必要去回避赋值来保证性能最优化
>



