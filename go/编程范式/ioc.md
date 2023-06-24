### 反转控制
反转控制IoC – Inversion of Control 是一种软件设计的方法，其主要的思想是把控制逻辑与业务逻辑分享，不要在业务逻辑里写控制逻辑，这样会让控制逻辑依赖于业务逻辑，而是反过来，让业务逻辑依赖控制逻辑

#### 结构体的嵌套和方法重写
```
package main

type Drawer interface {
	Draw()
	Paint()
}

type Widget struct {
	X, Y int
}

type Label struct {
	Widget
	Text string
}

func (w *Widget) Draw() {
	println("Widget.Draw()")
}

func (w *Widget) Paint() {
	println("Widget.Paint()")
}

// 相当于重写
func (l *Label) Draw() {
	println("Label.Draw()")
}

func main() {
	var l Drawer = &Label{Widget{10, 10}, "State:"}
	l.Draw()
	l.Paint()
}
```
#### 反转控制
由控制逻辑 Undo 来依赖业务逻辑 IntSet，而是由业务逻辑 IntSet 依赖 Undo 。这里依赖的是其实是一个协议，这个协议是一个没有参数的函数数组。可以看到，这样一来，我们 Undo 的代码就可以复用了
```
package main

type Undo []func()

type UndoInset struct {
	undo Undo
	Data map[string]bool
}

func NewUndoInset() *UndoInset {
	return &UndoInset{
		Data: make(map[string]bool),
	}
}

func (u *UndoInset) Add(key string) {
	u.Data[key] = true
	u.undo = append(u.undo, func() { delete(u.Data, key) })
}

func (u *UndoInset) Undo() {
	for _, undo := range u.undo {
		undo()
	}
}

func (u *UndoInset) Contains(key string) bool {
	return u.Data[key]
}

```
