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
import java.util.Comparator;
import java.util.Iterator;
import java.util.NoSuchElementException;

/**
 * @author LFool
 * @create 2020-07-14 16:48
 */
public class MaxPQ<T> implements Iterable<T> {

    /**
     * store items at indices 1 to n
     */
    private T[] pq;
    /**
     * number of items on priority queue
     */
    private int n;
    /**
     * optional comparator
     */
    private Comparator<T> comparator;

    @SuppressWarnings("unchecked")
    public MaxPQ(int initCapacity) {
        pq = (T[]) new Object[initCapacity + 1];
        n = 0;
    }

    public MaxPQ() {
        this(1);
    }

    @SuppressWarnings("unchecked")
    public MaxPQ(int initCapacity, Comparator<T> comparator) {
        this.comparator = comparator;
        pq = (T[]) new Object[initCapacity + 1];
        n = 0;
    }

    public MaxPQ(Comparator<T> comparator) {
        this(1, comparator);
    }

    @SuppressWarnings("unchecked")
    public MaxPQ(T[] elements) {
        n = elements.length;
        pq = (T[]) new Object[elements.length + 1];
        System.arraycopy(elements, 0, pq, 1, elements.length);
        // time complexity : logN
        for (int k = n / 2; k >= 1; k--) {
            sink(k);
        }
        assert isMaxHeap();
    }

    public boolean isEmpty() {
        return n == 0;
    }

    public int size() {
        return n;
    }

    public T max() {
        if (isEmpty()) {
            throw new NoSuchElementException("Priority queue underflow");
        }
        return pq[1];
    }

    /**
     * helper function to double the size of the heap array
     */
    @SuppressWarnings("unchecked")
    private void resize(int capacity) {
        assert capacity > n;
        T[] temp = (T[]) new Object[capacity];
        if (n >= 0) {
            System.arraycopy(pq, 1, temp, 1, n);
        }
        pq = temp;
    }

    /**
     * Adds a new key to this priority queue.
     *
     * @param x the new key to add to this priority queue
     */
    public void insert(T x) {

        // double size of array if necessary
        if (n == pq.length - 1) {
            resize(2 * pq.length);
        }

        // add x, and percolate it up to maintain heap invariant
        pq[++n] = x;
        swim(n);
        assert isMaxHeap();
    }

    public T delMax() {
        if (isEmpty()) {
            throw new NoSuchElementException("Priority queue underflow");
        }
        T max = pq[1];
        exch(1, n--);
        sink(1);
        // to avoid loitering and help with garbage collection
        pq[n + 1] = null;
        if (n > 0 && n == (pq.length - 1) / 4) {
            resize(pq.length / 2);
        }
        assert isMaxHeap();
        return max;
    }

    /**
     * bottom to top
     * @param k the index of moving element
     */
    private void swim(int k) {
        while (k > 1 && less(k / 2, k)) {
            exch(k, k / 2);
            k = k / 2;
        }
    }

    /**
     * top to bottom
     * @param k the index of moving element
     */
    private void sink(int k) {
        while (2 * k <= n) {
            int child = 2 * k;
            if (child < n && less(child, child + 1)) {
                child++;
            }
            if (!less(k, child)) {
                break;
            }
            exch(k, child);
            k = child;
        }
    }

    @SuppressWarnings("unchecked")
    private boolean less(int i, int j) {
        if (comparator == null) {
            return ((Comparable<T>) pq[i]).compareTo(pq[j]) < 0;
        } else {
            return comparator.compare(pq[i], pq[j]) < 0;
        }
    }

    private void exch(int i, int j) {
        T swap = pq[i];
        pq[i] = pq[j];
        pq[j] = swap;
    }

    /**
     * is pq[1..n] a max heap?
     * @return isMaxHeap
     */
    private boolean isMaxHeap() {
        for (int i = 1; i <= n; i++) {
            if (pq[i] == null) {
                return false;
            }
        }
        for (int i = n + 1; i < pq.length; i++) {
            if (pq[i] != null) {
                return false;
            }
        }
        if (pq[0] != null) {
            return false;
        }
        return isMaxHeapOrdered(1);
    }

    /**
     * is subtree of pq[1..n] rooted at k a max heap?
     * @param k the root index of subtree
     * @return isMaxHeapOrdered
     */
    private boolean isMaxHeapOrdered(int k) {
        if (k > n) {
            return true;
        }
        int left = 2 * k;
        int right = 2 * k + 1;
        if (left <= n && less(k, left)) {
            return false;
        }
        if (right <= n && less(k, right)) {
            return false;
        }
        return isMaxHeapOrdered(left) && isMaxHeapOrdered(right);
    }

    @Override
    public Iterator<T> iterator() {
        return new HeapIterator();
    }

    private class HeapIterator implements Iterator<T> {

        // create a new pq
        private final MaxPQ<T> copy;

        // add all items to copy of heap
        // takes linear time since already in heap order so no keys move
        public HeapIterator() {
            if (comparator == null) {
                copy = new MaxPQ<>(size());
            } else {
                copy = new MaxPQ<>(size(), comparator);
            }
            for (int i = 1; i <= n; i++) {
                copy.insert(pq[i]);
            }
        }

        @Override
        public boolean hasNext() {
            return !copy.isEmpty();
        }

        @Override
        public T next() {
            if (!hasNext()) {
                throw new NoSuchElementException();
            }
            return copy.delMax();
        }
    }

    public static void main(String[] args) {
        MaxPQ<Integer> pq = new MaxPQ<>();

        pq.insert(5);
        pq.insert(3);
        pq.insert(17);
        pq.insert(10);
        pq.insert(84);
        pq.insert(19);
        pq.insert(6);
        pq.insert(22);
        pq.insert(9);

        Integer deMax = pq.delMax();
        System.out.println("The max val is " + deMax);

        for (int t : pq) {
            System.out.println(t);
        }
    }

}
```





