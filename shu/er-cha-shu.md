# 二叉树

## 1. 二叉树的定义

二叉树 T：一个有穷的结点集合

这个集合 **可以为空**

若不为空，则它是由 **根节点** 和其称为其 **左子树** $$T_L$$ 和 **右子树** $$T_R$$ 的两个不想交的二叉树组成

### 1.1 特殊二叉树

**斜二叉树（Skewed Binary Tree）**

![](../.gitbook/assets/image%20%2821%29.png)

**完美二叉树（Perfect Binary Tree）/ 满二叉树（Full Binary Tree）**

![](../.gitbook/assets/image%20%288%29.png)

**完全二叉树（Complete Binary Tree）**

**与满二叉树的区别：**叶子结点可以不满，但是要按照从左到右的顺序存放

![](../.gitbook/assets/image%20%281%29.png)

## 2. 二叉树性质

1. 一个二叉树第 $$i$$ 层的最大结点树为： $$2^{i - 1}, i \ge 1$$ 
2. 深度为 $$k$$ 的二叉树有最大结点总数为： $$2^k - 1, i \ge 1$$ 
3. 对于任何非空二叉树 T，若 $$n_0$$ 表示叶结点的个数、 $$n_2$$ 是度为 2 的非叶结点个数，那么两者满足关系 $$n_0 = n_2 + 1$$ 

   这类关系的证明只需要把握一个等式关系：**总边数 = 总度数**

   $$0 * n_0 + 1 * n_1 + 2 * n_2 = n_0 + n_1 + n_2 - 1$$ 

## 3. 二叉树的抽象数据类型定义

**类型名称：**二叉树

**数据对象集：**一个有穷的结点集合。若不为空，则由 **根结点和其左、右二叉子树** 组成

**操作集：** $$BT \in BinTree, Item \in ElementType$$ ，重要的操作有：

* `Boolean IsEmpty(BinTree BT)`：判断 ****$$BT$$ 是否为空；
* `void Traversal(BinTree BT)`：遍历，按某顺序访问每个结点；
* `BinTree CreatBinTree()`：创建一个二叉树。

**常用的遍历方法有：**

* `void PreOrderTraversal(BinTree BT)` ：先序（根、左子树、右子树）
* `void InOrderTraversal(BinTree BT)` ：中序（左子树、根、右子树）
* `void PostOrderTraversal(BinTree BT)` ：后序（左子树、右子树、根）
* `void LevelOrderTraversal(BinTree BT)` ：层次遍历（从上到下、从左到右）

## 4. 二叉树的存储结构

### 4.1 顺序存储结构

利用数组存储，结点下标从 1 开始计数，结点 $$i$$ 的左孩子下标为 $$2i$$ ，右孩子下标为 $$2i + 1$$ 

若一棵树非完全二叉树，就会造成空间浪费

![](../.gitbook/assets/image%20%2819%29.png)

### 4.2 链式存储结构

```cpp
typedef struct TreeNode* BinTree;
typedef BinTree Position;
struct TreeNode {
    ElementType val;
    BinTree left;
    BinTree right;
};
```

## 5. 二叉树的遍历

### 5.1 先序遍历

{% tabs %}
{% tab title="Recursive" %}
```cpp
void preorderTraversal(TreeNode* root, vector<int>& v) {
    if (root) {
        v.push_back(root->val);
        preorderTraversal(root->left, v);
        preorderTraversal(root->right, v);
    }
}
```
{% endtab %}

{% tab title="Iterative" %}
```cpp
vector<int> preorderTraversal(TreeNode* root) {
    vector<int> res;
    stack<TreeNode*> stack;

    while (root || !stack.empty()) {
        while (root) {
            res.push_back(root->val);
            stack.push(root);
            root = root->left;
        }

        root = stack.top();
        stack.pop();
        root = root->right;
    }
    return res;
}
```
{% endtab %}
{% endtabs %}

### 5.2 中序遍历

{% tabs %}
{% tab title="Recursive" %}
```cpp
void inorderTraversal(TreeNode* root, vector<int>& v) {
    if (root) {
        inorderTraversal(root->left, v);
        v.push_back(root->val);
        inorderTraversal(root->right, v);
    }
}
```
{% endtab %}

{% tab title="Iterative" %}
```cpp
vector<int> inorderTraversal(TreeNode* root) {
    vector<int> res;
    stack<TreeNode*> stack;

    while (root || !stack.empty()) {
        while (root) {
            stack.push(root);
            root = root->left;
        }

        root = stack.top();
        stack.pop();
        res.push_back(root->val);
        root = root->right;
    }
    return res;
}
```
{% endtab %}
{% endtabs %}

### 5.3 后序遍历

{% tabs %}
{% tab title="Recursive" %}
```cpp
void postorderTraversal(TreeNode* root, vector<int>& v) {
    if (root) {
        postorderTraversal(root -> left, v);
        postorderTraversal(root -> right, v);
        v.push_back(root -> val);
    }
}
```
{% endtab %}

{% tab title="Iterative" %}
```cpp
vector<int> postorderTraversal(TreeNode* root) {
    vector<int> res;
    stack<TreeNode*> stack;

    while (root || !stack.empty()) {
        while (root) {
            stack.push(root);
            if (root -> left) root = root->left;
            else root = root->right;
        }

        TreeNode* node = stack.top();
        stack.pop();
        res.push_back(node->val);
        if (!stack.empty() && stack.top()->left == node) {
            root = stack.top()->right;
        }
    }

    return res;
}
```
{% endtab %}
{% endtabs %}

### 5.4 层序遍历

```cpp
vector<vector<int>> levelOrder(TreeNode* root) {
    vector<vector<int>> res;
    if (!root) return res;
    queue<TreeNode*> queue;
    queue.push(root);

    while (!queue.empty()) {
        vector<int> temp;
        int size = queue.size();
        for (int i = 0; i < size; i++) {
            TreeNode* node = queue.front();
            queue.pop();
            temp.push_back(node->val);
            if (node->left) queue.push(node->left);
            if (node->right) queue.push(node->right);
        }
        res.push_back(temp);
    }

    return res;
}
```

## 6. 常用算法

### 6.1 求二叉树中的叶子结点

```cpp
void preorderGetLeaves(TreeNode* root, vector<int>& v) {
    if (root) {
        if (!root->left && !root->right)
            v.push_back(root->val);
        preorderTraversal(root->left, v);
        preorderTraversal(root->right, v);
    }
}
```

### 6.2 求二叉树的高度

```cpp
int postorderGetHeight(TreeNode* root) {
    if (!root)
        return 0;
    int HL = postorderGetHeight(root->left);
    int HR = postorderGetHeight(root->right);
    return max(HL, HR) + 1;
}
```



