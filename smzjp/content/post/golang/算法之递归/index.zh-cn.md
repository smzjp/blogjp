---
original: true
title: 算法之递归
description: 边界
date: 2024-04-09
slug: 
image: 
tags:
    - 递归
categories:
    - Golang
    - Swift
    - 算法
---



递归一般有两种思路，一种是递，另一种是归。递是传递的意思，从递归开始的根节点开始计算，把计算数值通过递归函数的参数传递给下一个子节点，比如是用前序遍历，先计算跟节点，再计算左子树（先计算左子树里面的根节点），最后计算右子树。归是回归的意思，从叶子节点开始计算，把计算数值通过递归函数的返回参数返回给前一个父节点。也可以这么理解，递归算法都包含递和归的过程。如果是在递的过程中计算数值，就称为递的思想，如果是在归的过程中计算数值，就称为归的思想。

##### 例子1，leetcode 1026题

给定二叉树的根节点 `root`，找出存在于 **不同** 节点 `A` 和 `B` 之间的最大值 `V`，其中 `V = |A.val - B.val|`，且 `A` 是 `B` 的祖先。

（如果 A 的任何子节点之一为 B，或者 A 的任何子节点是 B 的祖先，那么我们认为 A 是 B 的祖先）

1. 递的思想(摘自leetcode题解)

```swift
func maxAncestorDiff(root *TreeNode) (ans int) {
    var dfs func(*TreeNode, int, int)
    dfs = func(node *TreeNode, mn, mx int) {
        if node == nil {
            return
        }
        mn = min(mn, node.Val)
        mx = max(mx, node.Val)
        ans = max(ans, node.Val-mn, mx-node.Val)
        dfs(node.Left, mn, mx)
        dfs(node.Right, mn, mx)
    }
    dfs(root, root.Val, root.Val)
    return
}

```

1. 归的思想

```swift
	func maxAncestorDiff(_ root: TreeNode?) -> Int {
				var ans = 0
        func min(a:Int,b:Int) -> Int {
            if a > b {
                return b
            }
            return a
        }
        func max(a:Int,b:Int) -> Int {
            if a > b {
                return a
            }
            return b
        }
        func dfs(node: TreeNode?) -> (Int,Int) {
            if node == nil {
                return (Int.min,Int.max)
            }
    
            var (lmx,lmn) = dfs(node: node?.left)
            var (rmx,rmn) = dfs(node: node?.right)
            
            var mn = min(a: node!.val, b: lmn)
            mn = min(a: mn, b: rmn)

            var mx = max(a: node!.val, b: lmx)
            mx = max(a: mx, b: rmx)
            
            ans = max(a: ans, b: node!.val-mn)
            ans = max(a: ans, b: mx-node!.val)

            return (mx,mn)
        }
        dfs(node: root)
       return ans

    }
```

##### 例子2 leetcode 98题

给你一个二叉树的根节点 `root` ，判断其是否是一个有效的二叉搜索树。

**有效** 二叉搜索树定义如下：

- 节点的左子树只包含小于 当前节点的数。
- 节点的右子树只包含 **大于** 当前节点的数。
- 所有左子树和右子树自身必须也是二叉搜索树。

1.递的思想

```swift
func isValidBST(_ root: TreeNode?) -> Bool {

				func maxV(a:Int64,b:Int64) -> Int64 {
            if a > b {
                return a
            }
            return b
        }
        
        func minV(a:Int64,b:Int64) -> Int64 {
            if a < b {
                return a
            }
            return b
        }
        
        func dfs(_ node: TreeNode) -> (Bool,Int64,Int64) {
            
            var max = Int64(node.val)
            var min = Int64(node.val)
            
            var left = true
            if let leftNode = node.left {
                var (leftFLag,leftMax,leftMin) = dfs(leftNode)
                if leftMin < min {
                    min = leftMin
                }
                left = leftFLag && (leftMax < node.val)
            }
                        
            var right = true
            if let rihtNode = node.right {
                var (rightFlag,rightMax,rightMin) = dfs(rihtNode)
                if rightMax > max {
                    max = rightMax
                }
                right = rightFlag && (rightMin > node.val)
            }
           
            if (left && right) == false {
                return (false,0,0)
            }
            
            return (left && right,max,min)
        }
        
        let (ans,_,_) = dfs(root!)
        
        return ans
    }
```

2.归的思想(摘自leetcode题解)

```go
	func dfs(node *TreeNode) (int, int) {
    if node == nil {
        return math.MaxInt, math.MinInt
    }
    lMin, lMax := dfs(node.Left)
    rMin, rMax := dfs(node.Right)
    x := node.Val
    if x <= lMax || x >= rMin { // 也可以在递归完左子树之后立刻判断，如果发现不是二叉搜索树，就不用递归右子树了
        return math.MinInt, math.MaxInt
    }
    return min(lMin, x), max(rMax, x)
}

func isValidBST(root *TreeNode) bool {
    _, mx := dfs(root)
    return mx != math.MaxInt
}

```

