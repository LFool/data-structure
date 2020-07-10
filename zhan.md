# 栈

## 1. 栈的定义

**栈：**具有一定操作约束的线性表

* 只在一端（栈顶，Top）做 **插入**、**删除**
* 插入数据：**入栈（Push）**
* 删除数据：**出栈（Pop）**
* **先入后出**

## 2. 栈的抽象数据类型描述

**类型名称：**栈（ $$Stack$$ ）

**数据对象集：**一个有 0 个或多个元素的有穷线性表

**操作集：**长度为 $$MaxSize$$ 的栈 $$S \in Stack$$ ，栈元素 $$item \in ElementType$$ 

* `Stack CreateStack(int MaxSize)`：生成空栈，其最大长度为 ****$$MaxSize$$ ；
* `int IsFull(Stack S, int MaxSize)`：判断堆栈 $$S$$ 是否已满；
* `void Push(Stack S, ElementType item)`：将元素 $$item$$ 压入栈；
* `int IsEmpty(Stack S)`：判断栈 ****$$S$$ 是否为空；
* `ElementType Pop(Stack S)`：删除并返回栈顶元素。

## 3. 栈的顺序存储实现

栈的顺序存储结构通常由一个 **一维数据** 和一个记录 **栈顶** 元素位置的变量组成

```cpp
typedef struct SNode* Stack;
struct SNode {
    ElementType Data[MAXSIZE];
    int Top;
};
```

### 3.1 入栈

```cpp
void Push(Stack PtrS, ElementType item) {
    if (PtrS->Top == MAXSIZE - 1) {
        printf("栈满\n");
    } else {
        PtrS->Data[++(PtrS->Top)] = item;
    }
    return;
}
```

### 3.2 出栈

```cpp
ElementType Pop(Stack PtrS) {
    if (PtrS->Top == -1) {
        printf("栈空\n");
        return ERROR;
    } else {
        return PtrS->Data[(PtrS->Top)--];
    }
}
```

### 3.3 用一个数组实现两个栈

**思路：**从 **两头开始向中间生长**，当两个栈的 **栈顶指针相遇** 时，表示两个栈都满了

```cpp
#include <stdio.h>
#include <stdlib.h>

#define MAXSIZE 100
#define ElementType int
#define ERROR -1

struct DStack {
    ElementType Data[MAXSIZE];
    int Top1;  // 栈 1 的栈顶指针
    int Top2;  // 栈 2 的栈顶指针
} S;

void Push(struct DStack* PtrS, ElementType item, int Tag) {
    /* Tag 作为区分两个堆栈的标志，取值为 1 和 2 */
    if (PtrS->Top2 - PtrS->Top1 == 1) {
        printf("栈满\n");
        return;
    }
    if (Tag == 1) {
        PtrS->Data[++(PtrS->Top1)] = item;
    } else {
        PtrS->Data[--(PtrS->Top2)] = item;
    }
}

ElementType Pop(struct DStack* PtrS, int Tag) {
    /* Tag 作为区分两个堆栈的标志，取值为 1 和 2 */
    if (Tag == 1) {
        if (PtrS->Top1 == -1) {
            printf("栈1空\n");
            return NULL;
        } else {
            return PtrS->Data[(PtrS->Top1)--];
        }
    } else {
        if (PtrS->Top2 == MAXSIZE) {
            printf("栈2空\n");
            return NULL;
        } else {
            return PtrS->Data[(PtrS->Top2)++];
        }
    }
}

```

## 4. 栈的链式存储实现

