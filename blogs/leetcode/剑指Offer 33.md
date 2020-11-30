---
title: 剑指Offer 32-I
date: 2020-09-19
tags:
  - leetcode
  - 二叉树
  - 二叉搜索树
  - 剑指Offer
categories:
 - leetcode
publish: true
author: Milky
---

# 剑指Offer 33

```
 剑指 Offer 33. 二叉搜索树的后序遍历序列
输入一个整数数组，判断该数组是不是某二叉搜索树的后序遍历结果。如果是则返回 true，否则返回 false。假设输入的数组的任意两个数字都互不相同。

 

参考以下这颗二叉搜索树：

     5
    / \
   2   6
  / \
 1   3
示例 1：

输入: [1,6,3,2,5]
输出: false
示例 2：

输入: [1,3,2,6,5]
输出: true
 

提示：

数组长度 <= 1000
```

二叉搜索树，又称二叉排序树，定义：父节点左子树上任意一个节点值都小于父节点，右子树上任意一个节点值都大于父节点

思路：由后序遍历可以得知，最后一个节点为根节点，通过与根节点值比较大小，可以划分出左子树和右子树，校验左子树上所有节点是否小于根节点，右子树上所有节点是否大于根节点，再对左子树右子树按相同方式递归划分校验

```java
class Solution33 {

    public boolean verifyPostorder(int[] postorder) {
        // 最大的树
        return verify(postorder,0,postorder.length-1);
    }

    public boolean verify(int[] postorder, int start, int end){
        //没有左/右子树
        if (start >= end) return true;
        int p = start;
        //划分左子树
        while (postorder[p] < postorder[end]) p++;
        //记录左子树根节点
        int m = p-1;
        //划分右子树
        while (postorder[p] > postorder[end]) p++;
        //p==end表示每个节点都符合与根节点的大小关系
        //再递归校验左子树和右子树
        return p == end && verify(postorder,start,m) && verify(postorder,m+1,end-1);
    }
}
```

