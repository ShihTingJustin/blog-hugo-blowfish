---
title: "[Leetcode with JavaScript] 刷題筆記 - 刷題常用的三種解法 Brute Force, Hash Map, Two Pointers"
summary: 這篇文章會透過上千題中 Leetcode 的第一題 Two Sum 及進階題 3Sum，來說明三種基本的解題方法，分別是暴力解(Brute Force)、雜湊表 Hash Table、雙指針 Two Pointers。
image: https://i.imgur.com/PSbzbj9.png
categories: ["blog"]
tags:
  [
    "ALPHA Camp",
    "Bootcamp",
    "SoftwareEngineer",
    "LeetCode",
    "TwoPointers",
    "HashMap",
    "BruteForce",
    "JavaScript",
    "軟體工程師",
    "刷題",
  ]
#externalUrl: ""
# showSummary: true
date: 2020-08-30
draft: false
---

## 前言

這個系列是在記錄參與 ALPHA Camp A+ 人才計畫的過程中，練習刷題的想法及筆記。

這篇文章會透過上千題中 Leetcode 的第一題[Two Sum](https://leetcode.com/problems/two-sum/) 及進階題 [3Sum](https://leetcode.com/problems/3sum/)，來說明三種基本的解題方法，分別是**暴力解(Brute Force)**、**雜湊表 Hash Table**、**雙指針 Two Pointers**。如果你和我一樣是剛開始刷題的話，那就繼續看下去吧～

## #1 Two Sum (Easy)

> Given an array of integers `nums` and and integer `target`, return _the indices of the two numbers such that they add up to_ `*target*`.

> You may assume that each input would have **\*exactly\* one solution**, and you may not use the _same_ element twice.

> You can return the answer in any order.

**Example :**

```
Input: nums = [2,7,11,15], target = 9
Output: [0,1]
Output: Because nums[0] + nums[1] == 9, we return [0, 1]
```

題目給一個陣列 nums 及一個目標值 target，兩者都是整數，要找出兩個陣列元素相加後與目標值相等的元素之 index。

## 暴力解 (Brute Force)

這是我腦海中第一個想法，利用巢狀迴圈的 index i 和 j 一前一後遍歷題目給的陣列元素，再用 if 判斷兩個 index 取出的值相加後是否等於 target。

不過使用巢狀迴圈的缺點就是很費時，時間複雜度也會變成 O(n^2)，所以可以看到 Runtime 只比 17.9% 的人快。Memory Usage 則是因為沒有宣告新的變數，直接使用迴圈的 index 所以表現不錯。

```js copy showLineNumbers
var twoSum = function (nums, target) {
  for (let i = 0; i < nums.length; i++) {
    for (let j = i; j < nums.length; j++) {
      if (i !== j && nums[i] + nums[j] === target) {
        return [i, j];
      }
    }
  }
};
```

![1_lx9WPQXvJxxPXXk_63guuQ](https://i.imgur.com/nefsG58.jpg)

## 雜湊表 Hash Table

之後學了 Hash Table，先透過第一次 for 迴圈將陣列 nums 中所有的元素，以 key-value pair 型式記錄到物件 map 中，這時因為 map 還是空的所以不會進到 if 條件判斷。從第二次迴圈開始 map 有資料了程式就會進入 if 條件判斷，檢查遍歷的陣列元素和物件 map 中 key-value pair 的關係。

與暴力解相比，Hash Table 的優點是有效降低時間複雜度(O(n))，可以看到 Runtime 快很多，代價是要使用比較多的記憶體空間。**Hash Table 最大的好處是 input 不需要是 sorted array 也能使用。**

```js copy showLineNumbers
var twoSum = function (nums, target) {
  let map = {};
  for (let i = 0; i < nums.length; i++) {
    let num1 = nums[i];
    let num2 = target - num1;
    if (map[num2] >= 0) {
      return [map[num2], i];
    }
    map[num1] = i;
  }
};
```

![1_MZzczpwfuS84sZo_kyE7lw](https://i.imgur.com/E9QzlHy.jpg)

看起來很簡單的題目，其實可以練習到不同的解法，難怪有人說越簡單的題目，其實越困難！

## #15 3Sum (Medium)

> Given an array `nums` of _n_ integers, are there elements _a_, _b_, _c_ in `nums` such that _a_ + _b_ + _c_ = 0? Find all unique triplets in the array which gives the sum of zero.

> **Note:**

> The solution set must not contain duplicate triplets.

**Example:**

```
Given array nums = [-1, 0, 1, 2, -1, -4],
A solution set is:
[
  [-1, 0, 1],
  [-1, -1, 2]
]
```

這題給了一個陣列，元素皆為整數且有正有負，要我們找出三個元素相加後為 0 的組合放進陣列回傳，且不能有重複的組合。

為避免重複，先 sort 但預設是字元大小排序 正負數混合的情況下會不如預期 例如 -1 -1 2 -2 會變成 -1 1 2 -2 但我們期待的是 -2 -1 1 2

題目要求三個數字相加為 0，假設是 num1, num2, num3，如果像是 Two Sum 那樣，先讓 num1 固定不動，那 num2 + num3 就會是 num1 的相反數，因此關鍵是要在 num1 之後的數字中，找出兩數相加等於 num1 的相反數，這個時候就相當適合使用**雙指針 Two Pointers**。

## 雙指針 Two Pointers

這個方法最重要的是 input 需為 sorted array，讓 nums 由小排到大，這樣當 num1 固定時，就可以讓 num2, num3 也就是左右指針，從 num1 之後的數字序列的「頭」和「尾」向中間「夾」。

```js copy showLineNumbers
/**
 * @param {number[]} nums
 * @return {number[][]}
 */
var threeSum = function (nums) {
  nums.sort((a, z) => a - z);
  const result = [];

  for (let i = 0; i < nums.length; i++) {
    // 固定第一個數字
    const num = nums[i];
    if (num > 0) break; // 因為sort過，所以第一個數字大於零，代表整個nums都是正整數
    if (num === nums[i - 1]) continue; // 若連續兩個數字相同，就繼續執行

    const ans = -num; // 要找 num 的相反數
    let left = i + 1; // 左指針的index
    let right = nums.length - 1; // 右指針的index

    while (left < right) {
      // 當左指針小於右指針就執行
      if (nums[left] + nums[right] === ans) {
        result.push([num, nums[left], nums[right]]);
        left++; // 找到一組解就讓左右指針向中間夾
        right--;
        // 左右指針移動過程中碰到重複的數字要跳過
        while (left < right && nums[left] === nums[left - 1]) left++;
        while (left < right && nums[right] === nums[right + 1]) right--;
      } else if (nums[left] + nums[right] < ans) {
        left++;
        while (left < right && nums[left] === nums[left - 1]) left++;
      } else {
        right--;
        while (left < right && nums[right] === nums[right + 1]) right--;
      }
    }
  }
  return result;
};
```
