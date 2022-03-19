### 判断是否回文数

### 定义

```
正序（从左向右）和倒序（从右向左）读都是一样的整数
```
```
func isPalindrome(x int) bool {
    if(x<0 || (x>0&&x%10==0)){return false}
    y:=x
    c:=0
    for (y>0){
        c=c*10+y%10
        y=y/10
    }
    return c==x
}
```
题解：通过y/10相当于对于数字丛右到从循坏