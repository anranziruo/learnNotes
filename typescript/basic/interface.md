### 内容

#### interface as type
```
interface KeyPair {
    key: number;
    value: string;
}
let kv1: KeyPair = { key:1, value:"Steve" }; // OK
```

#### interface as function type
有点类似于golang的interface方法
```
interface KeyValueProcessor
{
    (key: number, value: string): void;
};

function addKeyValue(key:number, value:string):void { 
    console.log('addKeyValue: key = ' + key + ', value = ' + value)
}

function updateKeyValue(key: number, value:string):void { 
    console.log('updateKeyValue: key = '+ key + ', value = ' + value)
}
    
let kvp: KeyValueProcessor = addKeyValue;
kvp(1, 'Bill'); //Output: addKeyValue: key = 1, value = Bill 

kvp = updateKeyValue;
kvp(2, 'Steve'); //Output: updateKeyValue: key = 2, value = Steve 
```
#### interface as array
可以生成map和array
```
interface NumList {
  [index:number]:number
}

let numArr: NumList = [1, 2, 3];
numArr[0];
numArr[1];
console.log(numArr)

interface IStringList {
  [index:string]:string
}

let strArr : IStringList = {};
strArr["TS"] = "TypeScript";
strArr["JS"] = "JavaScript";
console.log(strArr)
```
#### interface可选属性
```
interface IEmployee {
  empCode: number;
  empName: string;
  empDept?:string;
}

let empObj1:IEmployee = {   // OK
  empCode:1,
  empName:"Steve"
}

let empObj2:IEmployee = {    // OK
  empCode:1,
  empName:"Bill",
  empDept:"IT"
}
```
#### 只读属性设置
```
interface Citizen {
  name: string;
  readonly SSN: number;
}

let personObj: Citizen  = { SSN: 110555444, name: 'James Bond' }

personObj.name = 'Steve Smith'; // OK
//personObj.SSN = '333666888'; // Compiler Error
```
#### 接口继承
```
interface IPerson {
    name: string;
    gender: string;
}

interface IEmployee extends IPerson {
    empCode: number;
}

let empObj:IEmployee = {
    empCode:1,
    name:"Bill",
    gender:"Male"
}
```
#### 接口继承
此处类似于golang的结构体继承
```
interface IPerson {
  name: string;
  gender: string;
}

interface IEmployee extends IPerson {
  empCode: number;
}

let empObj:IEmployee = {
  empCode:1,
  name:"Bill",
  gender:"Male"
}
```
#### 类实现接口
```
interface IEmployee {
    empCode: number;
    name: string;
    getSalary:(empCode: number) => number;
}

class Employee implements IEmployee { 
    empCode: number;
    name: string;

    constructor(code: number, name: string) { 
        this.empCode = code;
        this.name = name;
    }

    getSalary(empCode:number):number { 
        return 20000;
    }
}

let emp = new Employee(1, "Steve");
console.log(emp.getSalary(1))
```