---
layout: php
title: php 解 LeetCode 162 寻找峰值.md
date: 2021-09-15 23:33:42
tags:
- php LeetCode
---
## 一.读题

峰值元素是指其值严格大于左右相邻值的元素。

给你一个整数数组 nums，找到峰值元素并返回其索引。数组可能包含多个峰值，在这种情况下，返回 任何一个峰值 所在位置即可。

你可以假设 nums[-1] = nums[n] = -∞ 。

你必须实现时间复杂度为 O(log n) 的算法来解决此问题。

地址：https://leetcode-cn.com/problems/find-peak-element/
<!--more -->

## 二.解题
只要满足nums[i-1] < nums[i] > nums[i+1]就可以了

### 1. 爬坡法：
当nums[i+1] > nums[i] 时，则 i = i+1,继续对比，当 i+1 < i 时 i就是峰值

```php 
<?php
    function findPeakElement($nums) {
        $i  = 0;
        $l = count($nums)-1;
        while ($i < $l)
        {

            // 当前和下一个比，如果下一个大 则取下一个继续对比
            if ($nums[$i] < $nums[$i+1]) {
                $i = $i+1;
            }else{ //反之就是峰点
                $l = $i;
            }
        }

        return  $i;
    }
```

### 2. 二分查找优化 

```` php
<?php
    function findPeakElement($nums) {
    
        $i  = 0;
        $l = count($nums)-1;
        while ($i < $l)
        {
            //取中间
            $mid = (int)(($i+$l)/2);
    
            // 当前和下一个比，如果下一个大 则取下一个和最右边的区间继续 
            if ($nums[$mid] < $nums[$mid+1]) {
                $i = $mid+1;
            }else{ //反之取左边
                $l = $mid;
            }
        }
    
        return  $i;
    }
````
