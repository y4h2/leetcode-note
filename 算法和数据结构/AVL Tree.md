
  
  

平衡二叉树: 任意一个节点, 左右子树高度差不超过1
1. 标注节点高度
2. 计算平衡因子

  
  

节点需要维护高度值. 在Add操作时, 更新高度.


什么时候维护平衡?

加入节点后, 向上回溯维护平衡性


左子树高度 - 右子树高度 > 1: 右旋转

![](__attachment/7181f1f2d3b40a03285f0a36a73d5451_MD5.png)

![](__attachment/d94e5da31be3a5c107a1727a92dc5d82_MD5.png)

  
  
  

倾斜

LL/RR

![](__attachment/7431c9d5aa018acfafc497826a2cd7d0_MD5.png)

  

LR

![](__attachment/a1d1aa41db8ea759f4da36316f4803b0_MD5.png)

对于LR, 先对x进行左旋转, 转化成LL

![](__attachment/8279464a1f5c28c7ebb38c95099769ed_MD5.png)

  
  

RL

![](__attachment/153ae47b87b9810c1e28e5783a148394_MD5.png)

对于RL, 先对x进行右旋转, 转化成RR

![](__attachment/cf2c34e6093ec33ae280487edf6091f8_MD5.png)

  
  
  

删除节点

删除节点直接删除指定数值的节点, 把该节点的右子树中的最小值节点作为后继节点放入原来节点的位置, 删除完之后按照add的逻辑去平衡二叉树

## go语言实现
```go
package main

type Node struct {
	key    int
	value  int
	height int
	left   *Node
	right  *Node
}

func NewNode(K, V int) *Node {
	return &Node{
		key:    K,
		value:  V,
		height: 1,
	}
}

type AVLTree struct {
	root *Node
}

func NewAVLTree() *AVLTree {
	return &AVLTree{}
}

func (this *AVLTree) Add(K, V int) {
	this.root = this.add(this.root, K, V)
}

func (this *AVLTree) add(node *Node, K, V int) *Node {
	if node == nil {
		return NewNode(K, V)
	}
	if K < node.key {
		node.left = this.add(node.left, K, V)
	} else if K > node.key {
		node.right = this.add(node.right, K, V)
	} else { // K == node.key
		node.value = V
	}

	// 更新height
	node.height = 1 + max(GetHeight(node.left), GetHeight(node.right))
	balanceFactor := GetBalanceFactor(node)

	// LL 倾斜发生在左子树的左侧节点
	// right rotate
	if balanceFactor > 1 && GetBalanceFactor(node.left) >= 0 {
		return this.rightRotate(node)
	}

	// RR 倾斜发生在右子树的右侧节点
	// left rotate
	if balanceFactor < -1 && GetBalanceFactor(node.right) <= 0 {
		return this.leftRotate(node)
	}

	// LR
	if balanceFactor > 1 && GetBalanceFactor(node.left) < 0 {
		node.left = this.leftRotate(node.left)
		return this.rightRotate(node)
	}

	// RL
	if balanceFactor < -1 && GetBalanceFactor(node.right) > 0 {
		node.right = this.rightRotate(node.right)
		return this.leftRotate(node)
	}

	return node
}

func (this *AVLTree) remove(node *Node, key int) *Node {
	if node == nil {
		return nil
	}

	var retNode *Node
	if key < node.key {
		node.left = this.remove(node.left, key)
		retNode = node
	} else if key > node.key {
		node.right = this.remove(node.right, key)
		retNode = node
	} else { // key == node.key
		if node.left == nil {
			rightNode := node.right
			node.right = nil

			retNode = rightNode
		} else if node.right == nil {
			leftNode := node.left
			node.left = nil

			retNode = leftNode
		} else {
			successor := this.min(node.right)
			successor.right = this.remove(node.right, successor.key)
			successor.left = node.left

			node.left, node.right = nil, nil
			retNode = successor
		}

	}

	if retNode == nil {
		return nil
	}

	// 更新height
	retNode.height = 1 + max(GetHeight(retNode.left), GetHeight(retNode.right))
	balanceFactor := GetBalanceFactor(retNode)

	// LL 倾斜发生在左子树的左侧节点
	// right rotate
	if balanceFactor > 1 && GetBalanceFactor(retNode.left) >= 0 {
		return this.rightRotate(retNode)
	}

	// RR 倾斜发生在右子树的右侧节点
	// left rotate
	if balanceFactor < -1 && GetBalanceFactor(retNode.right) <= 0 {
		return this.leftRotate(retNode)
	}

	// LR
	if balanceFactor > 1 && GetBalanceFactor(retNode.left) < 0 {
		retNode.left = this.leftRotate(retNode.left)
		return this.rightRotate(retNode)
	}

	// RL
	if balanceFactor < -1 && GetBalanceFactor(retNode.right) > 0 {
		retNode.right = this.rightRotate(retNode.right)
		return this.leftRotate(retNode)
	}

	return retNode
}

/*
right rotate
       y                x
      /  \            /   \
     x   T4          z     y
    / \       =>    / \   / \
   z  T3           T1 T2 T3 T4
  / \
 T1 T2

右旋之后返回新的根节点
*/
func (this *AVLTree) rightRotate(y *Node) *Node {
	x := y.left
	t3 := x.right

	x.right = y
	y.left = t3

	// update height
	y.height = max(GetHeight(y.left), GetHeight(y.right)) + 1
	x.height = max(GetHeight(x.left), GetHeight(x.right)) + 1

	return x
}

/*
right rotate
       y                 x
      /  \             /   \
     T1   x           y     z
         / \   =>    / \   / \
        T2  z       T1 T2 T3 T4
					 / \
					T3 T4

右旋之后返回新的根节点
*/
func (this *AVLTree) leftRotate(y *Node) *Node {
	x := y.right
	t2 := x.left

	x.left = y
	y.right = t2

	y.height = 1 + max(y.left.height, y.right.height)
	x.height = 1 + max(x.left.height, x.right.height)

	return x
}

func (this *AVLTree) max(node *Node) *Node {
	if node.right == nil {
		return node
	}

	return this.max(node.right)
}

func (this *AVLTree) min(node *Node) *Node {
	if node.left == nil {
		return node
	}

	return this.min(node.left)
}

func GetHeight(node *Node) int {
	if node == nil {
		return 0
	}

	return node.height
}

func GetBalanceFactor(node *Node) int {
	if node == nil {
		return 0
	}
	return GetHeight(node.left) - GetHeight(node.right)
}

func max(a, b int) int {
	if a < b {
		return b
	}
	return a
}

func abs(a int) int {
	if a < 0 {
		return -a
	}

	return a
}

```
