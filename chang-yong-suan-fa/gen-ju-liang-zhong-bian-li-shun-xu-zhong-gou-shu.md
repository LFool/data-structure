# 根据两种遍历顺序重构树

## 1. 先序 + 中序

```java
public TreeNode buildTree(int[] preorder, int[] inorder) {
    
    if (preorder == null || inorder == null || preorder.length != inorder.length) return null;
    
    Map<Integer, Integer> map = new HashMap<>();
    for (int i = 0; i < inorder.length; i++) map.put(inorder[i], i);
    
    return helper(preorder, 0, preorder.length - 1, map, 0, inorder.length - 1);
    
}

private TreeNode helper(int[] preorder, int pb, int pe, Map<Integer, Integer> map, int ib, int ie) {
    
    if (pb > pe || ib > ie) return null;
    
    TreeNode root = new TreeNode(preorder[pb]);
    int ci = map.get(preorder[pb]);
    
    root.left = helper(preorder, pb + 1, pb + ci - ib, map, ib, ci - 1);
    root.right = helper(preorder, pb + ci -ib + 1, pe, map, ci + 1, ie);
    
    return root;
    
}
```

## 2. 中序 + 后序

```java
public TreeNode buildTree(int[] inorder, int[] postorder) {

    if (inorder == null || postorder == null || inorder.length != postorder.length) return null;

    Map<Integer, Integer> map = new HashMap<>();
    for (int i = 0; i < inorder.length; i++) map.put(inorder[i], i);

    return helper(map, 0, inorder.length - 1, postorder, 0, postorder.length - 1);

}

private TreeNode helper(Map<Integer, Integer> map, int ib, int ie, int[] postorder, int pb, int pe) {

    if (ib > ie || pb > pe) return null;

    TreeNode root = new TreeNode(postorder[pe]);
    int ci = map.get(postorder[pe]);

    root.left = helper(map, ib, ci - 1, postorder, pb, pe - ie + ci - 1);
    root.right = helper(map, ci + 1, ie, postorder, pe - ie + ci, pe - 1);

    return root;

}
```

## 3. 先序 + 后序

## 4. 先序 + 二叉搜索树

