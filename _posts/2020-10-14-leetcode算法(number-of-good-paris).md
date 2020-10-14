---
categories:
 - 算法
tags:
 - leetcode
 - js
---

### 题目 
 [https://leetcode.com/problems/number-of-good-pairs/](https://leetcode.com/problems/number-of-good-pairs/)

 - 难度
 *简单*
 - 说明
*Given an array of integers nums.*
*A pair (i,j) is called good if nums[i] == nums[j] and i < j.*
*Return the number of good pairs.*

 - 示例

``` bash
Input: nums = [1,2,3,1,1,3]
Output: 4
Explanation: There are 4 good pairs (0,3), (0,4), (3,4), (2,5) 0-indexed.
 ```

``` bash
Input: nums = [1,1,1,1]
Output: 6
Explanation: Each pair in the array are good.
 ```
```bash
Input: nums = [1,2,3]
Output: 0
```
### 解题

``` js
/**
 * @param {number[]} nums
 * @return {number}
 */
var numIdenticalPairs = function(nums) {
    let tmp=[],arr=[];
    nums.map((i,j)=>{
     tmp.push({val:i,index:j})
    })
    nums.map((i)=>{
        let m=tmp.filter(j=>j.val==i);
        let n=arr.filter(k=>k.val==i)
        if(m.length&&!n.length){
           arr.push({len:m.length,val:i});
        }
    })
    let count=0;
    arr.map((i)=>{
        count+=i.len*(i.len-1)/2;
    })
    return count;
};
```
