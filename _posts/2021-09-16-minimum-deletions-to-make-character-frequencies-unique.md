---
title: 1647. Minimum Deletions to Make Character Frequencies Unique
author: Yu-Lun Chiang
date: 2021-09-16 16:20:00 +0800
categories: [Leetcode, An-Apple-Per-Day]
tags: [leetcode, medium, string, hashmap]
math: true
---

---
## 題目描述

A string s is called good if there are no two different characters in s that have the same frequency.
Given a string s, return the minimum number of characters you need to delete to make s good.

The frequency of a character in a string is the number of times it appears in the string. For example, in the string "aab", the frequency of 'a' is 2, while the frequency of 'b' is 1.

### 白話文運動

有一字串 s 若被稱為<span style="color:green">__好字串__</span>，代表字串中的字元頻率必唯一；反之，若被稱為<span style="color:green">__壞字串__</span>，那麼，請回傳最少執行幾次刪除操作(一次只能刪除一個字元)，才可以讓字串改邪歸正。

何謂字元頻率，舉例來說，"<span style="color:red">aa</span><span style="color:blue">b</span>" 字串中，"<span style="color:red">a</span>" 的字元頻率為 <span style="color:red">2</span>，"<span style="color:blue">b</span>" 的字元頻率為 <span style="color:blue">1</span>。 

### 題目限制
- 1 <= s.length <= 105
- s contains only lowercase English letters.


---
## 具體例子

### Case 1
```
Input  : s = "aab"
Output : 0 
Explanation : s is already good
```

### Case 2
```
Input  : s = "aaabbbccc"
Output : 2
Explanation : You can delete two 'b's resulting in the good string "aaabcc".
Another way it to delete one 'b' and one 'c' resulting in the good string "aaabbc".
```

### Case 3
```
Input  : s = "ceabaacb"
Output : 2
Explanation : You can delete both 'c's resulting in the good string "eabaab".
Note that we only care about characters that are still in the string at the end (i.e. frequency of 0 is ignored).
```


---
## 解題想法

### 概念
1. 計算字元頻率

    在進行刪除操作之前，須先掌握字串中的各個字元頻率，因此一開始應先思考<b><span style="color:green">如何計算字元頻率</span></b>。
    我的想法是建立一個 hashmap，裡頭存放 <span style="color:red">key</span>:<span style="color:blue">value</span>，意即 <span style="color:red">字元</span>:<span style="color:blue">字元個數</span>。此時，取得特定字元的字元頻率所花費的時間複雜度僅需 O(1)，且取用方便。

2. 貪婪演算法計算最少刪除操作次數

    本題無需特別注意 corner case，因此採用<b><span style="color:green">貪婪法 (greedy algorithm)</span></b>的想法，來計算最少需執行幾次的刪除操作，才可讓字串成為好字串。
    遍歷整個 hashmap，每當遇到重複的字元頻率，將其刪除之，並計一次刪除操作，直至<b><span style="color:green">沒有與其他字元重複頻率</span></b>或是<b><span style="color:green">字元完全被消除殆盡</span></b>。


### Pseudo Code
```
GIVEN: s is an input string.

## Create hashmap to record frequency of character of string
SET hashmap is a dictionary.

FOR each character c of s do
  hashmap[c] <- hashmap[c] + 1 
ENDFOR

## Greedy algorithm
SET cnt_tmp is a list.
SET ret is 0.

FOR each count cnt of hashmap do
  
  WHILE cnt > 0 and cnt in cnt_tmp
    cnt <- cnt - 1
    ret <- ret + 1
  ENDWHILE

  cnt_tmp <- cnt_tmp + [cnt]

ENDFOR

RETURN ret
```

### 複雜度分析
- 時間複雜度: O(N) ~ O(N) + O(N^2)
- 空間複雜度: O(N) + O(N) = O(N)


---
## 實際解題

### Python
```python
class Solution:
    def minDeletions(self, s: str) -> int:
        
        ## Create hashmap to record frequency of character of string
        hash_map = dict()
        for c in s:
            if c not in hash_map:
                hash_map[c] = 1
            else:
                hash_map[c] += 1

        ## Greedy algorithm
        cnt_tmp = list()
        ret = 0
        for c, cnt in hash_map.items():
            
            while cnt > 0 and cnt in cnt_tmp:
                cnt -= 1
                ret += 1
            cnt_tmp.append(cnt)
        
        return ret
```


---
## 討論區

尚整理
