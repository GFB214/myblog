---
title: 剑指Offer 45
date: 2020-10-07
tags:
  - leetcode
  - 贪心
  - 剑指Offer
categories:
 - leetcode
publish: true
author: Milky
---

# 剑指Offer 45

```
剑指 Offer 45. 把数组排成最小的数
输入一个非负整数数组，把数组里所有数字拼接起来排成一个数，打印能拼接出的所有数字中最小的一个。

 

示例 1:

输入: [10,2]
输出: "102"
示例 2:

输入: [3,30,34,5,9]
输出: "3033459"
 

提示:

0 < nums.length <= 100
说明:

输出结果可能非常大，所以你需要返回一个字符串而不是整数
拼接起来的数字可能会有前导 0，最后结果不需要去掉前导 0
```

思路：贪心，很容易想到要按字典序排序，但是要注意像“3”，“30”，“39”组成最小数字顺序应该是“30”，“3”，”39“

考虑到要组成数字，比较时可以将”3“和”30“连起比较，”303“<"330"，所以”30“在“3”前面

```java
class Solution45 {

    public String minNumber(int[] nums) {
        String res = "";
        String[] nums2Str = new String[nums.length];
        for (int i = 0; i < nums.length; i++) {
            nums2Str[i] = nums[i] + "";
        }
//        Arrays.sort(nums2Str);
        //自定义排序
        Arrays.sort(nums2Str, (s1,s2) -> (s1+s2).compareTo(s2+s1));
        for (int i = 0; i < nums2Str.length; i++) {
            res += nums2Str[i];
        }
        return res;
    }
}
```

