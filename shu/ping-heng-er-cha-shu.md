# 平衡二叉树

> 二叉搜索树的效率很高，但是存在一个问题。可能由于插入的顺序原因，导致最后树的形状出现一边倒的现象。这样会导致树的深度增加，而二叉搜索树的查询效率就是与树的深度相关。

## 1. 平衡二叉树的定义

**平衡因子（Balance Factor）：** $$BF(T) = h_L - h_R$$ ，其中 $$h_L$$ 和 $$h_R$$ 分别为 $$T$$ 的左、右子树的高度

**平衡二叉树（Balance Binary Tree）（AVL树）：**

* 空树，或者
* 任一结点左、右子树高度差的绝对值不超过 1，即 $$\mid BF(T) \mid \le 1$$ 

## 2. 平衡二叉树的调整

**发现者：**结点的平衡因子出现大于1的情况，称该结点为发现者

**麻烦结点：**插入过程中，导致出现发现者的结点

### 2.1 RR旋转（右单旋）

**麻烦结点** 在发现者 **右**子树的 **右边**

![](../.gitbook/assets/image%20%289%29.png)

### 2.2 LL旋转（左单旋）

**麻烦结点** 在发现者 **左**子树的 **左边**

![](../.gitbook/assets/image%20%282%29.png)

### 2.3 LR旋转

**麻烦结点** 在发现者 **左**子树的 **右边**

![](../.gitbook/assets/image%20%2817%29.png)

### 2.4 RL旋转

**麻烦结点** 在发现者 **右**子树的 **左边**

![](../.gitbook/assets/image%20%285%29.png)

## 3. 实现

