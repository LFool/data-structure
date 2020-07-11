# 队列

## 1. 队列的定义

**队列：**具有一定操作约束的线性表

* 插入和删除操作：只能在 **一端插入**，而在 **另一端删除**
* 插入数据：**入队列（AddQ）**
* 删除数据：**出队列（DeleteQ）**
* **先来先服务、先进先出（FIFO）**

## 2. 队列的抽象数据类型描述

**类型名称：**队列（ $$Queue$$ ）

**数据对象集：**一个有 0 个或多个元素的有穷线性表

**操作集：**长度为 $$MaxSize$$ 的队列 $$Q \in Queue$$ ，栈元素 $$item \in ElementType$$ 

* `Queue CreateQueue(int MaxSize)`：生成空队列，其最大长度为 ****$$MaxSize$$ ；
* `int IsFullQ(Queue Q, int MaxSize)`：判断队列 $$Q$$ 是否已满；
* `void AddQ(Queue Q, ElementType item)`：将元素 $$item$$ 插入队列Q中；
* `int IsEmpty(Queue Q)`：判断栈 ****$$Q$$ 是否为空；
* `ElementType DeleteQ(Queue Q)`：将队头数据元素从队列中删除并返回。

## 3. 队列的顺序存储实现

队列的顺序存储结构通常由一个 **一维数组** 和一个记录队列头元素位置的变量 **front** 以及一个记录队列尾元素位置的变量 **rear** 组成。

本次顺序存储实现，为了最大的利用内存空间，我们采用 **循环队列**

循环队列存在一个很大的问题，**判断队列空或满的条件是什么？**如果判断条件为 **front** 和 **rear** 是否重合，这样会导致 **满和空** 的条件一样。

**处理方法：**空一个空间不存数据

* 当 $$front = rear$$ ，队列空
* 当 $$(rear + 1) \% MAXSIZE == front $$  ，队列满

```cpp
typedef struct QNode* Queue;
struct QNode {
    ElementType Data[MAXSIZE];
    int rear;   // 指向当前最后一个元素
    int front;  // 指向第一个元素的前一个位置
};
```

### 3.1 入队列

```cpp
void AddQ(Queue PtrQ, ElementType item) {
    if ((PtrQ->rear + 1) % MAXSIZE == PtrQ->front) {
        printf("队列满\n");
        return;
    }
    PtrQ->rear = (PtrQ->rear + 1) % MAXSIZE;
    PtrQ->Data[PtrQ->rear] = item;
}
```

### 3.2 出队列

```cpp
ElementType DeleteQ(Queue PtrQ) {
    if (PtrQ->rear == PtrQ->front) {
        printf("队列空\n");
        return ERROR;
    }
    PtrQ->front = (PtrQ->front + 1) % MAXSIZE;
    return PtrQ->Data[PtrQ->front];
}
```

## 4. 队列的链式存储实现

队列的链式存储结构可以用一个 **单链表** 实现，插入和删除操作分别在链表的两头进行。队列指针 **front** 指向链表左边，队列指针 **rear** 指向链表右边。

```cpp
struct Node {
    ElementType Data;
    struct Node* next;
};
struct QNode {
    struct Node* rear;
    struct Node* front;
};
typedef struct QNode* Queue;
```

### 4.1 入队列

```cpp
void AddQ(Queue PtrQ, ElementType item) {
    struct Node* NewCell;
    NewCell->Data = item;
    NewCell->next = NULL;

    PtrQ->rear->next = NewCell;
    PtrQ->rear = NewCell;
}
```

### 4.2 出队列

```cpp
ElementType DeleteQ(Queue PtrQ) {
    struct Node* FrontCell;
    ElementType FrontElem;

    if (PtrQ->front == NULL) {
        printf("队列空\n");
        return ERROR;
    }

    FrontCell = PtrQ->front;
    if (PtrQ->front == PtrQ->rear) {      // 若队列中只有一个元素
        PtrQ->front = PtrQ->rear = NULL;  // 删除后队列置为空
    } else {
        PtrQ->front = PtrQ->front->next;
    }
    FrontElem = FrontCell->Data;
    free(FrontCell);
    return FrontElem;
}
```

