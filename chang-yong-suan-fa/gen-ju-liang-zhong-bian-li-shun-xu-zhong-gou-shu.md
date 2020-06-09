# 根据两种遍历顺序重构树

## 先序 + 中序

```java
public TreeNode buildTree(int[] preorder, int[] inorder) {
    
    if (preorder == null || inorder == null || preorder.length != inorder.length) return null;
    
    Map<Integer, Integer> map = new HashMap<>();
    for (int i = 0; i < inorder.length; i++) map.put(inorder[i], i);
    
    return helper(preorder, 0, preorder.length - 1, map, 0, inorder.length - 1);
    
}

public TreeNode helper(int[] preorder, int pb, int pe, Map<Integer, Integer> map, int ib, int ie) {
    
    if (pb > pe || ib > ie) return null;
    
    TreeNode root = new TreeNode(preorder[pb]);
    int ci = map.get(preorder[pb]);
    
    root.left = helper(preorder, pb + 1, pb + ci - ib, map, ib, ci - 1);
    root.right = helper(preorder, pb + ci -ib + 1, pe, map, ci + 1, ie);
    
    return root;
    
}
```