```java
class Node {
    int key;
    int height;
    Node left;
    Node right;

    public Node(int val) {
        this.key = val;
        this.height = 1;
    }
}

public class AVLTree {

    Node root;

    public int height(Node node) {
        if (node == null) {
            return 0;
        }
        return node.height;
    }

    public int max(int a, int b) {
        return Math.max(a, b);
    }

    /**
     *      y                               x
     *     / \     Right Rotation          /  \
     *    x   T3   - - - - - - - >        T1   y
     *   / \       < - - - - - - -            / \
     *  T1  T2     Left Rotation            T2  T3
     *
     */
    public Node rightRotate(Node y) {
        Node x = y.left;
        Node T2 = x.right;

        // Perform rotation
        x.right = y;
        y.left = T2;

        // Update heights
        y.height = max(height(y.left), height(y.right)) + 1;
        x.height = max(height(x.left), height(x.right)) + 1;

        // Return new root
        return x;
    }

    /**
     *      y                               x
     *     / \     Right Rotation          /  \
     *    x   T3   - - - - - - - >        T1   y
     *   / \       < - - - - - - -            / \
     *  T1  T2     Left Rotation            T2  T3
     *
     */
    public Node leftRotate(Node x) {
        Node y = x.right;
        Node T2 = y.left;

        // Perform rotation
        y.left = x;
        x.right = T2;

        // Update heights
        x.height = max(height(x.left), height(x.right)) + 1;
        y.height = max(height(y.left), height(y.right)) + 1;

        // Return new root
        return y;
    }

    /**
     * Get Balance factor of node N
     */
    public int getBalance(Node node) {
        if (node == null) {
            return 0;
        }
        return height(node.left) - height(node.right);
    }

    public Node insert(Node node, int key) {
        /* 1. Perform the normal BST insertion */
        if (node == null) {
            return new Node(key);
        }

        if (key < node.key) {
            node.left = insert(node.left, key);
        } else if (key > node.key) {
            node.right = insert(node.right, key);
        } else { // Duplicate keys not allowed
            return node;
        }

        /* 2. Update height of this ancestor node */
        node.height = max(height(node.left), height(node.right)) + 1;

        /* 3. Get the balance factor of this ancestor
              node to check whether this node became
              unbalance */
        int balance = getBalance(node);

        /**
         * If this node become unbalance, then there
         * are 4 cases Left Left Case
         *          z                                      y
         *         / \                                   /   \
         *        y   T4      Right Rotate (z)          x      z
         *       / \          - - - - - - - - ->      /  \    /  \
         *      x   T3                               T1  T2  T3  T4
         *     / \
         *   T1   T2
         */
        if (balance > 1 && key < node.left.key) {
            return rightRotate(node);
        }

        /**
         * Right Right Case
         *   z                                y
         *  /  \                            /   \
         * T1   y     Left Rotate(z)       z      x
         *     /  \   - - - - - - - ->    / \    / \
         *    T2   x                     T1  T2 T3  T4
         *        / \
         *      T3  T4
         */
        if (balance < -1 && key > node.right.key) {
            return leftRotate(node);
        }

        /**
         * Left Right Case
         *      z                               z                           x
         *     / \                            /   \                        /  \
         *    y   T4  Left Rotate (y)        x    T4  Right Rotate(z)    y      z
         *   / \      - - - - - - - - ->    /  \      - - - - - - - ->  / \    / \
         * T1   x                          y    T3                    T1  T2 T3  T4
         *     / \                        / \
         *   T2   T3                    T1   T2
         */
        if (balance > 1 && key > node.left.key) {
            node.left = leftRotate(node.left);
            return rightRotate(node);
        }

        /**
         * Right Left Case
         *    z                            z                            x
         *   / \                          / \                          /  \
         * T1   y   Right Rotate (y)    T1   x      Left Rotate(z)   z      y
         *     / \  - - - - - - - - ->     /  \   - - - - - - - ->  / \    / \
         *    x   T4                      T2   y                  T1  T2  T3  T4
         *   / \                              /  \
         * T2   T3                           T3   T4
         */
        if (balance < -1 && key < node.right.key) {
            node.right = rightRotate(node.right);
            return leftRotate(node);
        }

        /* return the (unchanged) node pointer */
        return node;
    }

    public Node minValueNode(Node node) {
        Node current = node;
        while (current.left != null) {
            current = current.left;
        }
        return current;
    }

    public Node deleteNode(Node root, int key) {
        if (root == null) {
            return null;
        }
        if (key < root.key) {
            root.left = deleteNode(root.left, key);
        } else if (key > root.key) {
            root.right = deleteNode(root.right, key);
        } else {
            if (root.left != null && root.right != null) {
                Node minValueNode = minValueNode(root.right);
                root.key = minValueNode.key;
                root.right = deleteNode(root.right, minValueNode.key);
            } else {
                if (root.left == null) {
                    root = root.right;
                } else {
                    root = root.left;
                }
            }
        }
        if (root == null) {
            return null;
        }
        root.height = max(height(root.left), height(root.right)) + 1;

        int balance = getBalance(root);

        // Left Left Case
        if (balance > 1 && getBalance(root.left) >= 0) {
            return rightRotate(root);
        }

        // Left Right Case
        if (balance > 1 && getBalance(root.left) < 0)
        {
            root.left = leftRotate(root.left);
            return rightRotate(root);
        }

        // Right Right Case
        if (balance < -1 && getBalance(root.right) <= 0) {
            return leftRotate(root);
        }

        // Right Left Case
        if (balance < -1 && getBalance(root.right) > 0)
        {
            root.right = rightRotate(root.right);
            return leftRotate(root);
        }

        return root;
    }

    // A utility function to print preorder traversal
    // of the tree.
    // The function also prints height of every node
    public void preOrder(Node node) {
        if (node != null) {
            System.out.print(node.key + " ");
            preOrder(node.left);
            preOrder(node.right);
        }
    }

    public static void main(String[] args)
    {
        AVLTree tree = new AVLTree();

        /* Constructing tree given in the above figure */
        tree.root = tree.insert(tree.root, 9);
        tree.root = tree.insert(tree.root, 5);
        tree.root = tree.insert(tree.root, 10);
        tree.root = tree.insert(tree.root, 0);
        tree.root = tree.insert(tree.root, 6);
        tree.root = tree.insert(tree.root, 11);
        tree.root = tree.insert(tree.root, -1);
        tree.root = tree.insert(tree.root, 1);
        tree.root = tree.insert(tree.root, 2);

        /* The constructed AVL Tree would be
              9
             /  \
             1  10
            / \   \
           0   5  11
          /   / \
         -1  2  6
        */
        System.out.println("Preorder traversal of "+
                "constructed tree is : ");
        tree.preOrder(tree.root);

        tree.root = tree.deleteNode(tree.root, 10);

        /* The AVL Tree after deletion of 10
               1
              / \
             0   9
            /   / \
           -1  5 11
           / \
           2 6
        */
        System.out.println("");
        System.out.println("Preorder traversal after "+
                "deletion of 10 :");
        tree.preOrder(tree.root);
    }
}

```

