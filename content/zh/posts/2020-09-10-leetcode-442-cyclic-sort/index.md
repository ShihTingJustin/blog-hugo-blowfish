---
title: "[Leetcode with JavaScript] 刷題筆記 — 特殊方法 Cyclic Sort"
summary: 這是最近刷 Leetcode 學習到的特殊方法，適合在需要排序且題目要求 in-place，也就是空間複雜度 O(1) 的狀況下使用(使用的記憶體不會隨輸入資料增加而增加)
image: https://i.imgur.com/Nr88uWx.png
categories: ["blog"]
tags:
  [
    "ALPHA Camp",
    "Bootcamp",
    "SoftwareEngineer",
    "LeetCode",
    "CyclicSort",
    "JavaScript",
    "排序",
    "軟體工程師",
    "刷題",
  ]
#externalUrl: ""
# showSummary: true
date: 2020-09-10
draft: false
---

## 前言

這是最近刷 Leetcode 學習到的特殊方法，適合在需要排序且題目要求 in-place，也就是空間複雜度 O(1) 的狀況下使用(使用的記憶體不會隨輸入資料增加而增加)；它的時間複雜度為 O(n) 也比一般 `sort()` 更好 O(n²)。這個方法我覺得直接看範例就很容易懂了～

## 舉例

Reference #1 是 AC 的學長 Danny 寫的文章，由於他的範例設計得非常好，所以我就直接借用他的範例來說明了。

假設有一個陣列是 `[3, 1, 0, 2]`，Cyclic Sort 排序的方式會是這樣：

1. 第一個元素 `3`，它的位置應該要在 `index = 3`，所以把它跟在 `index = 3`的元素交換，陣列會變成 `[2, 1, 0, 3]`
2. 第一個元素 `2`，它的位置應該要在 `index = 2`，所以把它跟在 `index = 2`的元素交換，陣列會變成 `[0, 1, 2, 3]`
3. 第一個元素 `0`，它的位置應該要在 `index = 0`，它的位置是正確的，因此陣列維持 `[0, 1, 2, 3]`，第一個元素擺在正確位置後，下個步驟會開始看第二個元素
4. 第二個元素 `1`，它的位置應該要在 `index = 1`，它的位置是正確的，因此陣列維持 `[0, 1, 2, 3]`，第二個元素擺在正確位置後，下個步驟會開始看第三個元素
5. 第三個元素 `2`，它的位置應該要在 `index = 2`，它的位置是正確的，因此陣列維持 `[0, 1, 2, 3]`，第三個元素擺在正確位置後，下個步驟會開始看第四個元素
6. 第四個元素 `3`，它的位置應該要在 `index = 3`，它的位置是正確的，因此陣列維持 `[0, 1, 2, 3]`，這已經是最後一個元素，因此陣列中所有元素已經檢查一遍了，也都放在正確的位置，Cyclic Sort 結束。

## 用 JavaScript 實現

Cyclic Sort 是利用陣列元素當作 targetIndex，也就是元素應該要在的位置；若元素不在該在的位置，與該位置的元素互換位置，若元素已經在對的位置，則往下一個元素檢查。

```js copy showLineNumbers
function cyclicSort(nums) {
  let index = 0; // 目前指向的元素

  while (index < nums.length) {
    // 利用陣列元素值當作 targetIndex
    // 也就是目前的元素應該要在的位置
    const targetIndex = nums[index];

    // 若目前元素不在正確的位置，則與該位置的元素互換
    if (nums[index] !== nums[targetIndex]) {
      // 利用解構賦值方法讓兩個元素互換
      [nums[index], nums[targetIndex]] = [nums[targetIndex], nums[index]];
    } else {
      // 若元素已經在正確的位置，則往下一個元素看
      index++;
    }
  }
  return nums;
}

console.log(cyclicSort([3, 1, 0, 2])); // [0, 1, 2, 3]
```

如果可以理解的話，就馬上來做一題試試吧！

## 442. Find All Duplicates in an Array

> Given an array of integers, 1 ≤ a[i] ≤ _n_ (_n_ = size of array), some elements appear **twice** and others appear **once**.

> Find all the elements that appear **twice** in this array.

> Could you do it without extra space and in O(_n_) runtime?

**Example:**

```
Input:
[4,3,2,7,8,2,3,1]
Output:
[2,3]
```

這題是要找出陣列中出現兩次的數字，然後用陣列形式回傳，時間複雜度與空間複雜度的要求都很嚴格，很適合使用 Cyclic Sort。

思考方式：

1. 用 Cyclic Sort 讓輸入的陣列排序
2. 找出陣列中與 index 不符合的元素

```js copy showLineNumbers
var findDuplicates = function (nums) {
  let result = [];
  let index = 0;
  while (index < nums.length) {
    // 符合題目規定的範圍 1 ≤ a[i] ≤ n
    const targetIndex = nums[index] - 1;
    if (nums[index] !== nums[targetIndex]) {
      [nums[index], nums[targetIndex]] = [nums[targetIndex], nums[index]];
    } else {
      index++;
    }
  }
  // 若有元素與index不同,就代表這個數字是重複的
  for (let i = 0; i < nums.length; i++) {
    if (nums[i] !== i + 1) result.push(nums[i]);
  }
  return result;
};
```

這題解法為了符合題目規定的數字範圍，要在 `targetIndex`的值 `-1`，才不會變成 0 導致出現 undefined 哦。

## Reference

1. [javascript leetcode 刷題筆記-coding pattern:Cyclic Sort](https://eruditeness.news.blog/2020/08/26/javascript-leetcode刷題筆記-coding-patterncyclic-sort/)
2. [Leetcode 刷題 pattern — Cyclic Sort](https://blog.techbridge.cc/2020/02/16/leetcode-刷題-pattern-cyclic-sort/)
3. [[YouTube] Cycle Sort](https://www.youtube.com/watch?v=ZSJGf5Ngw18)
