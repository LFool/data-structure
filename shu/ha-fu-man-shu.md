# 哈夫曼树

## 1. 哈夫曼树的定义

**带权路径长度（WPL）：**设二叉树有 **n 个叶子节点**，每个叶子结点带有权值 $$w_k $$ ，从根节点到每个叶子结点的长度为 $$I_k$$ ，则每个叶子结点的带权路径长度之和为： $$WPL = \sum^n_{k=1}w_kI_k$$ 

**最优二叉树** 或 **哈夫曼树**：**WPL** 最小的二叉树

## 2. 哈夫曼树的构造

每次把 **权值最小的两棵** 二叉树合并

```java
class HuffmanNode {
    int data;
    int weight;
    HuffmanNode left;
    HuffmanNode right;

    public HuffmanNode(int weight) {
        this.weight = weight;
    }
}

public class HuffmanTree {

    public HuffmanNode huffman(HuffmanNode[] nodes) {
        PriorityQueue<HuffmanNode> minHeap = new PriorityQueue<>(Comparator.comparingInt(o -> o.weight));

        minHeap.addAll(Arrays.asList(nodes));

        while (minHeap.size() > 1) {
            HuffmanNode root = new HuffmanNode(0);
            root.left = minHeap.poll();
            root.right = minHeap.poll();
            assert root.right != null;
            root.weight = root.left.weight + root.right.weight;
            minHeap.add(root);
        }
        return minHeap.poll();
    }

    // A utility function to print preorder traversal
    // of the tree.
    // The function also prints height of every node
    public void preOrder(HuffmanNode node) {
        if (node != null) {
            System.out.print(node.weight + " ");
            preOrder(node.left);
            preOrder(node.right);
        }
    }

    @Test
    public void test() {
        HuffmanNode[] huffmanNodes = new HuffmanNode[5];
        for (int i = 0; i < 5; i++) {
            huffmanNodes[i] = new HuffmanNode( i + 1);
        }
        HuffmanNode root = huffman(huffmanNodes);
        preOrder(root);
    }

}
```

## 3. 哈夫曼树的特点

* 没有度为 1 的结点
* n 个叶子结点的哈夫曼树共有 2n-1 个结点（可通过公式证明：边数 = 度数）
* 哈夫曼树的任意非叶结点的 **左右子树交换** 后仍是哈夫曼树
* 对同一组权值 $$\{ w_1, w_2, \cdots , w_n \}$$ ，存在不同构的两棵哈夫曼树

## 4. 哈夫曼编码

根据出现的频率，构造哈夫曼树，这样所需的代价最小

![](../.gitbook/assets/image%20%289%29.png)

