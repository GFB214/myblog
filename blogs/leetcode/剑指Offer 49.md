---
title: 剑指Offer 49
date: 2020-07-08
tags:
  - leetcode
  - 动态规划
  - dp
  - 剑指Offer
categories:
 - leetcode
publish: true
author: Milky
---

# 剑指Offer 49

```
我们把只包含质因子 2、3 和 5 的数称作丑数（Ugly Number）。求按从小到大的顺序的第 n 个丑数。

 

示例:

输入: n = 10
输出: 12
解释: 1, 2, 3, 4, 5, 6, 8, 9, 10, 12 是前 10 个丑数。
说明:  

1 是丑数。
n 不超过1690。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/chou-shu-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
```

**思路：丑数*2 3 5还是丑数，dp[i]代表第i个丑数，计算dp[i]，可以将dp[i]前面的丑数都乘与2，取大与dp[i-1]中最小的数dp[x2] * 2=m2,同理计算m3,m5,dp[i]就是m2,m3,m5中最小的那个。因为丑数是递增的，如果取m2，那么计算dp[i+1]时x2之前的丑数不需要考虑乘与2**

## 代码

```java
class Solution49 {

    public int nthUglyNumber(int n) {
        int x2 = 1, x3 = 1, x5 = 1, m2, m3, dp5;
        int[] dp = new int[1700];
        dp[1] = 1;
        for (int i = 2; i <= n ; i++){
            m2 = dp[x2] * 2;
            m3 = dp[x3] * 3;
            dp5 = dp[x5] * 5;
            dp[i] = Math.min(m2,Math.min(m3,dp5));
            //三个if要分开，避免重复，比如2*3=6 3*2=6
            if (dp[i] == m2) x2++;
            if (dp[i] == m3) x3++;
            if (dp[i] == dp5) x5++;
        }
        return dp[n];
    }
}
```

