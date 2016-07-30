# Swift 数组
### 语法
Array\<Element> Element的类型是这个数组中唯一存在的数据类型
Swift数组也可以使用[Element]这样简单的语法形式，苹果官方也是推荐简短的那一种。

### 长线一个空数组
~~~swift
var intArray = [Int]() //创建一个空数组
let int = intArray.count //count 方法返回数组的长度 intArray的类型会被自动推断为Int类型
~~~

### 创建带有默认值的数组
~~~swift
var threeDoubles = [Double](count: 3, repeatedValue: 0.0)//[0.0, 0.0, 0.0]
//还可以两个数组相加创建一个新数组前提是两个数组必须存储相同的类型
var anotherThreeDoubles = Array(count: 3, repeatedValue: 2.5)
var sixDoubles = threeDoubles + anotherThreeDoubles
//[0.0, 0.0, 0.0, 2.5, 2.5, 2.5]
~~~

### 用字面量构造数组
~~~swift
var shoppingList = ["Eggs", "Milk"]
~~~

### 访问和修改数组
* count 是数组的只读属性 Array.count 
* isEmpty 检查数组count属性是否为零的捷径 Array.isEmpty
* append(_:) 在数组尾添加新元素 Array.append(data)
* 也可以直接使用下标语法来获取对应索引的数据项 Array[index]
* insert(_:atIndex:)在某个具体索引值之前添加数据项 Array.insert(data, atIndex:0)
* 类似还有 removeAtIndex(_:)
* removeLast() 移除数组最后一个元素

**使用数组下标一定要确认下标是否越界**

### 数组的遍历
~~~swift
for item in Array {
	statements
}
~~~

如果我们同时需要每个数据项的值和索引值，可以使用enumerate()方法来进行数组遍历。enumerate()返回一个由每一个数据项索引值和数据值组成的元组。我们可以把这个元组分解成临时常量或者变量来进行遍历：

for (index, value) in Array.enumerate() {
	statements
}







