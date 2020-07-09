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

**时间性能为** $$O(n)$$ ****

### 2.2 查找

{% tabs %}
{% tab title="按序号查找" %}
```cpp
List FindKth(int K, List PtrL) {
    List p = PtrL;
    int i = 1;
    while (p != NULL && i < K) {
        p = p->next;
        i++;
    }
    if (i == K)
        return p;
    else
        return NULL;
}
```
{% endtab %}

{% tab title="按值查找" %}
```cpp
List Find(ElementType X, List PtrL) {
    List p = PtrL;
    while (p != NULL && p->Data != X)
        p = p->next;
    return p;
}
```
{% endtab %}
{% endtabs %}

**平均时间性能为** $$O(n)$$ ****

### 2.3 插入（在第 $$i (1 \le i \le n + 1)$$ 个位置上插入一个值为 $$X$$ 的新元素）

* 先构建一个新结点，用 s 指向
* 再找到链表的第 i-1 个结点，用 p 指向
* 然后修改指针，插入结点（p 之后插入新结点是 s）

```cpp
List Insert(ElementType X, int i, List PtrL) {
    List p, s;
    if (i == 1) {
        s = (List)malloc(sizeof(struct LNode));
        s->Data = X;
        s->next = PtrL;
        return s;
    }
    p = FindKth(i - 1, PtrL);
    if (p == NULL) {
        printf("参数 i 错误\n");
        return;
    } else {
        s = (List)malloc(sizeof(struct LNode));
        s->Data = X;
        s->next = p->next;
        p->next = s;
        return PtrL;
    }
}
```

**平均查找次数为** $$n/2$$ **，平均时间性能为** $$O(n)$$ ****

### 2.4 删除（删除表的第 $$i (1 \le i \le n)$$ 个位置上的元素）

* 先找到链表的第 i-1 个结点，用 p 指向
* 再用指针 s 指向要被删除的结点（p 的下一个结点）
* 然后修改指针，删除 s 所指结点
* 最后释放 s 所指结点的空间

```cpp
List Delete(int i, List PtrL) {
    List p, s;
    if (i == 1) {
        s = PtrL;
        if (PtrL != NULL)
            PtrL = PtrL->next;
        else
            return NULL;
        free(s);
        return PtrL;
    }
    p = FindKth(i - 1, PtrL);
    if (p == NULL) {
        printf("第%d个结点不存在\n", i - 1);
        return NULL;
    } else if (p->next == NULL) {
        printf("第%d个结点不存在\n", i);
        return NULL;
    } else {
        s = p->next;
        p->next = s->next;
        free(s);
        return PtrL;
    }
}
```

**平均查找次数为** $$n/2$$ **，平均时间性能为** $$O(n)$$ ****

