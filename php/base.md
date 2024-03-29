### 基础内容

### oop
```
oop是面向对象编程,面向对象编程是一种计算机编程架构,OOP 的一条基本原则是计算机程序是由单个能够起到子程序作用的单元或对象组合而成。
三大特性:
1、封装性：也称为信息隐藏，就是将一个类的使用和实现分开，只保留部分接口和方法与外部联系，或者说只公开了一些供开发人员使用的方法。于是开发人员只 需要关注这个类如何使用，而不用去关心其具体的实现过程，这样就能实现MVC分工合作，也能有效避免程序间相互依赖，实现代码模块间松藕合。
2、继承性：就是子类自动继承其父级类中的属性和方法，并可以添加新的属性和方法或者对部分属性和方法进行重写。继承增加了代码的可重用性。PHP只支持单继承，也就是说一个子类只能有一个父类。
3、多态性：子类继承了来自父级类中的属性和方法，并对其中部分方法进行重写。于是多个子类中虽然都具有同一个方法，但是这些子类实例化的对象调用这些相同的方法后却可以获得完全不同的结果，这种技术就是多态性。多态性增强了软件的灵活性。
```

### php编译过程
```
PHP是解析型高级语言，事实上从Zend内核的角度来看PHP就是一个普通的C程序，它有main函数，我们写的PHP代码是这个程序的输入，然后经过内核的处理输出结果，内核将PHP代码"翻译"为C程序可识别的过程就是PHP的编译。
只是PHP代码没有编译成机器码，而是解析成了若干条opcode数组，每条opcode就是C里面普通的struct，含义对应C程序的机器指令，执行的过程就是引擎依次执行opcode
```
### 垃圾回收
```
1.以php的引用计数机制为基础（php5.3以前只有该机制）
2同时使用根缓冲区机制，当php发现有存在循环引用的zval时，就会把其投入到根缓冲区，当根缓冲区达到配置文件中的指定数量后，就会进行垃圾回收，以此解决循环引用导致的内存泄漏问题（php5.3开始引入该机制）
```
### 常用函数总结
```
1.echo(),print(),print_r()的区别
echo是PHP语句
print是打印简单的变量
print_r是打印复杂的变量
2.include和require的区别是什么?
require是无条件包含也就是如果一个流程里加入require,无论条件成立与否都会先执行require
include有返回值，而require没有(可能因为如此require的速度比include快)
require在运行前载入
include在运行时载入
require_once
include_once
包含文件不存在或者语法错误的时候require是致命的错误终止执行,include不是
3.isset()与empty()的区别
isset：检测变量是否设置
empty:判断值为否为空
```
### cookie和session的区别
```
1.存储位置：session存储于服务器，cookie存储于浏览器
2.安全性：session安全性比cookie高
3.session为‘会话服务’，在使用时需要开启服务，cookie不需要开启，可以直接用
```
### 常用框架总结
```
laravel篇
服务提供者:服务提供者是所有 Laravel 应用程序引导启动的中心, Laravel 的核心服务器、注册服务容器绑定、事件监听、中间件、路由注册以及我们的应用程序都是由服务提供者引导启动的。
IoC 容器:
IoC(Inversion of Control)译为 「控制反转」，也被叫做「依赖注入」(DI)。什么是「控制反转」?对象 A 功能依赖于对象 B，但是控制权由对象 A 来控制，控制权被颠倒，所以叫做「控制反转」，而「依赖注入」是实现 IoC 的方法，就是由 IoC 容器在运行期间，动态地将某种依赖关系注入到对象之中。
Laravel中通过控制反转的方式可以解决相同的调用方式，但可以采取不同的实现方式。这样我们就不用更改业务逻辑中的代码，就能更换不同的解决问题的方式。
Facades:
Facades（一种设计模式，通常翻译为外观模式）提供了一个"static"（静态）接口去访问注册到 IoC 容器中的类。
Contract:
Contract（契约）是 laravel 定义框架提供的核心服务的接口。Contract 和 Facades 并没有本质意义上的区别，其作用就是使接口低耦合、更简单
Model Observers:
Model Observers 是 Laravel 的功能。它用于为模型建立事件监听器的群集。这些类的方法名称描述了 Eloquent 事件。Observers 类方法将模型作为参数接收
策略类包括 Laravel 应用程序的授权逻辑。这些类用于特定的模型或资源。
```
### 设计模式
```
单例模式:
	单例模式是一种常用的软件设计模式。
	在它的核心结构中只包含一个被称为单例类的特殊类。通过单例模式可以保证系统中一个类只有一个实例而且该实例易于外界访问，从而方便对实例个数的控制并节约系统资源。
	应用场景：如果希望在系统中某个类的对象只能存在一个，单例模式是最好的解决方案。
工厂模式:
	工厂模式主要是为创建对象提供了接口。
	应用场景如下：
	a、 在编码时不能预见需要创建哪种类的实例。
	b、 系统不应依赖于产品类实例如何被创建、组合和表达的细节
观察者模式:
	观察者模式又被称作发布/订阅模式，定义了对象间一对多依赖，当一个对象改变状态时，它的所有依赖者都会收到通知并自动更新。	
迭代器模式。
	迭代器模式提供一种方法顺序访问一个聚合对象中各个元素，而又不暴露该对象的内部表示	
适配器模式
	把一个类的接口变换成客户端所期待的另一种接口，从而使原本因接口不匹配而无法在一起工作的两个类能够在一起工作。	
```

