---
categories:
 - 算法
tags:
 - leetcode
 - js
---

### 题目 
 [https://leetcode.com/problems/split-a-string-in-balanced-strings/](https://leetcode.com/problems/split-a-string-in-balanced-strings/)

 - 难度
 *简单*
 - 说明
*Balanced strings are those who have equal quantity of 'L' and 'R' characters.*
*Given a balanced string s split it in the maximum amount of balanced strings.*
*Return the maximum amount of splitted balanced strings.*

 - 示例

``` bash
Input: s = "RLRRLLRLRL"
Output: 4
Explanation: s can be split into "RL", "RRLL", "RL", "RL", each substring contains same number of 'L' and 'R'.
 ```

``` bash
Input: s = "RLLLLRRRLR"
Output: 3
Explanation: s can be split into "RL", "LLLRRR", "LR", each substring contains same number of 'L' and 'R'.
 ```
```bash
Input: s = "LLLLRRRR"
Output: 1
Explanation: s can be split into "LLLLRRRR".
```
### 解题

``` js
/**
 * @param {string} s
 * @return {number}
 */
var balancedStringSplit = function(num) {
    let hour={
        0:[0],
        1:[1,2,4,8],
        2:[3,5,6,9,10],
        3:[7,11]
    }；
    let minute={
        0:[0],
        1:[1,2,4,8,16,32],
        2:[3,4,5,6,9,10,12,17,18,20,24,33,34,36,40,48],
        3:[7,11,13,14,17,37,38,41,42,44,49,50,52,56],
        4:[15,],
        5:[31,57,58,59]
    }
};
    return result.length;
};
