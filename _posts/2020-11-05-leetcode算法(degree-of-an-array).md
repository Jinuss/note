---
categories:
 - 算法
tags:
 - leetcode
 - js
---

### 题目 
 [https://leetcode.com/problems/degree-of-an-array/](https://leetcode.com/problems/degree-of-an-array/)

 - 难度
 *简单*
 - 说明
*Given a non-empty array of non-negative integers nums, the degree of this array is defined as the maximum frequency of any one of its elements.*

*Your task is to find the smallest possible length of a (contiguous) subarray of nums, that has the same degree as nums.*

 - 示例

``` bash
Input: nums = [1,2,2,3,1]
Output: 2
Explanation: 
The input array has a degree of 2 because both elements 1 and 2 appear twice.
Of the subarrays that have the same degree:
[1, 2, 2, 3, 1], [1, 2, 2, 3], [2, 2, 3, 1], [1, 2, 2], [2, 2, 3], [2, 2]
The shortest length is 2. So return 2.
 ```

``` bash
Input: nums = [1,2,2,3,1,4,2]
Output: 6
Explanation: 
The degree is 3 because the element 2 is repeated 3 times.
So [2,2,3,1,4,2] is the shortest subarray, therefore returning 6.
 ```
### 解题

``` js
/**
 * @param {number[]} nums
 * @return {number}
 */
var findShortestSubArray = function(nums) {
       let arr=[];
    let lenObj={},maxLen=0;
    nums.map((i,j)=>{
        if(lenObj[i]==undefined){
           lenObj[i]=0;
           let arr=nums.filter(k=>k==i);
           lenObj[i]=arr.length;
        }
    })
    for(let key in lenObj){
        maxLen=Math.max(maxLen,lenObj[key])
    }
    for(let key in lenObj){
        if(maxLen==lenObj[key]){
           let tmp={val:Number(key),len:maxLen,distinct:0}
           let m=nums.lastIndexOf(tmp.val),n=nums.indexOf(tmp.val)
           tmp.distinct=m-n+1;
           arr.push(tmp);
        }
    }
    arr.sort((i,j)=>i.distinct-j.distinct)
    return arr[0].distinct;
};
```
