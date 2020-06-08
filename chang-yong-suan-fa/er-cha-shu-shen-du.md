# 二叉树深度

```java
public int Depth(TreeNode root) {
    if (root == null) return 0;
    
    return Math.max(maxDepth(root.left), maxDepth(root.right)) + 1;
}
```



