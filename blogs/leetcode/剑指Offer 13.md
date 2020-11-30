---
title: 剑指Offer 13
date: 2020-09-11
tags:
  - leetcode
  - dfs
  - 深度优先搜索
  - 剑指Offer
categories:
 - leetcode
publish: true
author: Milky
---

# 剑指Offer 13

```
地上有一个m行n列的方格，从坐标 [0,0] 到坐标 [m-1,n-1] 。一个机器人从坐标 [0, 0] 的格子开始移动，它每次可以向左、右、上、下移动一格（不能移动到方格外），也不能进入行坐标和列坐标的数位之和大于k的格子。例如，当k为18时，机器人能够进入方格 [35, 37] ，因为3+5+3+7=18。但它不能进入方格 [35, 38]，因为3+5+3+8=19。请问该机器人能够到达多少个格子？

 

示例 1：

输入：m = 2, n = 3, k = 1
输出：3
示例 2：

输入：m = 3, n = 1, k = 0
输出：1
提示：

1 <= n,m <= 100
0 <= k <= 20

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/ji-qi-ren-de-yun-dong-fan-wei-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
```

暴力，dfs记录能到达的格子

```java
class Solution {

    public static boolean[][] visit;

    public int movingCount(int m, int n, int k) {
        int res = 0;
        visit = new boolean[m][n];
        dfs(m,n,k,0,0);
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (visit[i][j]){
                    res++;
                }
            }
        }
        return res;
    }

    public void dfs(int m, int n, int k, int x, int y){
        if (x < 0 || x >= n || y < 0 || y >= m || visit[y][x]) return;
        if (getSum(x,y) > k) return;
        visit[y][x] = true;
        dfs(m,n,k,x+1,y);
        dfs(m,n,k,x-1,y);
        dfs(m,n,k,x,y+1);
        dfs(m,n,k,x,y-1);
    }

    public int getSum(int x, int y){
        int res = 0, tmp = 0;
        while (x != 0){
            res += x%10;
            x /= 10;
        }
        while (y != 0){
            res += y%10;
            y /= 10;
        }
        return res;
    }
}
```

