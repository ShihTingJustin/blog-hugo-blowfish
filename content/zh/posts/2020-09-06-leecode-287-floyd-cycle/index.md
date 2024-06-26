---
title: "[Leetcode with JavaScript] 刷題筆記 — 特殊方法 Floyd's Cycle Detection"
summary: 這是最近刷 Leetcode 學習到的特殊方法，適合拿來解尋找重複數字的題目。
image: https://i.imgur.com/k16HQ1G.png
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
date: 2020-09-06
draft: false
---

## 前言

這是最近刷 Leetcode 學習到的特殊方法，適合拿來解尋找重複數字的題目， 如 Array 題型 **287. Find the Duplicate Number**，還能解答 Linked List 相關題型如 **141. Linked List Cycle、142. Linked List Cycle II**，覺得相當實用，因此決定寫成筆記，內容著重於應用在刷題，若想細究原理的讀者可以參考底下的 Reference #4 #5。

## Floyd’s Cycle Detection

有發現封面圖片上出現了烏龜與兔子嗎？因為這個演算法俗稱龜兔賽跑演算法。以下是原理及結論：

1. 假設數列是一個賽道，龜兔一起出發，兔子跑得比烏龜快，如果這個賽道中沒有環(cycle)存在，在龜兔速度不變的情況下，烏龜永遠不會與兔子相遇；但**\*只要賽道中有環，那龜兔一定會在某一點相遇\*** (可參考 Reference #1)
2. 龜兔相遇後，可藉由將烏龜退回起點，兔子在原地不動，此時龜兔之間的距離，會是環的整數倍，然後讓龜兔每次只動一格，兩者交會時就會是環的起點。

龜兔兩個在數列裡跑，聽起來好像有點熟悉，沒錯，這個概念和我在[上一篇刷題筆記](https://medium.com/@mercedes722s/leetcode-with-javascript-刷題筆記-第一題-two-sum-就學會三種-approach-59ae63cebe29)中所介紹的雙指針 Two Pointers 方法很像，只是指針一個快一個慢，因此又稱為「**快慢指針」。**

接下來直接看題目實作吧！

## 287. Find the Duplicate Number

> Given an array of integers `nums` containing `n + 1` integers where each integer is in the range `[1, n]` inclusive.

> There is only **one duplicate number** in `nums`, return _this duplicate number_.

**Follow-ups:**

1. How can we prove that at least one duplicate number must exist in `nums`?
2. Can you solve the problem **without** modifying the array `nums`?
3. Can you solve the problem using only constant, `O(1)` extra space?
4. Can you solve the problem with runtime complexity less than `O(n2)`?

**Example 1:**

```
Input: nums = [1,3,4,2,2]
Output: 2
```

**Example 2:**

```
Input: nums = [3,1,3,4,2]
Output: 3
```

題目要求是找出陣列中重複的數字然後回傳，聽起來好像不難嘛 😏，但是最後還有但書，空間複雜度 O(1)，時間複雜度 O(n²) 😱，因此若想用暴力法搭配 .sort() 都不符合題目的規範。

這時候就需要使用快慢指針了，以下是思考方式：

1. 使用兩個變數 `slow` `fast` 來代表慢指針(烏龜) 及 快指針(兔子)，他們一開始會在原點也就是 `nums[0]`
2. 然後讓龜兔速度不同，因此 `fast`會用兩層陣列的方式，來實作兔子走得比較快的狀況
3. 利用迴圈讓龜兔重複 #2 的動作，**直到** `**slow**` `**fast**`**相等(龜兔相遇)，代表有環存在**
4. 讓`slow`退回原點`nums[0]`，`fast`保持在原地不動
5. 然後再次利用迴圈讓龜兔開始跑，**但這次龜兔的速度要是一致的，再次相遇時，就是環的起點，也代表重複的數字**

```js copy showLineNumbers
var findDuplicate = function (nums) {
  let slow = nums[0];
  let fast = nums[0];

  slow = nums[slow];
  fast = nums[nums[fast]];

  // find meeting point
  while (slow !== fast) {
    slow = nums[slow];
    fast = nums[nums[fast]];
  }

  // find entry point of cycle
  slow = nums[0];
  while (slow !== fast) {
    slow = nums[slow]; // slow back to start point
    fast = nums[fast]; // fast stay at meeting point
  }
  return fast;
};
```

這個方法也能應用在 Linked List 的題型，先來題簡單的：

## 141. Linked List Cycle

> Given a linked list, determine if it has a cycle in it.

> To represent a cycle in the given linked list, we use an integer `pos` which represents the position (0-indexed) in the linked list where the tail connects to. If `pos == -1`, then there is no cycle in the linked list.

> **Follow up:**

> Can you solve it using _O(1)_ (i.e. constant) memory?

**Example 1:**

![0_zfG8Tyew6vuExFyU](https://i.imgur.com/I2RQZcE.png)

```
Input: head = [3,2,0,-4], pos = 1
Output: true
Explanation: There is a cycle in the linked list, where tail connects to the second node.
```

這題對空間複雜度有相同的要求，因此也相當適合使用快慢指針，基本上邏輯相同，就不再贅述了，如果有學過 Linked List 的人可以順便練習一下。

```js copy showLineNumbers
var hasCycle = function (head) {
  let fast = head;
  let slow = head;

  // check the cycle
  while (fast && fast.next) {
    fast = fast.next.next;
    slow = slow.next;
    // cycle exist
    if (fast === slow) return true;
  }
  // no cycle
  return false;
};
```

## 142. Linked List Cycle II

> Given a linked list, return the node where the cycle begins. If there is no cycle, return `null`.

> To represent a cycle in the given linked list, we use an integer `pos` which represents the position (0-indexed) in the linked list where tail connects to. If `pos` is `-1`, then there is no cycle in the linked list.

> **Note:** Do not modify the linked list.

**Example 1:**

```
Input: head = [3,2,0,-4], pos = 1
Output: tail connects to node index 1
Explanation: There is a cycle in the linked list, where tail connects to the second node.
```

![0_FHndGxUHRjw2FvcK](https://i.imgur.com/Rd1i1Pv.png)

```js copy showLineNumbers
var detectCycle = function (head) {
  let fast = head;
  let slow = head;
  while (fast && fast.next) {
    fast = fast.next.next;
    slow = slow.next;
    if (fast === slow) {
      slow = head;
      while (fast !== slow) {
        fast = fast.next;
        slow = slow.next;
      }
      return slow;
    }
  }
  return null;
};
```

## Reference

1. [If Programming Was An Anime](https://www.youtube.com/watch?v=pKO9UjSeLew)
2. [判圈算法](https://blog.csdn.net/u011221820/article/details/78821464)
3. [找出重複數字：Floyd Cycle Detection Algorithm 龜兔賽跑演算法](https://medium.com/@chiafangsung/找出重複數字-floyd-cycle-detection-algorithm-龜兔賽跑演算法-c7c2a0315f68)
4. [探索 Floyd Cycle Detection Algorithm](https://medium.com/@orionssl/探索-floyd-cycle-detection-algorithm-934cdd05beb9)
5. [[Wiki\] Cycle detection](https://en.wikipedia.org/wiki/Cycle_detection)
