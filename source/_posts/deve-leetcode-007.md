---
title: "leetcode刷题记录007-Reverse Integer"
toc: true
date: 2016-10-13 21:31:58
tags:
- leetcode
- 算法
- java
categories: 设计开发
---
# 题目

题目链接：https://leetcode.com/problems/reverse-integer/

Reverse digits of an integer.

Example1: x = 123, return 321
Example2: x = -123, return -321

<!--more-->

# 解题过程
## 第一次思路
1、循环内x不断除以10，直到变成0，跳出循环。
2、循环内x不断除以10求余，并把求得的余数添加到结果字符串上。
3、强制转换结果字符串为int类型。
```
public class Solution {
    public int reverse(int x) {
        String resultStr = "";
        if(x==0){
            resultStr = "0";
        }else if(x>0){
            do{
                resultStr = resultStr + x%10 ;
                x = x/10;
                if(x>=1 && x<= 9){
                    resultStr = resultStr + x%10 ;
                }
            }while(x/10 !=0 );
        }else{
            x=-x;
            do{
                resultStr = resultStr + x%10 ;
                x = x/10;
                if(x>=1 && x<= 9){
                    resultStr = resultStr + x%10 ;
                }
            }while(x/10 !=0 );
            resultStr = "-"+resultStr;
        }
        return Integer.parseInt(resultStr);   
    }
}

```
报错Line 26: java.lang.NumberFormatException: For input string: "9646324351"。

## 第二次思路
单击click to show spoilers.，发现还需要处理int溢出的问题。
那就加上int超出范围的错误处理。
```
public class Solution {
    public int reverse(int x) {
        String resultStr = "";
        if(x==0){
            resultStr = "0";
        }else if(x>0){
            do{
                resultStr = resultStr + x%10 ;
                x = x/10;
                if(x>=1 && x<= 9){
                    resultStr = resultStr + x%10 ;
                }
            }while(x/10 !=0 );
        }else{
            x=-x;
            do{
                resultStr = resultStr + x%10 ;
                x = x/10;
                if(x>=1 && x<= 9){
                    resultStr = resultStr + x%10 ;
                }
            }while(x/10 !=0 );
            resultStr = "-"+resultStr;
        }
        try {
            return Integer.parseInt(resultStr);
        } catch (Exception e) {
            return 0;
        }
        
    }
}

```
Accepted。


# 源码地址
https://github.com/voidking/leetcode/tree/master/src/com/voidking/leetcode/question007


