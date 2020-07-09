---
description: 线性表的链式存储实现
---

# 链表

## 1. 线性表的链式存储实现

**不要求逻辑上相邻的两个元素物理上也相邻**，通过 **链** 建立起数据元素之间的逻辑关系

* 插入、删除 **不需要移动数据元素**，只需要修改 **链**

```cpp
typedef struct LNode* List;
struct LNode {
    ElementType Data;
    List next;
};

struct LNode L;
List PtrL;
```

## 2. 主要操作的实现

### 2.1 求表长

```cpp
int Length(List PtrL) {
    List p = PtrL;
    int j = 0;
    while (p) {
        p = p->next;
        j++;
    }
    return j;
}
```

### 2.2 查找

### 2.3 插入（在第 $$i (1 \le i \le n + 1)$$ 个位置上插入一个值为 $$X$$ 的新元素）

### 2.4 删除（删除表的第 $$i (1 \le i \le n)$$ 个位置上的元素）

