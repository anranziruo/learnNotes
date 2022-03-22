### 二叉树的层序遍历

题目要求
```
二叉树的根节点root ，返回其节点值的层序遍历 （即逐层地,从左到右访问所有节点）
```
解法1：
```
func levelOrder(root *TreeNode) [][]int {
	var ret [][]int
	if root == nil {
		return ret
	}
	q := []*TreeNode{root}
	for i := 0; len(q) > 0; i++ {
		var p []*TreeNode
		ret = append(ret, []int{})
		for j := 0; j < len(q); j++ {
			node := q[j]
			ret[i] = append(ret[i], node.Val)
			if node.Left != nil {
				p = append(p, node.Left)
			}
			if node.Right != nil {
				p = append(p, node.Right)
			}
		}
		q = p
	}
	return ret
}
```
解法2：基于dfs的递归算法
```
func levelOrder(root *TreeNode) [][]int {
    var ret [][]int
    var dfOrder func(r *TreeNode, level int)
	dfOrder = func(r *TreeNode, level int) {
        if r == nil{
            return 
        }
        if level == len(ret){
            ret = append(ret, []int{})
        }
		ret[level] = append(ret[level], r.Val)
		dfOrder(r.Left, level+1)
		dfOrder(r.Right, level+1)
	}
	dfOrder(root, 0)
    return ret
}
```

#### 二叉树的前序遍历，中序遍历，后序遍历

前序，中序，后序遍历的代码 
```
func frontendOrder(root *TreeNode) (res []int) {
	var forder func(r *TreeNode) //匿名函数
	forder = func(r *TreeNode) {
		if r == nil {
			return
		}
        //前序遍历 => 根左右
		res = append(res, r.Val)
		forder(r.Left)
		forder(r.Right)
        //中序遍历 => 左根右
        forder(r.Left)
		res = append(res, r.Val)
		forder(r.Right)
        //后序遍历 => 左右根
        forder(r.Left)
		forder(r.Right)
        res = append(res, r.Val)
	}
	forder(root)
	return res
}
```
#### 二叉树的根深度的计算
```
func maxDepth(root *TreeNode) int {
     if root == nil{return 0}
     leftInt:=maxDepth(root.Left)+1
     RightInt:=maxDepth(root.Right)+1
     resInt:=leftInt
     if RightInt>leftInt{
         resInt = RightInt
     }
     return resInt
}
```
