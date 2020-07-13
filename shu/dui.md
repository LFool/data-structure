# 堆

> 学习堆之前先了解一下优先队列

## 1. 优先队列的定义

**优先队列（Priority Queue）：**特殊的“**队列**”，取出元素的顺序是 依照元素的**优先权（关键字）**大小，而不是元素进入队列的先后顺序。

## 2. 优先队列的实现

可以采用数组或者链表实现，但是效率不怎么好

采用**完全二叉树**结构组织数据实现优先队列

## 3. 堆和优先队列的关系

优先队列的实现有很多方法，堆只是其中一种

## 4. 堆的特性

**结构性：**用数组表示的完全二叉树

**有序性：**任一结点的关键字是其子树所有结点的最大值（或最小值）

* **最大堆（MaxHeap）**，也称 **大顶堆**：最大值
* **最小堆（MinHeap）**，也称 **小顶堆**：最小值

## 5. 堆的抽象数据类型描述

**类型名称：**最大堆（ $$MaxHeap$$ ）

**数据对象集：**完全二叉树，每个结点的元素值 不小于 其子结点的元素值

**操作集：**最大堆  $$H \in MaxHeap$$ ，元素 $$item \in ElementType$$ ，主要操作有：

* `MaxHeap Create(int MaxSize)`：创建一个空的最大堆；
* `Boolean IsFull(MaxHeap H)`：判断最大堆 $$H$$ 是否已满；
* `void Insert(MaxHeap H, ElementType item)`：将元素 $$item$$ 插入最大堆 $$H$$ ；
* `Boolean IsEmpty(MaxHeap H)`：判断最大堆 ****$$H$$ 是否为空；
* `ElementType DeleteMax(MaxHeap H)`：删除并返回 $$H$$ 中最大元素（高优先级）。

## 6. 最大堆的操作

```java
public class MaxHeap {

    private int[] heap;
    private int size;
    private int maxsize;

    public MaxHeap(int maxsize) {
        this.maxsize = maxsize;
        this.size = 0;
        heap = new int[this.maxsize + 1];
        heap[0] = Integer.MAX_VALUE;
    }

    private int parent(int pos) {
        return pos / 2;
    }

    private int leftChild(int pos) {
        return 2 * pos;
    }

    private int rightChild(int pos) {
        return 2 * pos + 1;
    }

    private boolean isLeaf(int pos) {
        return pos > size / 2 && pos <= size;
    }

    private void swap(int fpos, int spos) {
        int tmp;
        tmp = heap[fpos];
        heap[fpos] = heap[spos];
        heap[spos] = tmp;
    }

    private void recMaxHeapify(int pos) {
        if (isLeaf(pos)) {
            return ;
        }
        if (heap[pos] < heap[leftChild(pos)] || heap[pos] < heap[rightChild(pos)]) {
            if (heap[leftChild(pos)] > heap[rightChild(pos)]) {
                swap(pos, leftChild(pos));
                recMaxHeapify(leftChild(pos));
            } else {
                swap(pos, rightChild(pos));
                recMaxHeapify(rightChild(pos));
            }
        }
    }

    private void maxHeapify(int pos) {
        int parent = pos;
        while (parent <= size / 2) {
            int child  = parent * 2;
            if (child != size && heap[child] < heap[child + 1]) {
                child++;
            }
            if (heap[parent] >= heap[child]) {
                break;
            }
            swap(parent, child);
            parent = child;
        }
    }

    public void insert(int element) {
        heap[++size] = element;

        int current = size;
        while (heap[current] > heap[parent(current)]) {
            swap(current, parent(current));
            current = parent(current);
        }
    }

    /**
     * Remove an element from max heap
     */
    public int extractMax() {
        int popped = heap[1];
        heap[1] = heap[size--];
        // recMaxHeapify(1);
        maxHeapify(1);
        return popped;
    }

    @Override
    public String toString() {

        StringBuilder str = new StringBuilder();
        str.append("MaxHeap{ \n");

        for (int i = 1; i <= size / 2; i++) {
            str.append("\t").
                    append("PARENT : ").append(heap[i]).
                    append(" LEFT CHILD : ").append(heap[2 * i]);
            if (2 * i + 1 <= size) {
                str.append(" RIGHT CHILD :").append(heap[2 * i + 1]);
            }
            str.append("\n");
        }
        str.append("}");

        return str.toString();
    }

    public static void main(String[] args) {
        MaxHeap maxHeap = new MaxHeap(15);
        maxHeap.insert(5);
        maxHeap.insert(3);
        maxHeap.insert(17);
        maxHeap.insert(10);
        maxHeap.insert(84);
        maxHeap.insert(19);
        maxHeap.insert(6);
        maxHeap.insert(22);
        maxHeap.insert(9);

        System.out.println(maxHeap.toString());
        System.out.println("The max val is " + maxHeap.extractMax());
        System.out.println(maxHeap.toString());
    }
}
```





