# 二叉树遍历

## 1. 先序遍历

{% tabs %}
{% tab title="递归" %}
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
{% endtab %}

{% tab title="迭代" %}
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
{% endtab %}
{% endtabs %}

## 2. 中序遍历

{% tabs %}
{% tab title="递归" %}
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
{% endtab %}

{% tab title="迭代" %}
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
{% endtab %}
{% endtabs %}

## 3. 后序遍历

{% tabs %}
{% tab title="递归" %}
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
{% endtab %}

{% tab title="迭代" %}
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
{% endtab %}
{% endtabs %}

## 4. 层序遍历

```java
public List<List<Integer>> levelOrder(TreeNode root) {
    List<List<Integer>> resultList = new ArrayList<>();
    if (root == null) return resultList;
    Queue<TreeNode> queue = new LinkedList<>();
    queue.add(root);
    while (!queue.isEmpty()) {
        List<Integer> list = new ArrayList<>();
        int size = queue.size();
        for (int i = 0; i < size; i++) {
            if (queue.peek().left != null) queue.add(queue.peek().left);
            if (queue.peek().right != null) queue.add(queue.peek().right);
            list.add(queue.poll().val);
        }
        resultList.add(list);
    }
    return resultList;
}
```

