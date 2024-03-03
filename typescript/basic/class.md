## 内容

### class的基础概念

#### 构造函数
```
class Employee {

    empCode: number;
    empName: string;
    
    constructor(empcode: number, name: string ) {
        this.empCode = empcode;
        this.empName = name;
    }
}

let ep:Employee = new Employee(1,"CR7")
console.log(ep.empCode,ep.empName)

//无构造函数的类
class Employee {

    empCode: number;
    empName: string;
}
let ep:Employee = new Employee()
console.log(ep.empCode,ep.empName)
```
#### 属性设置
```
public:所有公共成员可以在任何地方被访问，没有限制
private:只在当前类可以被看到
protected:只在子类和父类可以访问,实例化的类是不能访问到的
```
#### 类的继承
```
class Person {
    name: string;
    
    constructor(name: string) {
        this.name = name;
    }
}

class Employee extends Person {
    empCode: number;
    
    constructor(empcode: number, name:string) {
        super(name); //必须在构造函数里面调用super,才可以继承
        this.empCode = empcode;
    }
    
    displayName():void {
        console.log("Name = " + this.name +  ", Employee Code = " + this.empCode);
    }
}

let emp = new Employee(100, "Bill");
emp.displayName(); // Name = Bill, Employee Code = 100
```

#### 方法重载
```
class Car {
    name: string;
        
    constructor(name: string) {
        this.name = name;
    }
    
    run(speed:number = 0) {
        console.log("A " + this.name + " is moving at " + speed + " mph!");
    }
}

class Mercedes extends Car {
    
    constructor(name: string) {
        super(name);
    }
    
    run(speed = 150) {
        console.log('A Mercedes started')
        super.run(speed);
    }
}

class Honda extends Car {
    
    constructor(name: string) {
        super(name);
    }
    
    run(speed = 100) {
        console.log('A Honda started')
        super.run(speed);
    }
}

let mercObj = new Mercedes("Mercedes-Benz GLA");
let hondaObj = new Honda("Honda City")

mercObj.run();  // A Mercedes started A Mercedes-Benz GLA is moving at 150 mph!
hondaObj.run(); // A Honda started A Honda City is moving at 100 mph!
```
#### 抽象类
An abstract class typically includes one or more abstract methods or property declarations. The class which extends the abstract class must define all the abstract methods
(一个类继承抽象类，必须要实现所有的抽象方法和属性)
```
abstract class Person {
    abstract name: string;

    //如果是抽象方法,则雇员类必须要实现void
    display(): void{
        console.log(this.name);
    }
}

class Employee extends Person { 
    name: string;
    empCode: number;
    
    constructor(name: string, code: number) { 
        super(); // must call super()
        
        this.empCode = code;
        this.name = name;
    }
}
let emp: Person = new Employee("James", 100);
emp.display(); //James
```
#### 静态属性和方法
静态属性和方法可以直接访问
```
class Circle {
    static pi: number = 3.14;
    
    static calculateArea(radius:number) {
        return this.pi * radius * radius;
    }
}
Circle.pi; // returns 3.14
Circle.calculateArea(5); // returns 78.5
```