---
title: php 解 LeetCode 650 只有两个键的键盘
date: 2021-09-19 15:43:35 

categories:
- php
- 算法

tags:
- php Leetcode
---
## 读题 
最初记事本上只有一个字符 'A' 。你每次可以对这个记事本进行两种操作：

Copy All（复制全部）：复制这个记事本中的所有字符（不允许仅复制部分字符）。
Paste（粘贴）：粘贴 上一次 复制的字符。
给你一个数字 n ，你需要使用最少的操作次数，在记事本上输出 恰好 n 个 'A' 。返回能够打印出 n 个 'A' 的最少操作次数。

示例 1：
```
输入：3
输出：3
解释：
最初, 只有一个字符 'A'。
第 1 步, 使用 Copy All 操作。
第 2 步, 使用 Paste 操作来获得 'AA'。
第 3 步, 使用 Paste 操作来获得 'AAA'。
```
示例 2：
````
输入：n = 1
输出：0
````
地址：https://leetcode-cn.com/problems/2-keys-keyboard/

## 解题
### 1.思路
n=1，不用走 
n为偶数时，需要额外添加两步，全复制，和粘贴
找出当前n的最大约数

### 代码
```` php
<?php
class Solution {

    /**
     * @param Integer $n
     * @return Integer
     */
    function minSteps($n) {
        //为1时 不需要操作
        if ($n == 1) {
            return  0;
        }

        //为偶数时需要额外添加2部复制全部和粘贴
        if ($n % 2== 0) {
            return $this->minSteps($n/2)+2;
        }

        for ($i = $n -1; $i > 1; $i--) {
            //求最大的约数 没有约数的话当前n是几就是几步
            if ($n % $i == 0) {
                //有几倍就要加几步
                return $this->minSteps($i) + ($n/$i);
            }
        }

        return $n;
    }
}

$obj = new Solution();
echo  $obj->minSteps(10);
````
