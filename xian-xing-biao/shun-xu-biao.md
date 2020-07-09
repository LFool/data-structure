---
description: 线性表的顺序存储实现
---

# 顺序表

## 1. 利用数组的 **连续存储空间顺序存放** 线性表的各元素

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



### 2.2 查找

### 2.3 插入（第 $$i (1 \le i \le n + 1)$$ 个位置上插入一个值为 $$X$$ 的新元素）

### 2.4 删除（删除表的第 $$i (1 \le i \le n)$$ 个位置上的元素）

