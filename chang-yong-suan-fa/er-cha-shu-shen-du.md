# 二叉树深度

```java
public int treeDepth(TreeNode root) {
    if (root == null) return 0;
    
    return Math.max(treeDepth(root.left), treeDepth(root.right)) + 1;
}
```



