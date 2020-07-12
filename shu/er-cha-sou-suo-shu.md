# 二叉搜索树

## 1. 二叉搜索树的定义

**二叉搜索树（BST，Binary Search Tree）**，也称 **二叉排序树** 或 **二叉查找树**

二叉搜索树：一颗二叉树，可以为空；如果不为空，则满足下列性质：

1. 非空 **左子树** 的所有 **键值小于其根节点** 的键值；
2. 非空 **右子树** 的所有 **键值大于其根节点** 的键值；
3. **左、右子树都是二叉搜索树**。

## 2. 二叉搜索树操作的特别函数

* `Position Find(ElementType X, BinTree BST)` ：从二叉搜索树 BST 中查找元素 X，返回其所在结点的地址；
* `Position FindMin(BinTree BST)` ：从二叉搜索树 BST 中查找并返回最小元素所在结点的地址；
* `Position FindMax(BinTree BST)` ：从二叉搜索树 BST 中查找并返回最大元素所在结点的地址；
* `BinTree Insert(ElementType X, BinTree BST)` ：插入元素 X 到二叉搜索树 BST 中；
* `BinTree Delete(ElementType X, BinTree BST)` ：从二叉搜索树 BST 中删除元素 X。

### 2.1 查找操作 Find

* 查找从根结点开始，如果 **树为空**，返回 **NULL**
* 若搜索树非空，则根结点 **关键字和 X 进行比较**，并进行不同处理：
  * 若 **X 小于根结点键值**，只需在 **左子树** 中继续搜索；
  * 若 **X 大于根结点键值**，只需在 **右子树** 中继续搜索；
  * 若两个比较结果 **相等**，搜索完成，返回指向此结点的 **指针**

{% tabs %}
{% tab title="Recursive" %}
```cpp
Position Find(ElementType X, BinTree BST) {
    if (!BST)
        return NULL;
    if (X > BST->val)
        return Find(X, BST->right);
    else if (X < BST->val)
        return Find(X, BST->left);
    else
        return BST;
}
```
{% endtab %}

{% tab title="Iterative" %}
```cpp
Position IterFind(ElementType X, BinTree BST) {
    while (BST) {
        if (X > BST->val)
            BST = BST->right;
        else if (X < BST->val)
            BST = BST->left;
        else
            return BST;
    }
    return NULL;
}
```
{% endtab %}
{% endtabs %}

### 2.2 查找最大和最小元素

{% tabs %}
{% tab title="Max\(Iterative\)" %}
```cpp
Position FindMax(BinTree BST) {
    if (BST) {
        while (BST->right)
            BST = BST->right;
    }
    return BST;
}
```
{% endtab %}

{% tab title="Min\(Recursive\)" %}
```cpp
Position FindMin(BinTree BST) {
    if (!BST)
        return NULL;
    if (!BST->left)
        return BST;
    else
        FindMin(BST->left);
}
```
{% endtab %}
{% endtabs %}

### 2.3 插入操作

```cpp
BinTree Insert(ElementType X, BinTree BST) {
    if (!BST) {
        BST = (BinTree)malloc(sizeof(struct TreeNode));
        BST->val = X;
        BST->left = BST->right = NULL;
    } else {
        if (X < BST->val) {
            BST->left = Insert(X, BST->left);
        } else if (X > BST->val) {
            BST->right = Insert(X, BST->right);
        }
    }
    return BST;
}
```

### 2.4 删除操作

考虑三种情况：

* 要删除的是**叶结点：**直接删除，然后修改其父结点指针，置为NULL
* 要删除的结点**只有一个孩子结点：**将其**父结点**的指针**指向**要删除结点的**孩子结点**
* 要删除的结点**有左、右两颗子树：**用另一结点代替被删除结点，**左子树最大元素** 或者 **右子树最小元素**

```cpp
BinTree Delete(ElementType X, BinTree BST) {
    Position Tmp;
    if (!BST)
        return NULL;
    if (X < BST->val) {
        BST->left = Delete(X, BST->left);
    } else if (X > BST->val) {
        BST->right = Delete(X, BST->right);
    } else {
        if (BST->left && BST->right) {  // 情况三
            Tmp = FindMax(BST->left);
            BST->val = Tmp->val;
            BST->left = Delete(BST->val, BST->left);
            // Tmp = FindMin(BST->right);
            // BST->val = Tmp->val;
            // BST->right = Delete(BST->val, BST->right);
        } else {
            Tmp = BST;
            if (!BST->left) {  // 左孩子为空或者无孩子结点
                BST = BST->right;
            } else if (!BST->right) {  // 右孩子为空或者无孩子结点
                BST = BST->left;
            }
            free(Tmp);
        }
    }
    return BST;
}
```

