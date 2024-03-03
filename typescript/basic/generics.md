## 内容

### 以any的方式
```
function getArray(items : any[] ) : any[] {
    return new Array().concat(items);
}

let myNumArr = getArray([100, 200, 300]);
let myStrArr = getArray(["Hello", "World"])
console.log(myNumArr,myStrArr)
```
### 使用T的模式
```
function getArray<T>(items : T[] ) : T[] {
    return new Array<T>().concat(items);
}

//使用的时候声明了类型，
let myNumArr = getArray<number>([100, 200, 300]);
let myStrArr = getArray<string>(["Hello", "World"]);

myNumArr.push(400); // OK
myStrArr.push("Hello TypeScript"); // OK

//myNumArr.push("Hi"); // Compiler Error
//myStrArr.push(500); // Compiler Error
```

### 多个参数使用泛型
```
function displayType<T, U>(id:T, name:U): void { 
    console.log(typeof(id) + ", " + typeof(name));  
  }
  
displayType<number, string>(1, "Steve"); // number, string
```

### 泛型的约束
```
class Person {
    firstName: string;
    lastName: string;

    constructor(fname:string,  lname:string) { 
        this.firstName = fname;
        this.lastName = lname;
    }
}

function display<T extends Person>(per: T): void {
    console.log(`${ per.firstName} ${per.lastName}` );
}
var per = new Person("Bill", "Gates");
display(per); //Output: Bill Gates

//display("Bill Gates");//Compiler Error
```
