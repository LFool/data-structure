# 二叉树

## 1. 二叉树的定义

二叉树 T：一个有穷的结点集合

这个集合 **可以为空**

若不为空，则它是由 **根节点** 和其称为其 **左子树** $$T_L$$ 和 **右子树** $$T_R$$ 的两个不想交的二叉树组成

### 1.1 特殊二叉树

**斜二叉树（Skewed Binary Tree）**

![](../.gitbook/assets/image%20%2811%29.png)

**完美二叉树（Perfect Binary Tree）/ 满二叉树（Full Binary Tree）**

![](../.gitbook/assets/image%20%285%29.png)

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



