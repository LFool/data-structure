---
description: 线性表的顺序存储实现
---

# 顺序表

## 1. 线性表的顺序存储实现

利用数组的 **连续存储空间顺序存放** 线性表的各元素

| 下标 i | 0 | 1 | $$\cdots$$  | i - 1 | i | $$\cdots$$  | n - 1 | $$\cdots$$  | MAXSIZE - 1 |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| Data | $$a_1$$  | $$a_2$$  | $$\cdots$$  | $$a_i$$  | $$a_{i+1}$$  | $$\cdots$$  | $$a_n$$  | $$\cdots$$  | - |

```c
typedef struct LNode* List;
struct LNode {
    ElementType Data[MAXSIZE];
    int Last;
};

struct LNode L;
List PtrL;

// 访问下标为 i 的元素：L.Data[i] 或 PtrL->Data[i]
// 线性表的长度：L.Last+1 或 PtrL->Last+1
```

## 2. 主要操作的实现

### 2.1 初始化（建立空的顺序表）

```cpp
List MakeEmpty() {
    List PtrL;
    PtrL = (List)malloc(sizeof(struct LNode));
    PtrL->Last = -1;
    return PtrL;
}
```

### 2.2 查找

```cpp
int Find(ElementType X, List PtrL) {
    int i = 0;
    while (i <= PtrL->Last && PtrL->Data[i] != X)
        i++;
    if (i > PtrL->Last)  // 如果没有找到，返回 -1
        return -1;
    else  // 找到后返回的是存储位置
        return i;
}
```

**查找成功的平均比较时间为** $$(n + 1) / 2$$ **，平均时间性能为** $$O(n)$$ ****

### 2.3 插入（在第 $$i (1 \le i \le n + 1)$$ 个位置上插入一个值为 $$X$$ 的新元素）

```cpp
void Insert(ElementType X, int i, List PtrL) {
    int j;
    if (PtrL->Last == MAXSIZE - 1) {
        printf("表满\n");
        return;
    }
    if (i < 1 || i > PtrL->Last + 2) {  // 1 <= i <= n + 1
        printf("位置不合法\n");
        return;
    }
    for (j = PtrL->Last; j >= i - 1; j--)
        PtrL->Data[j + 1] = PtrL->Data[j];
    PtrL->Data[i - 1] = X;
    PtrL->Last++;
    return;
}
```

**平均移动次数为** $$n/2$$ **，平均时间性能为** $$O(n)$$ ****

### 2.4 删除（删除表的第 $$i (1 \le i \le n)$$ 个位置上的元素）

```cpp
void Delete(int i, List PtrL) {
    int j;
    if (i < 1 || i > PtrL->Last + 1) {
        printf("不存在第%d个元素\n", i);
        return;
    }
    for (j = i; j <= PtrL->Last; j++)
        PtrL->Data[j - 1] = PtrL->Data[j];
    PtrL->Last--;
    return;
}
```

**平均移动次数为** $$(n-1)/2$$ **，平均时间性能为** $$O(n)$$ ****

