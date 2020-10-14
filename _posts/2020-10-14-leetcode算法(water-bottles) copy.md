---
categories:
 - 算法
tags:
 - leetcode
 - js
---

### 题目 
 [https://leetcode.com/problems/water-bottles/](https://leetcode.com/problems/water-bottles/)

 - 难度
 *简单*
 - 说明
*Given numBottles full water bottles, you can exchange numExchange empty water bottles for one full water bottle.*
*The operation of drinking a full water bottle turns it into an empty bottle.*
*Return the maximum number of water bottles you can drink.*

 - 示例

``` bash
Input: numBottles = 9, numExchange = 3
Output: 13
Explanation: You can exchange 3 empty bottles to get 1 full water bottle.
Number of water bottles you can drink: 9 + 3 + 1 = 13.
 ```

``` bash
Input: numBottles = 15, numExchange = 4
Output: 19
Explanation: You can exchange 4 empty bottles to get 1 full water bottle. 
Number of water bottles you can drink: 15 + 3 + 1 = 19.
 ```
```bash
Input: numBottles = 5, numExchange = 5
Output: 6
```
### 解题

``` js
/**
 * @param {number} numBottles
 * @param {number} numExchange
 * @return {number}
 */
var numWaterBottles = function(numBottles, numExchange) {
    if(numBottles<numExchange){
       return numBottles;
    }
    if(numBottles==numExchange){
       return numBottles+1;
    }
    let result=numBottles,swap=0,m;
    while(numBottles>=numExchange){
       let temp=Math.floor(numBottles/numExchange);
        result+=temp;
        swap=numBottles%numExchange+temp;
        numBottles=swap;
    }
    return result;
};
```
