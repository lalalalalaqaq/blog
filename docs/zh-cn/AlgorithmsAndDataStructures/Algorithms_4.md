# 二叉树

lc 226:

- 自顶向下解决
- 在第二层的交换后回交换左右节点
  - 再往下一层的交换的本质（递归的本质）：
    - 把左子树看成是一个完整的二叉树即可
    - 把每一次调用的函数看成操作的是只有两层的二叉树

```java
// lc226 反转二叉树
public TreeNode invertTree(TreeNode root) {

    if(root == null){return root;}

    TreeNode tmp = null;
    tmp = root.right;
    root.right = root.left;
    root.left = tmp;

    invertTree(root.left);
    invertTree(root.right);

    return root;  

}
```



lc 116:

- 自顶向下解决
- 同一分支下的左右节点可直接next,然后遍历(类似lc 226的思路)
- 问题的关键在于
  - left.right 和 right.left 的连接
  - 把这个节点的行为抽取出来即可（即connectPlus）

```java
public Node connect(Node root) {
    if (root == null) {return root;}
    connectPlus(root.left,root.right);
    return root;
}

public void connectPlus(Node node1, Node node2){
    if (node1 == null || node2 == null) {
        return;
    }
    node1.next = node2;
    connectPlus(node1.left,node1.right);
    connectPlus(node1.right,node2.left);
    connectPlus(node2.left,node2.right);

}
```



lc 114

- 目前没想明白
- 前序遍历

```JAVA
 public void flatten(TreeNode root) {

        // base case
        if (root == null) return;

        flatten(root.left);
        flatten(root.right);

        /**** 后序遍历位置 ****/
        // 1、左右⼦树已经被拉平成⼀条链表
        TreeNode left = root.left;
        TreeNode right = root.right;

        // 2、将左⼦树作为右⼦树
        root.left = null;
        root.right = left;
        // 3、将原先的右⼦树接到当前右⼦树的末端
        TreeNode p = root;
        while (p.right != null) {
            p = p.right;
        }
        p.right = right;

    }
```

