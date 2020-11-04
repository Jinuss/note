---
categories:
 - 算法
tags:
 - leetcode
 - js
---

### 题目 
 [https://leetcode.com/problems/can-place-flowers/](https://leetcode.com/problems/can-place-flowers/)

 - 难度
 *简单*
 - 说明
*Suppose you have a long flowerbed in which some of the plots are planted and some are not. However, flowers cannot be planted in adjacent plots - they would compete for water and both would die.*

*Given a flowerbed (represented as an array containing 0 and 1, where 0 means empty and 1 means not empty), and a number n, return if n new flowers can be planted in it without violating the no-adjacent-flowers rule.*

 - 示例

``` bash
Input: flowerbed = [1,0,0,0,1], n = 1
Output: True
 ```

``` bash
Input: flowerbed = [1,0,0,0,1], n = 2
Output: False
 ```
### 解题

``` js
/**
 * @param {number[]} flowerbed
 * @param {number} n
 * @return {boolean}
 */
var canPlaceFlowers = function(flowerbed, n) {
     function can(m){
        if(m<3){
           return 0  
        }
        return m%2==0?Math.floor((m-2)/2):Math.floor((m-1)/2);
    }
    let x=flowerbed.filter(i=>i==0)
    if(x.length==flowerbed.length){
        return can(x.length)+1>=n
    }
    let count=0,temp=[];
    flowerbed.map((i,index)=>{
        if((index==flowerbed.length-1||index==0)&&i==0){
            count++
         }
        if(i==0){
           count++;
        }
        if(i!=0||index==flowerbed.length-1){
           temp.push(count)
           count=0; 
        }
    })
    let total=0;
    temp.map((i,j)=>{
        total+=can(i);
    })
    return total>=n;
};
```
