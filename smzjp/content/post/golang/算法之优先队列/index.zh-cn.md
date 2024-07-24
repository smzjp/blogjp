---
original: true
title: 算法之优先队列
description: 最大堆，最小堆的使用
date: 2022-12-20
slug: 
image: 
tags:
    -  优先队列
categories:
    - Golang
---

优先队列用来求数组里面的前n个最大，最小值很有用。

#### 最小堆

最大最小堆是一种数据结构，用于在数据集合中快速找到最大或最小元素。堆是一种二叉树，其中每个父节点的值都大于或小于其两个子节点的值，具体来说，如果父节点的值大于子节点的值，则为最大堆；如果父节点的值小于子节点的值，则为最小堆。

```go
// This example demonstrates an integer heap built using the heap interface.
package main

import (
	"container/heap"
	"fmt"
)

// An IntHeap is a min-heap of ints.
type IntHeap []int

func (h IntHeap) Len() int           { return len(h) }
// 最大堆用 >
func (h IntHeap) Less(i, j int) bool { return h[i] < h[j] }
func (h IntHeap) Swap(i, j int)      { h[i], h[j] = h[j], h[i] }

func (h *IntHeap) Push(x any) {
	// Push and Pop use pointer receivers because they modify the slice's length,
	// not just its contents.
	*h = append(*h, x.(int))
}

func (h *IntHeap) Pop() any {
	old := *h
	n := len(old)
	x := old[n-1]
	*h = old[0 : n-1]
	return x
}

// This example inserts several ints into an IntHeap, checks the minimum,
// and removes them in order of priority.
func main() {
	h := &IntHeap{2, 1, 5}
	heap.Init(h)
	heap.Push(h, 3)
	fmt.Printf("minimum: %d\n", (*h)[0])
	for h.Len() > 0 {
		fmt.Printf("%d ", heap.Pop(h))
	}
}


```

#### 优先队列

优先队列与普通队列的区别：普通队列遵循先进先出的原则；优先队列的出队顺序与入队顺序无关，与优先级相关。优先队列可以使用队列的接口，只是在实现接口时，与普通队列有两处区别，一处在于优先队列出队的元素应该是优先级最高的元素，另一处在于队首元素也是优先级最高的元素。

```go
// This example demonstrates a priority queue built using the heap interface.
package main

import (
	"container/heap"
	"fmt"
)

// An Item is something we manage in a priority queue.
type Item struct {
	value    string // The value of the item; arbitrary.
	priority int    // The priority of the item in the queue.
	// The index is needed by update and is maintained by the heap.Interface methods.
	index int // The index of the item in the heap.
}

// A PriorityQueue implements heap.Interface and holds Items.
type PriorityQueue []*Item

func (pq PriorityQueue) Len() int { return len(pq) }

func (pq PriorityQueue) Less(i, j int) bool {
	// We want Pop to give us the highest, not lowest, priority so we use greater than here.
	return pq[i].priority > pq[j].priority
}

func (pq PriorityQueue) Swap(i, j int) {
	pq[i], pq[j] = pq[j], pq[i]
	pq[i].index = i
	pq[j].index = j
}

func (pq *PriorityQueue) Push(x any) {
	n := len(*pq)
	item := x.(*Item)
	item.index = n
	*pq = append(*pq, item)
}

func (pq *PriorityQueue) Pop() any {
	old := *pq
	n := len(old)
	item := old[n-1]
	old[n-1] = nil  // avoid memory leak
	item.index = -1 // for safety
	*pq = old[0 : n-1]
	return item
}

// update modifies the priority and value of an Item in the queue.
func (pq *PriorityQueue) update(item *Item, value string, priority int) {
	item.value = value
	item.priority = priority
	heap.Fix(pq, item.index)
}

// This example creates a PriorityQueue with some items, adds and manipulates an item,
// and then removes the items in priority order.
func main() {
	// Some items and their priorities.
	items := map[string]int{
		"banana": 3, "apple": 2, "pear": 4,
	}

	// Create a priority queue, put the items in it, and
	// establish the priority queue (heap) invariants.
	pq := make(PriorityQueue, len(items))
	i := 0
	for value, priority := range items {
		pq[i] = &Item{
			value:    value,
			priority: priority,
			index:    i,
		}
		i++
	}
	heap.Init(&pq)
  // Insert a new item and then modify its priority.
	item := &Item{
		value:    "orange",
		priority: 1,
	}
	heap.Push(&pq, item)
	pq.update(item, item.value, 5)

	// Take the items out; they arrive in decreasing priority order.
	for pq.Len() > 0 {
		item := heap.Pop(&pq).(*Item)
		fmt.Printf("%.2d:%s ", item.priority, item.value)
	}
}

```

#### 最大堆实现

```go
// 一个最大堆，一棵完全二叉树
// 最大堆要求节点元素都不小于其左右孩子
type Heap struct {
    // 堆的大小
    Size int
    // 使用内部的数组来模拟树
    // 一个节点下标为 i，那么父亲节点的下标为 (i-1)/2
    // 一个节点下标为 i，那么左儿子的下标为 2i+1，右儿子下标为 2i+2
    Array []int
}

// 初始化一个堆
func NewHeap(array []int) *Heap {
    h := new(Heap)
    h.Array = array
    return h
}

// 最大堆插入元素
func (h *Heap) Push(x int) {
    // 堆没有元素时，使元素成为顶点后退出
    if h.Size == 0 {
        h.Array[0] = x
        h.Size++
        return
    }

    // i 是要插入节点的下标
    i := h.Size

    // 如果下标存在
    // 将小的值 x 一直上浮
    for i > 0 {
        // parent为该元素父亲节点的下标
        parent := (i - 1) / 2

        // 如果插入的值小于等于父亲节点，那么可以直接退出循环，因为父亲仍然是最大的
        if x <= h.Array[parent] {
            break
        }

        // 否则将父亲节点与该节点互换，然后向上翻转，将最大的元素一直往上推
        h.Array[i] = h.Array[parent]
        i = parent
    }

    // 将该值 x 放在不会再翻转的位置
    h.Array[i] = x

    // 堆数量加一
    h.Size++
}

// 最大堆移除根节点元素，也就是最大的元素
func (h *Heap) Pop() int {
    // 没有元素，返回-1
    if h.Size == 0 {
        return -1
    }

    // 取出根节点
    ret := h.Array[0]

    // 因为根节点要被删除了，将最后一个节点放到根节点的位置上
    h.Size--
    x := h.Array[h.Size]  // 将最后一个元素的值先拿出来
    h.Array[h.Size] = ret // 将移除的元素放在最后一个元素的位置上

    // 对根节点进行向下翻转，小的值 x 一直下沉，维持最大堆的特征
    i := 0
    for {
        // a，b为下标 i 左右两个子节点的下标
        a := 2*i + 1
        b := 2*i + 2

        // 左儿子下标超出了，表示没有左子树，那么右子树也没有，直接返回
        if a >= h.Size {
            break
        }

        // 有右子树，拿到两个子节点中较大节点的下标
        if b < h.Size && h.Array[b] > h.Array[a] {
            a = b
        }

        // 父亲节点的值都大于或等于两个儿子较大的那个，不需要向下继续翻转了，返回
        if x >= h.Array[a] {
            break
        }

        // 将较大的儿子与父亲交换，维持这个最大堆的特征
        h.Array[i] = h.Array[a]

        // 继续往下操作
        i = a
    }

    // 将最后一个元素的值 x 放在不会再翻转的位置
    h.Array[i] = x
    return ret
}

```



