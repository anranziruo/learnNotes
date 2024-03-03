### 内容

### 普通函数
```
function display() {
    console.log("Hello TypeScript!");
}
display()

返回值为undefined

//设置返回值
function Sum(x: number, y: number) : number {
    return x + y;
}
```
### 多返回值函数
```
//interface
interface Person {
    name: string;
    age: number;
  }
  function returnPerson() :Person{
    return {name:"123",age:1}
  }
  let person = returnPerson()
  console.log(person.age,person.name)
//type
type Person = {
    name: string;
    age: number;
  };
  function returnPerson() :Person{
    return {name:"123",age:1}
  }
let person = returnPerson()
console.log(person.age,person.name)
//tuple
function getPersonInfo(): [string, number] {
    return ["John Doe", 30];
  }
let res:[string,number] = getPersonInfo()
console.log(res[0],res[1])
```

### Anonymous Function(匿名函数)
```
//不带参数的匿名函数
let greeting = function() {
  console.log("Hello TypeScript!");
};
greeting(); //Output: Hello Ty

//带参数的匿名函数
let Sum = function(x: number, y: number) : number
{
    return x + y;
}
Sum(2,3); // returns 5
```
### 函数参数

#### Optional Parameters(可选参数)
```
function Greet(greeting: string, name?: string ) : string {
  return greeting + ' ' + name + '!';
}

Greet('Hello','Steve');//OK, returns "Hello Steve!"
Greet('Hi'); // OK, returns "Hi undefined!"
//Greet('Hi','Bill','Gates'); 会编译报错参数不对
```

#### 默认参数
```
function Greet(name: string, greeting: string = "Hello") : string {
  return greeting + ' ' + name + '!';
}

Greet('Steve');//OK, returns "Hello Steve!"
Greet('Steve', 'Hi'); // OK, returns "Hi Steve!".
Greet('Bill'); //OK, returns "Hello Bill!"
```

#### Arrow Functions(箭头函数)
```
let sum = (x: number, y: number): number => {
  return x + y;
}
console.log(sum(10, 20)); 

let sum1 = (x: number, y: number) => x + y;
console.log(sum1(10, 20)); 
```
#### 函数重载
函数名相同,函数参数数量相同的函数可以声明多次
```
function add(a:string, b:string):string;

function add(a:number, b:number): number;

function add(a: any, b:any): any {
    return a + b;
}

add("Hello ", "Steve"); // returns "Hello Steve" 
add(10, 20); // returns 30 
```
#### 可变长参数
```
let Greet = (greeting: string, ...names: string[]) => {
    return greeting + " " + names.join(", ") + "!";
}
Greet("Hello", "Steve", "Bill"); // returns "Hello Steve, Bill!"
Greet("Hello");// returns "Hello !"
```
