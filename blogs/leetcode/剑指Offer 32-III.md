---
title: 剑指Offer 32-I
date: 2020-09-16
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

# 剑指Offer 32-III

```
剑指 Offer 32 - III. 从上到下打印二叉树 III
请实现一个函数按照之字形顺序打印二叉树，即第一行按照从左到右的顺序打印，第二层按照从右到左的顺序打印，第三行再按照从左到右的顺序打印，其他行以此类推。

 

例如:
给定二叉树: [3,9,20,null,null,15,7],

    3
   / \
  9  20
    /  \
   15   7
返回其层次遍历结果：

[
  [3],
  [20,9],
  [15,7]
]
 

提示：

节点总数 <= 1000
```

思路：用队列从左往右层序遍历，使用链表存储每一层的输出，需要从左往右输出的加到链表尾部，需要从右往左输出的加到链表头部

```java
class Solution32_III {

    public List<List<Integer>> levelOrder(TreeNode root) {
        List<List<Integer>> res = new ArrayList<>();
        //层序遍历
        Queue<TreeNode> queue = new LinkedList<>();
        if (root != null) queue.add(root);
        //队列大小
        int size;
        while (!queue.isEmpty()){
            LinkedList<Integer> linkedList = new LinkedList<>();
            //注意如果for循环中的size不要用queue.size()替换，每次都会poll一个，size-1
            size = queue.size();
            for (int i = 0; i < size; i++) {
                TreeNode poll = queue.poll();
                //从左往右
                if (res.size()%2==0) linkedList.addLast(poll.val);
                //从右往左
                else linkedList.addFirst(poll.val);
                if (poll.left != null) queue.add(poll.left);
                if (poll.right != null) queue.add(poll.right);
            }
            res.add(linkedList);
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



