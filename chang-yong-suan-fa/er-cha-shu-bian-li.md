# 二叉树遍历

```java
/**
 * Definition for a binary tree node.
 */
public class TreeNode {
    int val;
    TreeNode left;
    TreeNode right;
    TreeNode() {}
    TreeNode(int val) { this.val = val; }
    TreeNode(int val, TreeNode left, TreeNode right) {
        this.val = val;
        this.left = left;
        this.right = right;
    }
}
```

## 1. 先序遍历

### 递归实现

```java
public List<Integer> preorderTraversal(TreeNode root) {
    
    List<Integer> res = new ArrayList<>();
    preorderTraversal(root, res);
    return res;
    
}

private void preorderTraversal(TreeNode root, List<Integer> list) {

    if (root == null) return ;

    list.add(root.val);
    preorderTraversal(root.left, list);
    preorderTraversal(root.right, list);

}
```

### 迭代实现

```java
public List<Integer> preorderTraversal(TreeNode root) {

    List<Integer> res = new ArrayList<>();
    Stack<TreeNode> stack = new Stack<>();
    
    while (root != null || !stack.isEmpty()) {
    
        while (root != null) {
            res.add(root.val);
            stack.push(root);
            root = root.left;
        }
        root = stack.pop();
        root = root.right;
    }
    
    return res;
}
```

## 2. 中序遍历

### 递归实现

```java
public List<Integer> inorderTraversal(TreeNode root) {
    
    List<Integer> res = new ArrayList<>();
    inorderTraversal(root, res);
    return res;
    
}

private void inorderTraversal(TreeNode root, List<Integer> list) {
    
    if (root == null) return ;
            
    inorderTraversal(root.left, list);
    list.add(root.val);
    inorderTraversal(root.right, list);
    
}
```

### 迭代实现

```java
public List<Integer> inorderTraversal(TreeNode root) {
    
    List<Integer> res = new ArrayList<>();
    Stack<TreeNode> stack = new Stack<>();
    
    while (root != null || !stack.isEmpty()) {
    
        while (root != null) {
            stack.push(root);
            root = root.left;
        }
        root = stack.pop();
        res.add(root.val);
        root = root.right;
    }
    
    return res;
}
```

## 3. 后序遍历

### 递归实现

```java
public List<Integer> postorderTraversal(TreeNode root) {

    List<Integer> res = new ArrayList<>();
    postorderTraversal(root,res);
    return res;

}

private void postorderTraversal(TreeNode root, List<Integer> list) {

    if (root == null) return ;

    postorderTraversal(root.left, list);
    postorderTraversal(root.right, list);
    list.add(root.val);

}
```

### 迭代实现

```java
public List<Integer> postorderTraversal(TreeNode root) {

    List<Integer> res = new ArrayList<>();
    Stack<TreeNode> stack = new Stack<>();

    while (root != null || !stack.isEmpty()) {

        while (root != null) {
            stack.push(root);
            root = root.left == null ? root.right : root.left;
        } 
        TreeNode temp = stack.pop();
        res.add(temp.val);
        if (!stack.isEmpty() && stack.peek().left == temp) {
            root = stack.peek().right;
        }        
    }
    return res;
}
```

## 4. 层序遍历

