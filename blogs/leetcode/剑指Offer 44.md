---
title: 剑指Offer 44
date: 2020-07-07
tags:
  - leetcode
  - 暴力
  - 剑指Offer
categories:
 - leetcode
publish: true
author: Milky
---

# 剑指Offer 44

```
数字以0123456789101112131415…的格式序列化到一个字符序列中。在这个序列中，第5位（从下标0开始计数）是5，第13位是1，第19位是4，等等。

请写一个函数，求任意第n位对应的数字。

 

示例 1：

输入：n = 3
输出：3
示例 2：

输入：n = 11
输出：0
 

限制：

0 <= n < 2^31
```

**思路：先算出是几位数，然后算出这个数，最后取对应位**

## 代码

```java
class Solution44 {

    public int findNthDigit(int n) {
        int level = 1;//表示是几位数
        //求level
        while (n > 0.9*pow(level,10)*level){
            n -= 0.9*pow(level,10)*level;
            level++;
        }
        //算出这个数
        int result = pow(level-1,10) + (n-1)/level;
        //取对应位置
        return String.valueOf(result).charAt((n-1)%level) - '0';
    }

    public int pow(int a, int b){
        int c = 1;
        while (a > 0){
            c *= b;
            a --;
        }
        return c;
    }
}
```