### 传值和传引用
```
按值传递：函数范围内对值的任何改变在函数外部都会被忽略
按引用传递：函数范围内对值的任何改变在函数外部也能反映出这些修改
优缺点：按值传递时，php必须复制值。特别是对于大型的字符串和对象来说，这将会是一个代价很大的操作。按引用传递则不需要复制值，对于性能提高很有好处。
```
### private,protected,public
```
private : 私有成员, 在类的内部才可以访问。
protected : 保护成员，该类内部和继承类中可以访问。
public : 公共成员，完全公开，没有访问限制。
```
### 功能
```
在PHP中error_reporting这个函数有什么作用？
设置PHP的报错级别并返回当前级别。
```
## 基础数据结构
```
A、堆是程序运行期间动态分配的内存空间，你可以根据程序的运行情况确定要分配的堆内存的大小；
B、栈是编译期间就分配好的内存空间，因此你的代码中必须就栈的大小有明确的定义。
```
### smarty
```
1.它分离了逻辑代码和外在的显示,提供了一种易于管理和使用的方法,用来将混杂的php逻辑代码与html代码进行分离
2.smarty设定了缓存参数以后，第一次运行时候会把模板打开，在php替换里面值的时候把读取的html和php部分重新生成一个临时的php文件，这样就省去了每次打开都重新读取html了。如果修改了模板，只要重新刷下就行了
```
### larvel用到的设计模式
```
1：工厂模式
例如：Auth::user()
此处Auth这个类就是工厂中的方法，Auth是注册树中的别名。
好处：
类似于函数的封装，使对象有一个统一的生成（实例化）入口。当我们对象所对应的类的类名发生变化的时候，我们只需要改一下工厂类类里面的实例化方法即可。
2：单例模式
好处：
对象不可外部实例化并且只能实例化一次，节省资源。
声明一个类的私有或者保护的静态变量，构造方法声明为私有（不允许外部进行new操作），如果不存在则实例化它，然后返回，如果存在则直接返回。
3：注册树模式
使用：
config/app里的aliases数组便是一个注册树
好处：
注册树模式就是使用数组结构来存取对象，工厂方法只需要调用一次（可以放到系统环境初始化这样的地方），以后需要调用该对象的时候直接从注册树上面取出来即可，不需要再调用工厂方法和单例模式。
$alias表示别名，自己设定
在工厂模式中添加
Register::set(‘db1’,$db);
其他任何地方调用只需要调用注册器读取即可
Register::$objects[‘db1’];
4：适配器模式
将不同工具的不同函数接口封装成统一的API，方便调用。如：mysql，mysqli，PDO。
实现：在接口类里面申明统一的方法体，再让不同的类去实现这个接口，和重写其抽象方法。
5:策略模式
好处：
将一组特定的行为和算法封装成类，以适应某些特定的上下文环境，将逻辑判断和具体实现分离，实现了硬编码到解耦，并可实现IOC、依赖倒置、反转控制。
实现：
1.定义一个策略接口文件(UserStrategy.php)，定义策略接口，声明策略
2.定义具体类(FemaleUserStrategy.php，MaleUserStrategy.php)，实现策略接口，重写策略方法
6：数据对象映射模式
好处：将对象和数据存储映射起来，对一个对象的操作会映射为对数据存储的操作，这也是ORM的实现机制。
7:观察者模式
使用：
Event::fire(new /event);
好处：
当一个对象状态发生改变时，依赖它的对象全部会收到通知并自动更新，实现低耦合，非侵入式的通知与更新机制。
(PS:有关abstract和interface：http://blog.csdn.net/sunlylorn/article/details/6124319)
8：原型模式
与工厂模式类似，用于创建对象，不同在于：原型模式是先创建好一个原型对象，再通过clone原型对象来创建新的对象，原型模式适用于大对象的创建，仅需要内存拷贝即可。
9：装饰器模式
若要修改或添加一个类的功能，传统的方式是写一个子类继承它，并重新实现类的方法。装饰器模式仅需在运行时添加一个装饰器对象即可动态的添加或修改类的功能。
10:迭代器模式
在不需要了解内部实现的前提下，遍历一个聚合对象的内部元素，相对于传统编程方式它可以遍历元素所需的操作。
11:代理模式
在客户端与实体之间建立一个代理对象，客户端对实体进行操作全部委派给代理对象，隐藏实体的具体实现细节（slave读库与master写库分离）。代理对象还可以与业务代码分离，部署到另外的服务器，业务代码中通过PRC来委派任务。
```