---
title: 剑指Offer 46
date: 2020-10-10
tags:
  - leetcode
  - dp
  - dfs
  - 剑指Offer
categories:
 - leetcode
publish: true
author: Milky
---

# 剑指Offer 46

```
剑指 Offer 46. 把数字翻译成字符串
给定一个数字，我们按照如下规则把它翻译为字符串：0 翻译成 “a” ，1 翻译成 “b”，……，11 翻译成 “l”，……，25 翻译成 “z”。一个数字可能有多个翻译。请编程实现一个函数，用来计算一个数字有多少种不同的翻译方法。

 

示例 1:

输入: 12258
输出: 5
解释: 12258有5种不同的翻译，分别是"bccfi", "bwfi", "bczi", "mcfi"和"mzi"
 

提示：

0 <= num < 2^31
```

#### 解法一：dfs

考虑翻译一个或者翻译两个

```java
class Solution46 {

    int count;
    String[] chars;

    public int translateNum(int num) {
        String num2Str = num + " ";
        chars = num2Str.split("");
        chars[chars.length - 1] = "";
        dfs(0);
        return count;
    }

    public void dfs(int index) {
        //翻译完
        if (index >= chars.length - 1) {
            count++;
            return;
        }
        int num = Integer.parseInt(chars[index] + chars[index + 1]);
        dfs(index + 1);
        //10<=num<=25 翻译两个
        if (num >= 10 && num <= 25) dfs(index + 2);
    }

}
```

#### 解法二：动态规划

dp[i]表示0至i这段序列有多少种翻译方法

当前位和前一位组合数字x, 10<=x<=25 则可以一位一位翻或者两位连起来翻，故dp[i] = dp[i-1]+dp[i+2]

其余情况只能一位一位翻dp[i] = dp[i-1]

```java
class Solution46 {

    public int translateNum(int num){
        if (num < 10) return 1;
        String num2Str = num + "";
        char[] chars = num2Str.toCharArray();
        int[] dp = new int[chars.length];
        dp[0] = 1;
        dp[1] = (chars[0] == '1') || (chars[0] == '2' && chars[1] < '6')?2:1;
        for (int i = 2; i < dp.length; i++) {
            if ((chars[i - 1] == '1') || (chars[i-1] == '2' && chars[i] < '6')){
                dp[i] = dp[i-1] + dp[i-2];
            } else {
                dp[i] = dp[i-1];
            }
        }
        return dp[dp.length-1];
    }

}
```

