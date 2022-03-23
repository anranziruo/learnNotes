### N叉树的前序遍历

```
给定一个n叉树的根节点root,返回其节点值的前序遍历
```
示例代码如下:
```
func preorder(root *Node) []int {
    var res []int
    var forder func(r *Node)
    forder = func(r *Node){
        if r == nil{
            return
        }
        res = append(res,r.Val)
        if r.Children!=nil{
            for _,v:=range r.Children{
                forder(v)
            }
        }
    }
    forder(root)
    return res
}
```

### N叉树的后序遍历
```
给定一个n叉树的根节点root ，返回其节点值的后序遍历 。
```
示例代码如下:
```
func postorder(root *Node) []int {
    var res []int
    var forder func(r *Node)
    forder = func(r *Node){
        if r == nil{
            return
        }
        for _,val:=range r.Children{
            forder(val)
        }
        res = append(res,r.Val)
    }
    forder(root)
    return res
}
```