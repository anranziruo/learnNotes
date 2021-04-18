## 内容

### 树的遍历
```
前序遍历:先访问当前节点，遍历左子树，再遍历右子树
中序遍历:先访问左子树，再访问当前节点，在遍历右子树
后序遍历:先左子树，再右子树，在当前节点
```
golang代码如下:
```
package main

import "fmt"

type BaseTree struct {
	Data  int
	Left  *BaseTree
	Right *BaseTree
}

func FrontendTrans(base *BaseTree) {
	if base == nil {
		return
	}
	fmt.Println(base)
	FrontendTrans(base.Left)
	FrontendTrans(base.Right)
}

func Middilerans(base *BaseTree) {
	if base == nil {
		return
	}
	FrontendTrans(base.Left)
	fmt.Println(base)
	FrontendTrans(base.Right)
}

func BackendTrans(base *BaseTree) {
	if base == nil {
		return
	}
	FrontendTrans(base.Left)
	FrontendTrans(base.Right)
	fmt.Println(base)
}

func main() {
	root := new(BaseTree)
	root.Data = 10
	left := new(BaseTree)
	left.Data = 4
	right := new(BaseTree)
	right.Data = 1
	root.Left = left
	root.Right = right
	FrontendTrans(root)
	Middilerans(root)
	BackendTrans(root)
}
```
