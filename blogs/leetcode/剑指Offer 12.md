---
title: 剑指Offer 12
date: 2020-09-10
tags:
  - leetcode
  - dfs
  - 深度优先搜索
  - 剪枝
  - 剑指Offer
categories:
 - leetcode
publish: true
author: Milky
---

# 剑指Offer 12

```
请设计一个函数，用来判断在一个矩阵中是否存在一条包含某字符串所有字符的路径。路径可以从矩阵中的任意一格开始，每一步可以在矩阵中向左、右、上、下移动一格。如果一条路径经过了矩阵的某一格，那么该路径不能再次进入该格子。例如，在下面的3×4的矩阵中包含一条字符串“bfce”的路径（路径中的字母用加粗标出）。

[["a","b","c","e"],
["s","f","c","s"],
["a","d","e","e"]]

但矩阵中不包含字符串“abfb”的路径，因为字符串的第一个字符b占据了矩阵中的第一行第二个格子之后，路径不能再次进入这个格子。

 

示例 1：

输入：board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "ABCCED"
输出：true
示例 2：

输入：board = [["a","b"],["c","d"]], word = "abcd"
输出：false
提示：

1 <= board.length <= 200
1 <= board[i].length <= 200

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/ju-zhen-zhong-de-lu-jing-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
```

**思路：以每个字母开始dfs**

## 代码

暴力刚好过

```java
public boolean exist(char[][] board, String word) {
    char[] chars = word.toCharArray();
    for (int i = 0; i < board.length; i++) {
        for (int j = 0; j < board[i].length; j++) {
            boolean flag = dfs(board, chars, 0, i, j);
            if (flag) {
                return true;
            }
        }
    }
    return false;
}

public static boolean dfs(char[][] board, char[] words, int pos, int i, int j) {
    if (i < 0 || i >= board.length || j < 0 || j >= board[i].length || board[i][j] != words[pos]) return false;
    if (pos == words.length - 1) return true;
    char tmp = board[i][j];
    board[i][j] = '-';
    if (dfs(board, words, pos + 1, i - 1, j) || dfs(board, words, pos + 1, i + 1, j) || dfs(board, words, pos + 1, i, j - 1) || dfs(board, words, pos + 1, i, j + 1))
        return true;
    board[i][j] = tmp;
    return false;
}
```

剪枝

```java
class Solution {
    public static char[][] boards;
    public static char[] words;
    //visited[i][j][pos] = true表示dfs(pos,i,j)搜过且结果为false，可以跳过
    public static boolean[][][] visited = new boolean[100][100][10000];

    public boolean exist(char[][] board, String word) {
        words = word.toCharArray();
        boards = board;
        for (int i = 0; i < boards.length; i++) {
            for (int j = 0; j < boards[i].length; j++) {
                boolean flag = dfs2(0,i,j);
                if (flag){
                    return true;
                }
            }
        }
        return false;
    }

    public static boolean dfs2(int pos, int i, int j){
        //越界或者搜过则跳过
        if (i < 0 || i >= boards.length || j < 0 || j >= boards[i].length || visited[i][j][pos]) return false;
        if (boards[i][j] != words[pos]) {
            //修改标记
            visited[i][j][pos] = true;
            return false;
        }
        if (pos == words.length - 1) return true;
        char tmp = boards[i][j];
        boards[i][j] = '-';
        if (dfs2(pos + 1, i - 1, j) || dfs2(pos + 1, i + 1, j) || dfs2(pos + 1, i, j - 1) || dfs2(pos + 1, i, j + 1)) {
            return true;
        }
        boards[i][j] = tmp;
        //修改标记
        visited[i][j][pos] = true;
        return false;
    }
}
```

