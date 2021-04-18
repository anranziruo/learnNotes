### 代码如下
```
package main

import "fmt"

type LinkNode struct {
	Data interface{}
	Next *LinkNode
}

type Node struct {
	headNode *LinkNode
}

func (s *Node) GetListNodeLen() int {
	head := s.headNode
	length := 0
	for head != nil {
		length += 1
		head = head.Next
	}
	return length
}

func (s *Node) IsEmpty() bool {
	if s.GetListNodeLen() > 0 {
		return false
	}
	return true
}

func (s *Node) Reverse() *LinkNode {
	newLinkNode := new(LinkNode)
	head := s.headNode
	for head != nil {
		newLinkNode, head, head.Next = head, head.Next, newLinkNode
	}
	return newLinkNode
}

func (s *Node) PrintNode(nod *Node) {
	node := nod.headNode
	for node != nil {
		fmt.Println(node.Data)
		node = node.Next
	}
}

func (s *Node) AddFirst(data interface{}) {
	newHead := &LinkNode{Data: data}
	newHead.Next = s.headNode
	s.headNode = newHead
}

func (s *Node) AddTail(data interface{}) *LinkNode {
	newTail := &LinkNode{Data: data}
	head := s.headNode
	for head.Next != nil {
		head = head.Next
	}
	head.Next = newTail
	return head
}

func main() {
	var headNode = new(LinkNode)
	headNode.Data = 1
	var nextNode = new(LinkNode)
	nextNode.Data = 2
	var next2Node = new(LinkNode)
	next2Node.Data = 3
	headNode.Next = nextNode
	nextNode.Next = next2Node
	var node = new(Node)
	node.headNode = headNode
	node.AddFirst(1)
	node.AddTail(4)
	node.PrintNode(node)
}
```