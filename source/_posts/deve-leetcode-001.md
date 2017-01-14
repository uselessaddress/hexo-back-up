---
title: "leetcode刷题记录001-Two Sum"
toc: true
date: 2016-10-13 21:31:58
tags:
- leetcode
- 算法
- java
categories: 设计开发
---
# 前言

leetcode开刷，采用java。

# 题目

题目链接：https://leetcode.com/problems/two-sum/

## 概要
Given an array of integers, return indices of the two numbers such that they add up to a specific target.

You may assume that each input would have exactly one solution.

## Example:
```
Given nums = [2, 7, 11, 15], target = 9,

Because nums[0] + nums[1] = 2 + 7 = 9,
return [0, 1].
```

<!--more-->

# 解题过程
## 第一次思路
两次循环，遍历数组内的数字，两两相加。如果两两相加的和等于target，则把两个数对应的下标放入结果数组内。
```
public class Solution {
    public int[] twoSum(int[] nums, int target) {
        for(int i=0;i<nums.length;i++){
            for(int j=0;j<nums.length;j++){
                if(i!=j){
                    int sum = nums[i]+nums[j];
                    if(target == sum){
                        int[] result = new int[]{i,j};
                        return result;
                    }
                }
            }
        }
        return null;
    }
}

```
未通过，Time Limit Exceeded。leetcode对代码有效率要求，迫使我们优化代码，nice。

## 第二次思路
第二层循环，从i+1开始。
```
public class Solution {
    public int[] twoSum(int[] nums, int target) {
        for(int i=0;i<nums.length;i++){
            for(int j=i+1;j<nums.length;j++){
                int sum = nums[i]+nums[j];
                if(target == sum){
                    int[] result = new int[]{i,j};
                    return result;
                }
            }
        }
        return null;
    }
}
```
Accepted，但是我知道，这不是最优方案。先这样吧，有了更好的解法再补上。

# 源码地址
https://github.com/voidking/leetcode/tree/master/src/com/voidking/leetcode/question001

# 书签
LeetCode Online Judge
https://leetcode.com/

LeetCode 解题报告 | 书影博客
http://bookshadow.com/leetcode/

LintCode - 主页
http://www.lintcode.com/zh-cn/


