---
title: 剑指Offer 28
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
剑指 Offer 28. 对称的二叉树
请实现一个函数，用来判断一棵二叉树是不是对称的。如果一棵二叉树和它的镜像一样，那么它是对称的。

例如，二叉树 [1,2,2,3,4,4,3] 是对称的。

    1
   / \
  2   2
 / \ / \
3  4 4  3
但是下面这个 [1,2,2,null,3,null,3] 则不是镜像对称的:

    1
   / \
  2   2
   \   \
   3    3

 

示例 1：

输入：root = [1,2,2,3,4,4,3]
输出：true
示例 2：

输入：root = [1,2,2,null,3,null,3]
输出：false
 

限制：

0 <= 节点个数 <= 1000
```

对称移动指针

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
    public boolean isSymmetric(TreeNode root) {
        if (root == null || (root.left == null&&root.right == null)) return true;
        return dfs(root.left,root.right);
    }

    private boolean dfs(TreeNode left, TreeNode right){
        if (left==right) return true;
        if (left==null || right==null) return false;
        return (left.val == right.val) && dfs(left.left,right.right) && dfs(left.right,right.left);
    }
}
```


