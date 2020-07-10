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

栈的链式存储结构实际上就是一个 **单链表**，叫做 **链栈**。插入和删除操作只能在链栈的栈顶进行。

指针 TOP 应该在 **链表的左边**，会比较方便实现。

```cpp
typedef struct SNode* Stack;
struct SNode {
    ElementType Data;
    struct SNode* next;
};
```

### 4.1 初始化

```cpp
Stack CreateStack() {
    /* 构建一个栈的头结点，返回指针 */
    Stack S;
    S = (Stack)malloc(sizeof(struct SNode));
    S->next = NULL;
    return S;
}
```

### 4.2 入栈

```cpp
void Push(ElementType item, Stack S) {
    /* 将元素 item 压入栈 S */
    struct SNode* TmpCell;
    TmpCell = (struct SNode*)malloc(sizeof(struct SNode));
    TmpCell->Data = item;
    TmpCell->next = S->next;
    S->next = TmpCell;
}
```

### 4.3 出栈

```cpp
ElementType Pop(Stack S) {
    /* 删除并返回栈 S 的栈顶元素 */
    struct SNode* FirstCell;
    ElementType TopElem;
    if (IsEmpty(S)) {
        printf("栈空\n");
        return;
    } else {
        FirstCell = S->next;
        S->next = FirstCell->next;
        TopElem = FirstCell->Data;
        free(FirstCell);
        return TopElem;
    }
}
```

### 4.4 判断是否为空

```cpp
int IsEmpty(Stack S) {
    /* 判断栈 S 是否为空，若为空函数返回整数 1，否则返回 0 */
    return (S->next == NULL);
}
```

## 5. 表达式求值

对于一个后缀表达式，很容易利用栈来求值，所以我们的目标就是把中缀表达式转换成后缀表达式。

**方法：**从头到尾读取 **中缀表达式的每个对象**，对不同对象按照不同的情况处理。

1. **运算数：**直接输出；
2. **左括号：**压入栈；
3. **右括号：**将 **栈顶的运算符弹出** 并 **输出**，**直到遇到左括号**（出栈，不输出）；
4. **运算符：**
   * 若 **优先级大于栈顶运算符** 时，则把它 **压栈**；
   * 若 **优先级小于等于栈顶运算符** 时，将 **栈顶运算符弹出并输出**；再比较新的栈顶运算符，直到该运算符大于栈顶运算符优先级为止，然后将该 **运算符压栈**；
5. 若各对象 **处理完毕**，则把栈中存留的 **运算符一并输出**。

