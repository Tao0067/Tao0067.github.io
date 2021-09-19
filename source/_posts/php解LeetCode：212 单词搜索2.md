---
title: php 解 LeetCode 第212题 单词搜索2
date: 2021-09-19 10:12:31

categories:
- php
- 算法 
  
tags:
- php Leetcode
---
 ## 一.读题
给定一个 m x n 二维字符网格 board 和一个单词（字符串）列表 words，找出所有同时在二维网格和字典中出现的单词。

单词必须按照字母顺序，通过 相邻的单元格 内的字母构成，其中“相邻”单元格是那些水平相邻或垂直相邻的单元格。同一个单元格内的字母在一个单词中不允许被重复使用。

![img.png](/../images/img.png)

示例 1： 

    输入：board = [["o","a","a","n"],["e","t","a","e"],["i","h","k","r"],["i","f","l","v"]], words = ["oath","pea","eat","rain"]
    输出：["eat","oath"]

题目地址：https://leetcode-cn.com/problems/word-search-ii/
<!--more -->

## 二.解题
### 1.解题思路
需要把查询的单词转为一个字典树结构  
（图片来源维基百科）
![img.png](/../images/img_1.png)


然后以条件的字母每个下标为起点，进行回溯算法找出单词。

### 2.代码

```php
<?php
class Solution {

    /**
     * @param String[][] $board
     * @param String[] $words
     * @return String[]
     */
    // 以00为坐标的4个方向
    public $dirs = [[1, 0], [-1, 0], [0, 1], [0, -1]];

    function findWords($board, $words) {
        //初始化树
        $trie = new Trie();

        //添加单词
        foreach ($words as $word) {
            $trie->addString($trie, $word);
        }

        $ans = [];

        for($i = 0; $i < count($board); $i++) {
            for($j = 0; $j < count($board[0]); $j++) {
                 //以每个字母为起点查找
                $this->dfs($board, $i, $j, $trie, '',$ans);
            }
        }

      return $ans;
    }

    function dfs($board, $i, $j, $trie, $k, &$ans) {
        // 查询当前字母是否在这个节点的子节点点中 第一次为根节点，所以第一次是从单词的第一个开始查询
        $trie = $trie->searchChildNode($trie, $board[$i][$j]);

        //不存在返回
        if (!$trie) {
            return false;
        }

        //存在记录起来
        $k .= $trie->value;

        //如果是单词结尾 就装起来 并删除这个单词
        if ($trie->is_end) {
            $trie->is_end = false;
            $ans[] = $k;
        }

        //使用过的单词先置为# 不能用
        $str  = $board[$i][$j];
        $board[$i][$j] = '#';

        foreach ($this->dirs as $dir) {
                
            // 往4个方向查找
            if ($i+$dir[0] >= 0 && $i+$dir[0] < count($board) && $j+$dir[1] >= 0 && $j+$dir[1] < count($board[0])){
                $this->dfs($board, $i+$dir[0], $j+$dir[1], $trie, $k,$ans);
            }
        }

        //查找完了就变为原值
        $board[$i][$j] = $str;
    }
}

class Trie
{
    public $value; //值
    public $is_end = false; //是否最后一个字母
    public $child_node = array(); //下一个节点

    public function addChildNode(Trie $trie,$value, $is_end = false)
    {
        $node = $this->searchChildNode($trie, $value);
        if (!$node) {
            $node = new Trie();
            $node->value = $value;
            // trie 每一个字母都为上一个的子节点
            $trie->child_node[] = $node;
        }

        if ($is_end) {
            $node->is_end = $is_end;
        }
        return $node;
    }

    public function searchChildNode(Trie $trie,$value)
    {
        //当前节点的子节点是否有这个值
        foreach ($trie->child_node as $k => $node) {

            if ($node->value == $value) {
                return $node;
            }
        }
        return  false;
    }

    //添加单词 Trie 为根目录的子节点
    public function addString(Trie $trie, $str)
    {
        for ($i = 0; $i < strlen($str); $i++) {
            if ($str[$i]) {
                $is_end = ($i+1-strlen($str)) == 0 ? true : false;
                // trie 每一个字母都为上一个的子节点
                $trie = $this->addChildNode($trie, $str[$i], $is_end);
            }
        }
    }

}

$obj = new Solution();
$obj->findWords([["o","a","b","n"],["o","t","a","e"],["a","h","k","r"],["a","f","l","v"]],["oa","oaa"]);

```
