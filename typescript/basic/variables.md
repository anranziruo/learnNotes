## 内容

### 基础类型
```
string
number
boolean
any
array
```

### 声明
```
const hello:string = "hello"
console.log(hello)

let helloWord:string = "hello world"
console.log(helloWord)
helloWord = "123"
console.log(helloWord)

let inputNumbers:Number[] = [1,2,3]
inputNumbers.push(4)
console.log(inputNumbers)

let input : string| number| boolean = "nihao"
console.log(input)
input = 1
console.log(input)
input = false
console.log(input)

//此处声明就不合法了
input = [1,2,3]
```
备注: const是常量，一旦声明了，就不能二次更改值，let声明的变量可以二次更改

### 定义赋值
```
let age:number;
console.log(age)

//声明未赋值，输出undefined

//非法调用
let age:number=123+"b"
console.log(age)

//声明any类型就可以
let input:any = 123+"b"
console.log(input)
```

### 不同类型变量声明

#### number变量
```
let first:number = 123; // number 
let second: number = 0x37CF;  // hexadecimal
let third:number=0o377 ;      // octal
let fourth: number = 0b111001;// binary  

console.log(first);  // 123 
console.log(second); // 14287
console.log(third);  // 255
console.log(fourth); // 57 
```
#### string
```
let employeeName:string = 'John Smith'; 
//OR
let employeeName:string = "John Smith"
```

#### boolean
```
let isPresent:boolean = true;
```

#### array
```
let f1:string[] = ['Apple', 'Banana'];
// let f2: string[] = ['Apple',123, 'Banana'];//非法声明
let fAny: any[] = ['Apple',123, 'Banana'];
console.log(f1,fAny)

let a1: Array<string> = ['Apple', 'Orange', 'Banana'];
// let f2: Array<string> = ['Apple',123, 'Banana'];//非法声明
let aAny: Array<any> = ['Apple',123, 'Banana'];
console.log(f1,fAny)
```

#### tuples
```
初始化变量要按照声明顺序
let employee: [number, string] = [1, "Steve"];
let person: [number, string, boolean] = [1, "Steve", true];
let user: [number, string, boolean, number, string];// declare tuple variable
user = [1, "Steve", true, 20, "Admin"];// initialize tuple variable
```
#### union类型
```
let code: (string | number);
code = 123;   // OK
code = "ABC"; // OK
code = false; // Compiler Error
```

#### void
```
let nothing: void = undefined;
let num: void = 1; // Error

声明函数的返回值
function sayHi(): void { 
    console.log('Hi!')
} 

let speech: void = sayHi(); 
console.log(speech);
```
#### never
```
never类型认为情况永远不会发生
function throwError(errorMsg: string): never { 
            throw new Error(errorMsg); 
}
```

### 类型断言
```
let code: any = 123; 
let employeeCode = <number> code; 
console.log(typeof(employeeCode)); //Output: number
```

