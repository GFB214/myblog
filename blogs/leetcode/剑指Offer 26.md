---
title: 剑指Offer 26
date: 2020-09-12
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

# 剑指Offer 26

```
剑指 Offer 26. 树的子结构
输入两棵二叉树A和B，判断B是不是A的子结构。(约定空树不是任意一个树的子结构)

B是A的子结构， 即 A中有出现和B相同的结构和节点值。

例如:
给定的树 A:

     3
    / \
   4   5
  / \
 1   2
给定的树 B：

   4 
  /
 1
返回 true，因为 B 与 A 的一个子树拥有相同的结构和节点值。

示例 1：

输入：A = [1,2,3], B = [3,1]
输出：false
示例 2：

输入：A = [3,4,5,1,2], B = [4,1]
输出：true
限制：

0 <= 节点个数 <= 10000
```

看到题目的时候想到了用匹配子树用深搜，却没想到遍历树对每个节点进行匹配。。太蠢了

思路：搜索遍历树， 对各个节点再用搜索匹配

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
    public boolean isSubStructure(TreeNode A, TreeNode B) {
        if (B == null) return false;
        if (A == null) return false;
        //如果不匹配，则询问其子节点
        return dfs(A,B) || isSubStructure(A.left,B) || isSubStructure(A.right,B);
    }

    //检查树是否匹配
    public static boolean dfs(TreeNode A, TreeNode B){
        if (B == null) return true;
        if (A == null) return false;
        //仅当三个条件成立返回true
        return A.val == B.val && dfs(A.left,B.left) && dfs(A.right,B.right);
    }
}
```



