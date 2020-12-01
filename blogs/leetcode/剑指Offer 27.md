---
title: 剑指Offer 27
date: 2020-12-01
tags:
  - leetcode
  - dfs
  - 深度优先搜索
  - 树
  - 剑指Offer
categories:
 - leetcode
publish: true
author: Milky
---

# 剑指Offer 27

```
剑指 Offer 27. 二叉树的镜像
请完成一个函数，输入一个二叉树，该函数输出它的镜像。

例如输入：

     4
   /   \
  2     7
 / \   / \
1   3 6   9
镜像输出：

     4
   /   \
  7     2
 / \   / \
9   6 3   1

 

示例 1：

输入：root = [4,2,7,1,3,6,9]
输出：[4,7,2,9,6,3,1]
 

限制：

0 <= 节点个数 <= 1000
```

每个点都左右互换

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public TreeNode mirrorTree(TreeNode root) {
        if (root == null) return root;
        dfs(root);
        return root;
    }

    private void dfs(TreeNode node){
        if (node == null){
            return;
        }
        TreeNode tmp = node.right;
        node.right = node.left;
        node.left = tmp;
        dfs(node.left);
        dfs(node.right);
    }
}
```



