# SnapKit Swift自动布局框架
### 链式语法 DSL (Domain-specific language)
****
**基于官方文档翻译**

# 系统要求
* iOS 7.0+/OS X 10.9+
* Swift 2.0
* Xcode 7.0+

> 虽然SnapKit声明只支持iOS 7.0 和 OS X 10.9之后的平台，但是SnapKit的开发者依然兼顾一些历史遗留的平台，为此你需要手动把SnapKit的源文件整合到你的工程中。想获取更多关于历史遗留平台的信息请移步到 [Legacy Platforms](http://snapkit.io/legacy-platforms/)

# (安装)Installing
要享用SnapKit带来的便利，首先需要把SnapKit安装到你的项目中，SnapKit支持三种整合方式。

### Cocoapods
CocoaPods是一个iOS和OS X平台下的依赖管理工具

CocoPods 0.36 添加了对Swift和嵌入式框架的支持，通过以下命令安装

~~~sh
$ gem install cocoapods
~~~

通过CocoaPods将SnapKit整合到你的工程里，在你的Podfile加入以下声明：

~~~sh
source 'https://github.com/CocoaPods/Specs.git'
platform :ios, '8.0'
use_frameworks!

pod 'SnapKit', '~> 0.15.0'
~~~

之后执行如下命令:

~~~sh
$ pod install
~~~

### Carthage

Carthage 是一个自动将框架添加到你的程序中的分散(decentralized这个不知道咋翻译好)依赖管理工具。

可以通过[Homebrew](http://brew.sh/)安装carthage

~~~sh
$ brew update
$ brew install carthage
~~~

在cartfile加入以下声明，就可以将SnapKit添加到你的项目中

~~~swift
github "SnapKit/SnapKit" >= 0.21.0
~~~

### Embedded FrameWork(嵌入式框架)

* 添加SnapKit作为一个子模块，可以在终端下，执行下面命令，首先进入项目的根目录

~~~sh
$ git submodule add https://github.com/SnapKit/SnapKit.git
~~~
**这个以后再填坑**

# 使用(Usage)

SnapKit被设计的非常易用，下面的列子展示了如何使用SnapKit,为box添加相对于父视图的约束。

~~~swift
let box = UIView()
superview.addSubView(box)

box.snp_makeConstraints { (make) -> Void in
    make.top.equalTo(superview).offset(20)
    make.left.equalTo(superview).offset(20)
    make.bottom.equalTo(superview).offset(-20)
    make.right.equalTo(superview).offset(-20)
}
~~~

还可以用下面这种更简短的方式

~~~swift
let box = UIView()
superview.addSubview(box)

box.snp_makeConstraints { (make) -> Void in
    make.edges.equalTo(superview).inset(UIEdgeInsetsMake(20, 20, 20, 20))
}
~~~

不仅仅是为了缩短了代码的长度，为了增加约束的可读性SnapKit也十分关心下面这些重要的步骤。

* 选择最优的父视图设置约束。
* 持续跟踪被添加的约束，因此这些约束可以被很容易的移除
* 确保在适当的视图上调用`setTranslatesAutoresizingMaskIntoConstraints(false)`

# SnapKit 创建约束不仅仅局限于相等(equal)

>`.equalTo` 等价于 NSLayoutRelation.Equal(等于)
>`.lessThanOrEqualTo` 等价于NSLayoutRelation.GreaterThanOrEqual(小于等于)
>`.greaterThanOrEqualTo` 等价于NSLayoutRelation.GreaterThanOrEqual(大于等于)

以上三个同级创建约束的方法可以接受以下任何一个参数
### 1. 视图属性(ViewAttribute)
`make.centerX.lessThanOrEqualTo(view2.snp_left)`

|  viewAttribute  |  NSLayoutAttribute         |
| :--------------:| :------------------------: |
| view.snp_left   | NSLayoutAttribute.Left     |
| view.snp_right  | NSLayoutAttribute.Right    |
| view.snp_top    | NSLayoutAtttribute.Top     |
| view.snp_bottom | NSLayoutAttribute.Bottom   |
| view.snp_leading| NSLayoutAttribute.Leading  |
| view.snp_trailing| NSLayoutAttribute.Trailing|
| view.snp_width  |  NSLayoutAttribute.Width   |
| view.snp_height | NSLayoutAttribute.Height   |
| view.snp_centerX| NSLayoutAttribute.CenterX  |
| view.snp.centerY| NSLayoutAttribute.CenterY  |
| view.snp_baseline| NSLayoutAttribute.BaseLine|

### 2.UIView(iOS)/NSView(OX S)
>添加view.left大于等于label.left的约束:
~~~swift
//下面两行代码等价
make.left.greaterThanOrEqualTo(label)
make.left.greaterThanOrEqualTo(label.snp_left)
~~~

#### 3.严格的检查(Strict Checks) 
自动布局允许你将高度和宽度设置为常量，如果你想设置快读在一个区间里，你可以传递一个初始的平等快(**翻译有待推敲**)

~~~swift
//width >= 200 && width <= 400
make.width.greaterThanOrEqualTo(200)
make.width.greaterThanOrEqualTo(400)
~~~

你可以用其他语句和结构来构建你的约束，象下面一样：

~~~swift
make.top.equalTo(42)
make.height.equalTo(20)
make.size.equalTo(CGSizeMake(50, 100))
make.edges.equalTo(UIEdgeInsetsMake(10, 0, 10, 0))
make.left.equalTo(view).offset(UIEdgeInsetsMake(10, 0, 10, 0))
~~~

### 学习确定约束的优先级

>`.prorith` 让你指定一个确定的优先级
>`.priorityHigh` 高优先级 等价于 UILayoutPriority.DefaultHigh
>`.priorityMedium` 中优先级介于 高和低之间
>`.prorithLow` 低优先级 等价于 UILayoutPrority.DefaultLow

优先级可以放在链式约束的后面像下面一样:

~~~swift
make.left.greaterThanOrEqualTo(label.snp_left).priorityLow()
make.top.equalTo(label.snp_top).priority(600)
~~~

### 排版，排版，排版(外国大神说话看不太懂啊)
SnapKit 提供了一些便利的方法可以让你一次创建多个布局约束

#### edges
~~~swift
//使 top, left, bottom, right 等去 view2
make.edges.equalTo(view2)

//使 top = superView.top + 5, left = superView.left + 10,
// bottom = superView.bottom -15, right = superView.right -20

make.edges.equalTo(super).inset(UIEdgeInsetsMake(5, 10, 15, 20))
~~~
