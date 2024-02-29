## 内容

### 定义整型枚举

```
enum Direction {
    Up,
    Down,
    Left,
    Right,
  }
  console.log(Direction.Up,Direction.Right)

//类似于go的iota
const (
	Up = iota
	Down
	Left
	Right
)

enum Direction {
    Up=1,
    Down,
    Left=3,
    Right,
}
console.log(Direction.Up,Direction.Right)
//输出1和4

enum Direction {
    Up=0,
    Down=2,
    Left=4,
    Right=5,
  }
console.log(Direction.Up,Direction.Down,Direction.Left,Direction.Right)
也可以自由定义
```

### 定义字符串枚举
```
enum Direction {
  Up = "UP",
  Down = "DOWN",
  Left = "LEFT",
  Right = "RIGHT",
}
```
### 定义混合枚举
```
enum BooleanLikeHeterogeneousEnum {
  No = 0,
  Yes = "YES",
}
```



