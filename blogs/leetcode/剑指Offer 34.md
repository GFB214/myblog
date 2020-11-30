---
title: 剑指Offer 32-I
date: 2020-09-26
tags:
  - leetcode
  - 二叉树
  - 记录路径
  - 深搜
  - dfs
  - 剑指Offer
categories:
 - leetcode
publish: true
author: Milky
---

# 剑指Offer 34

```
剑指 Offer 34. 二叉树中和为某一值的路径
输入一棵二叉树和一个整数，打印出二叉树中节点值的和为输入整数的所有路径。从树的根节点开始往下一直到叶节点所经过的节点形成一条路径。

 

示例:
给定如下二叉树，以及目标和 sum = 22，

              5
             / \
            4   8
           /   / \
          11  13  4
         /  \    / \
        7    2  5   1
返回:

[
   [5,4,11,2],
   [5,8,4,5]
]
 

提示：

节点总数 <= 10000
```

深搜记录路径

此题路径必须到叶子节点

```java
class Solution34 {

    static List<Integer> list = new ArrayList<>();
    static List<List<Integer>> res = new ArrayList<>();

    public List<List<Integer>> pathSum(TreeNode root, int sum) {
        res = new ArrayList<>();
        list = new ArrayList<>();
        dfs(root,0,sum);
        return res;
    }

    public void dfs(TreeNode node, int count, int sum){
        if (node == null || count + node.val > sum) return;
        list.add(node.val);
        //必须是叶子节点结束
        if (count + node.val == sum && node.left==null && node.right==null) {
            List<Integer> tmp = new ArrayList<>();
            tmp.addAll(list);
            res.add(tmp);
        }
        dfs(node.left, count+node.val, sum);
        dfs(node.right, count+node.val, sum);
        //回退时要把最后一个节点删除
        if (list.size() > 0)list.remove(list.size()-1);
        return;
    }
}
```

