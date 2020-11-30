---
title: 剑指Offer 32-I
date: 2020-09-14
tags:
  - leetcode
  - bfs
  - 广度优先搜索
  - 二叉树
  - 剑指Offer
categories:
 - leetcode
publish: true
author: Milky
---

# 剑指Offer 32-I

```
剑指 Offer 32 - I. 从上到下打印二叉树
从上到下打印出二叉树的每个节点，同一层的节点按照从左到右的顺序打印。

 

例如:
给定二叉树: [3,9,20,null,null,15,7],

    3
   / \
  9  20
    /  \
   15   7
返回：

[3,9,20,15,7]
 

提示：

节点总数 <= 1000
```

层序遍历用队列，广搜

```java
class Solution32_I {

    public int[] levelOrder(TreeNode root) {
        List<Integer> list = new ArrayList();
        Queue<TreeNode> queue = new LinkedList<>();
        if (root != null)queue.add(root);
        while (!queue.isEmpty()){
            TreeNode node = queue.poll();
            list.add(node.val);
            if (node.left != null) queue.add(node.left);
            if (node.right != null) queue.add(node.right);
        }
        int[] res = new int[list.size()];
        int step = 0;
        for (Integer integer : list) {
            res[step++] = integer;
        }
        return res;
    }

    public class TreeNode {
        int val;
        TreeNode left;
        TreeNode right;

        TreeNode(int x) {
            val = x;
        }
    }
}
```

