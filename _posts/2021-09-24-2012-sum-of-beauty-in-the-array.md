---
title: 2012. Sum of Beauty in the Array
author: Yu-Lun Chiang
date: 2021-09-24 10:00:00 +0800
categories: [Leetcode, An-Apple-Per-Day]
tags: [leetcode, medium, array]
math: true
---

[Leetcode #2012](https://leetcode.com/problems/sum-of-beauty-in-the-array/)

---
## 題目描述

You are given a 0-indexed integer array `nums`. 

For each index `i` (`1 <= i <= nums.length - 2`) the beauty of `nums[i]` equals:

- `2`, if `nums[j] < nums[i] < nums[k]`, for **all** `0 <= j < i` and for **all** `i < k <= nums.length - 1`.
- `1`, if `nums[i - 1] < nums[i] < nums[i + 1]`, and the previous condition is not satisfied.
- `0`, if none of the previous conditions holds.

Return the sum of beauty of all `nums[i]` where `1 <= i <= nums.length - 2`.

### 白話文運動

請回傳一個一維陣列 `nums` 在 `1 <= i <= nums.length - 2` 的<span style="color:green">**漂亮**</span>總和。

什麼是<span style="color:green">**漂亮**</span>呢？

在一維陣列中 (`1 <= i <= nums.length - 2`)，`nums[i]` 的<span style="color:green">**漂亮**</span>值為:

- `2`, if `nums[j] < nums[i] < nums[k]`, for **all** `0 <= j < i` and for **all** `i < k <= nums.length - 1`.
- `1`, if `nums[i - 1] < nums[i] < nums[i + 1]`, 且第一個條件不滿足的前提之下。
- `0`, 且上述兩個條件都不滿足的前提之下。

### 題目限制

- 3 <= nums.length <= 10<sup>5</sup>
    
    看到這麼長的長度，基本上時間複雜度必須為 O(N)

- 1 <= nums[i] <= 10<sup>5</sup>


---
## 具體測資

### Case 1

```
Input: nums = [1,2,3]
Output: 2
Explanation: For each index i in the range 1 <= i <= 1:
- The beauty of nums[1] equals 2.
```

### Case 2

```
Input: nums = [2,4,6,4]
Output: 1
Explanation: For each index i in the range 1 <= i <= 2:
- The beauty of nums[1] equals 1.
- The beauty of nums[2] equals 0.
```

### Case 3

```
Input: nums = [3,2,1]
Output: 0
Explanation: For each index i in the range 1 <= i <= 1:
- The beauty of nums[1] equals 0.
```


---
## 解題想法

### 概念

滿足<span style="color:green">第一個漂亮條件</span>的前提是，當前的 `nums[i]` 必須<span style="color:red">**大於之前的數字**</span>、<span style="color:blue">**小於之後的數字**</span>，因此，我認為需要額外製作兩個矩陣，分別紀錄當前數字的<span style="color:red">**左邊最大值**</span>與<span style="color:blue">**右邊最小值**</span>。

稍待片刻，消化一下。

只要當前數字<span style="color:red">**大於左邊最大值**</span>，那麼，<u>當前數字就大於左邊所有數字</u>了！

同理，當前數字<span style="color:blue">**小於右邊最小值**</span>，那麼，<u>當前數字就小於右邊所有數字</u>了！

那麼，我們只需要分別寫一個迴圈，遍歷`nums`中的元素即可得到這兩個矩陣，時間複雜度只有O(N)唷！

另外，<span style="color:green">第二個漂亮條件</span>相對簡單，遍歷`nums`即可完成。

舉例來說:
```
nums      = [1,   2, 3, 9, 5, 6, 7  ]

left_max  = [0,   1, 2, 3, 9, 9, 0  ]
right_min = [inf, 3, 5, 5, 6, 7, inf]

beauty    = [x,   2, 2, 0, 0, 1, x  ]
ret       = 2 + 2 + 0 + 0 + 1
          = 5
```

### Pseudo Code

```
GIVEN: nums is a 1D array.

SET n is len(nums).
SET left_max is a n size 1D array where all elements are zero.
SET right_min is a n size 1D array where all elements are float('inf').
SET ret is a empty list.
        
## create left_max list
FOR i (1 to n-1) do
    IF nums[i-1] > left_max[i-1] do
        left_max[i] <- nums[i-1]
    ELSE
        left_max[i] <- left_max[i-1]
    ENDIF
ENDFOR
    
## create right_min list
FOR i (n-2 to 0) do
    IF nums[i+1] < right_min[i+1] do
        right_min[i] <- nums[i+1]
    ELSE
        right_min[i] <- right_min[i+1]
    ENDIF
ENDFOR

## collect ans
FOR i in (1 to n-1) do
    IF left_max[i] < nums[i] < right_min[i] do
        ret <- ret + 2
    ELIF nums[i-1] < nums[i] < nums[i+1] do
        ret <- ret + 1
    ENDIF
ENDFOR

RETURN ret
```

### 複雜度分析

- 時間複雜度: O(n)
- 空間複雜度: O(n)


---
## 實際解題

### Python

```python
class Solution:
    def sumOfBeauties(self, nums: List[int]) -> int:
        
        n = len(nums)
        
        ## create left_max list
        left_max = [0] * n
        for i in range(1, n-1, 1):
            if nums[i-1] > left_max[i-1]:
                left_max[i] = nums[i-1]
            else:
                left_max[i] = left_max[i-1]
            
        ## create right_min list
        right_min = [float("inf")] * n
        for i in range(n-2, 0, -1):
            if nums[i+1] < right_min[i+1]:
                right_min[i] = nums[i+1]
            else:
                right_min[i] = right_min[i+1]
        
        ## collect ans
        ret = 0
        for i in range(1, n-1, 1):
            if left_max[i] < nums[i] < right_min[i]:
                ret += 2
            elif nums[i-1] < nums[i] < nums[i+1]:
                ret += 1
        
        return ret
```


---
## 討論區

寫了 40 分鐘，後來跟討論區的解法雷同，真是開心 :)
