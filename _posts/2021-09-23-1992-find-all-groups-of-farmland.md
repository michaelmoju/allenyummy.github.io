---
title: 1992. Find All Groups of Farmland
author: Yu-Lun Chiang
date: 2021-09-23 11:00:00 +0800
categories: [Leetcode, An-Apple-Per-Day]
tags: [leetcode, medium, dfs]
math: true
---

---
## 題目描述

You are given a 0-indexed `m x n` binary matrix land where a `0` represents a hectare of forested land and a `1` represents a hectare of farmland.

To keep the land organized, there are designated rectangular areas of hectares that consist entirely of farmland. These rectangular areas are called groups. No two groups are adjacent, meaning farmland in one group is not four-directionally adjacent to another farmland in a different group.

`land` can be represented by a coordinate system where the top left corner of `land` is `(0, 0)` and the bottom right corner of `land` is `(m-1, n-1)`. Find the coordinates of the top left and bottom right corner of each group of farmland. A group of farmland with a top left corner at `(r1, c1)` and a bottom right corner at `(r2, c2)` is represented by the 4-length array `[r1, c1, r2, c2]`.

Return a 2D array containing the 4-length arrays described above for each group of farmland in `land`. If there are no groups of farmland, return an empty array. You may return the answer in any order.

### 白話文運動

給定一個 `m x n` 的二維陣列，其中元素由 0 與 1 組成。

在二維陣列中，由 1 組成了<b><span style="color:green">矩形區域</span></b> (不同矩形區域，左右上下不相鄰)。

每個矩形區域由<b><u><span style="color:red">左上座標</span></u></b>`[r1, c1]`與<b><u><span style="color:blue">右下座標</span></u></b>`[r2, c2]`代表之，形成 `[r1, c1, r2, c2]`。

請回傳所有的矩形區域。

### 題目限制

- m == land.length
- n == land[i].length
- 1 <= m, n <= 300
- land consists of only 0's and 1's.
- Groups of farmland are rectangular in shape.


---
## 具體測資

### Case 1

```
Input: land = [[1,0,0],[0,1,1],[0,1,1]]
Output: [[0,0,0,0],[1,1,2,2]]
Explanation:
The first group has a top left corner at land[0][0] and a bottom right corner at land[0][0].
The second group has a top left corner at land[1][1] and a bottom right corner at land[2][2].
```

### Case 2

```
Input: land = [[1,1],[1,1]]
Output: [[0,0,1,1]]
Explanation:
The first group has a top left corner at land[0][0] and a bottom right corner at land[1][1].
```

### Case 3

```
Input: land = [[0]]
Output: []
Explanation:
There are no groups of farmland.
```


---
## 解題想法

### 概念

遍歷二維陣列，並使用<b><span style="color:green">深度優先搜索 (DFS, Depth-First Search)</span></b>探索矩形區域之<b><span style="color:green">右下座標(座標最大值)</span></b>。

### Pseudo Code

```
GIVEN: land is a 2D array.

SET m is len(land).
SET n is len(land[0]).
SET visited_map is a 2D (mxn) array where all elements are False.
SET ret is a empty list.

FUNC dfs(i, j) do

    IF i >= m or i < 0 or j >= n or j < 0:
        RETURN (0, 0)
    ENDIF
    
    IF visited_map[i][j]:
        RETURN (0, 0)
    ENDIF
    
    IF land[i][j] == 0:
        RETURN (0, 0)
    ENDIF
    
    visited_map[i][j] = True
    RETURN max((i, j), dfs(i+1, j), dfs(i, j+1))

ENDFUNC


FOR i (0 to m) do

    FOR j (0 to n) do

        IF not visited_map[i][j] do

            IF land[i][j] == 1 do
                (p, q) <- dfs(i, j)
                ret <- ret + [i, j, p, q]
            ELSE
                visited_map[i][j] <- True
            ENDIF

        ENDIF

    ENDFOR

ENDFOR

RETURN ret
```

### 複雜度分析

- 時間複雜度: O(m*n)

- 空間複雜度: O(1) (不考慮回傳時所創建的矩陣)


---
## 實際解題

### Python

```python
class Solution:
    def findFarmland(self, land: List[List[int]]) -> List[List[int]]:
        
        m = len(land)
        n = len(land[0])
        visited_map = [[False] * n for _ in range(m)]
        
        ## 以 (i, j) 為矩形左上座標之視角，遍歷出矩形右下座標
        def dfs(i: int, j: int):
            if i >= m or i < 0 or j >= n or j < 0:
                return (0, 0)
            if visited_map[i][j]:
                return (0, 0)
            if land[i][j] == 0:
                return (0, 0)
            
            visited_map[i][j] = True
            return max((i, j), dfs(i+1, j), dfs(i, j+1))
        
        ret = list()
        for i in range(m):
            for j in range(n):
                ## 如果 (i, j) 還未遍歷過
                if not visited_map[i][j]:
                    ## 此時該點必為矩形左上座標
                    if land[i][j] == 1:
                        ## 找尋與該點配對之右下座標
                        (p, q) = dfs(i, j)
                        ret.append([i, j, p, q])
                    else:
                        visited_map[i][j] = True
        return ret
```


---
## 討論區


