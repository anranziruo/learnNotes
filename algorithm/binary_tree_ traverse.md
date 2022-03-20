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
			if node.Right != nil {
				p = append(p, node.Right)
			}
			if node.Left != nil {
				p = append(p, node.Left)
			}
		}
		q = p
	}
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
