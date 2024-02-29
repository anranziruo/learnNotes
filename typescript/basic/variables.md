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
