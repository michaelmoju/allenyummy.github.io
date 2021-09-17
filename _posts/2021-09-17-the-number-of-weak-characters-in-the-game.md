---
title: 1996. The Number of Weak Characters in the Game
author: Yu-Lun Chiang
date: 2021-09-17 13:40:00 +0800
categories: [Leetcode, An-Apple-Per-Day]
tags: [leetcode, medium, array]
math: true
---

---
## 題目描述

You are playing a game that contains multiple characters, and each of the characters has two main properties: <b><u><span style="color:red">attack</span></u></b> and <b><u><span style="color:blue">defense</span></u></b>. 

You are given a 2D integer array properties where <span style="color:green">properties[i]</span> = [<span style="color:red">attack<sub>i</sub></span>, <span style="color:blue">defense<sub>i</sub></span>] represents the properties of the i<sup>th</sup> character in the game.

A character is said to be <b><u><span style="color:green">weak</span></u></b> if any other character has both attack and defense levels <b><u><span style="color:green">strictly greater</span></u></b> than this character's attack and defense levels. 

More formally, a character i is said to be weak if there exists another character j where attack<sub>j</sub> > attack<sub>i</sub> and defense<sub>j</sub> > defense<sub>i</sub>.

Return the <b><u><span style="color:green">number</span></u></b> of weak characters.

### 白話文運動

給定一個二維矩陣，矩陣內的每個元素皆由一維陣列組成: <b><u><span style="color:red">攻擊</span></u></b> and <b><u><span style="color:blue">防禦</span></u></b>。

所謂<b><u><span style="color:green">弱元素</span></u></b>，意思是其攻擊與防禦值都比某個元素還要<b><u><span style="color:green">小</span></u></b>。

請回傳弱元素的數量。

### 題目限制

- 2 <= properties.length <= 10<sup>5</sup>
- properties[i].length == 2
- 1 <= attack<sub>i</sub>, defense<sub>i</sub> <= 10<sup>5</sup>


---
## 具體測資

### Case 1

```
Input  : properties = [[5,5],[6,3],[3,6]]
Output : 0
Explanation: No character has strictly greater attack and defense than the other.
```

### Case 2

```
Input  : properties = [[2,2],[3,3]]
Output : 1
Explanation: The first character is weak because the second character has a strictly greater attack and defense.
```

### Case 3

```
Input  : properties = [[1,5],[10,4],[4,3]]
Output : 1
Explanation: The third character is weak because the second character has a strictly greater attack and defense.
```

### Case 4

```
Input  : properties = [[5,5],[6,7],[5,6],[3,8],[3,4],[2,2]]
Output : 4
```

---
## 解題想法

### 概念

#### 暴力法

1. 遍歷整個二維矩陣，進行兩兩比對。

#### 快速解決法

1. 排序
        
    確保二為矩陣的順序，<b><span style="color:red">攻擊值</span></b>由大至小<b><span style="color:red">遞減</span></b>排列、<b><span style="color:blue">防禦值</span></b>由小至大<b><span style="color:blue">遞增</span></b>排列。

    ```
    properties = [[5,5],[6,7],[5,6],[3,8],[3,4],[2,2]]
    sorted_pro = [[6,7],[5,5],[5,6],[3,4],[3,8],[2,2]]
    ```

2. 遍歷排序的二維矩陣

    遍歷時，因為攻擊值由大至小遞減排列，因此，只需判斷<b><span style="color:green">當前元素的防禦值是否比當前最大防禦值還要小</span></b>，即可判定當前元素是否微弱元素。
    
    若較小，則當前元素即為弱元素。

    若較大，則更新最大防禦值。

    舉例來說:
    ```
    sorted_pro = [[6,7],[5,5],[5,6],[3,4],[3,8],[2,2]]
    -----
    curr_max_defense = 0

    [[6,7],[5,5],[5,6],[3,4],[3,8],[2,2]]
        ^
    -----
    curr_max_defense = 7

    [[6,7],[5,5],[5,6],[3,4],[3,8],[2,2]]
              ^
            weak
    -----
    curr_max_defense = 7

    [[6,7],[5,5],[5,6],[3,4],[3,8],[2,2]]
                    ^
                  weak
    -----
    curr_max_defense = 7

    [[6,7],[5,5],[5,6],[3,4],[3,8],[2,2]]
                          ^
                        weak
    -----
    curr_max_defense = 7

    [[6,7],[5,5],[5,6],[3,4],[3,8],[2,2]]
                                ^
    curr_max_defense = 8
    -----
    curr_max_defense = 8

    [[6,7],[5,5],[5,6],[3,4],[3,8],[2,2]]
                                      ^
                                    weak
    ```

### Pseudo Code

```
GIVEN: properties is a 2D array.

## Sort by attack value descendingly and sort by defense value ascendingly
sorted_properties <- Sort(properties)

## Iterate sorted_properties to count weak elements
SET cuur_max_defense is 0.
SET ret is 0.

FOR each (attack, defense) of sorted_properties do
  
  IF d < curr_max_defense do
    ret <- ret + 1
  ELSE
    curr_max_defense <- d
  ENDIF

ENDFOR

RETURN ret
```

### 複雜度分析

- 時間複雜度: O(NlogN)
  
1. 排序的平均時間複雜度為 O(NlogN)。

2. 遍歷的時間複雜度為 O(N)。

- 空間複雜度: O(1)


---
## 實際解題

### Python

```python
class Solution:
    def numberOfWeakCharacters(self, properties: List[List[int]]) -> int:
        
        sorted_p = sorted(properties, key=lambda k: (-k[0], k[1]))
        
        ret = 0
        curr_max = 0
        
        for _, d in sorted_p:
            if d < curr_max:
                ret += 1
            else:
                curr_max = d
        
        return ret
```


---
## 討論區

與上述想法一致。
