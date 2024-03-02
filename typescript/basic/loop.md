### 内容

### for
for loop
for..of loop
for..in loop

#### for loop
```
for (first expression; second expression; third expression ) {
    // statements to be executed repeatedly
}
for (let i = 0; i < 3; i++) {
  console.log ("Block statement execution no." + i);
}
```
#### for..of loop
```
let arr = [10, 20, 30, 40];

for (var val of arr) {
  console.log(val); // prints values: 10, 20, 30, 40
}

let str = "Hello World";

for (var char of str) {
  console.log(char); // prints chars: H e l l o  W o r l d
}
```
#### for..in loop
```
let arr = [10, 20, 30, 40];

for (var index1 in arr) {
  console.log(index1); // prints indexes: 0, 1, 2, 3
}
console.log(index1); //OK, prints 3 

for (let index2 in arr) {
  console.log(index2); // prints elements: 0, 1, 2, 3
}
```
### while

#### while
```
while (condition expression) {
    // code block to be executed
}
let i: number = 2;

while (i < 4) {
    console.log( "Block statement execution no." + i )
    i++;
}
//输出
Block statement execution no.2
Block statement execution no.3
```
#### do while
```
do {
// code block to be executed
}
while (condition expression);

let i: number = 2;
do {
    console.log("Block statement execution no." + i )
    i++;
} while ( i < 4)

//输出
Block statement execution no.2
Block statement execution no.3
```




