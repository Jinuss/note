---
categories:
 - 算法
tags:
 - leetcode
 - js
---

### 题目 
 [https://leetcode.com/problems/x-of-a-kind-in-a-deck-of-cards/](https://leetcode.com/problems/x-of-a-kind-in-a-deck-of-cards/)

 - 难度
 *简单*
 - 说明
*In a deck of cards, each card has an integer written on it.*
*Return true if and only if you can choose X >= 2 such that it is possible to split the entire deck into 1 or more groups of cards, where:*
 - *Each group has exactly X cards.*
 - *All the cards in each group have the same integer.*

 - 示例

``` bash
Input: deck = [1,2,3,4,4,3,2,1]
Output: true
Explanation: Possible partition [1,1],[2,2],[3,3],[4,4].
 ```

``` bash
Input: deck = [1,1,1,2,2,2,3,3]
Output: false´
Explanation: No possible partition.
 ```
```bash
Input: deck = [1,1]
Output: true
Explanation: Possible partition [1,1].
```
### 解题

``` js
/**
 * @param {number[]} deck
 * @return {boolean}
 */
var hasGroupsSizeX = function(deck) {
    if(deck.length==1){
       return false;
    }
    deck.sort((i,j)=>i-j);
    let tmp={};
    deck.map((i,j)=>{
        if(tmp[i]==undefined){
           tmp[i]=0;
        }
          tmp[i]++
    })
    let a=Object.values(tmp);
    let min=Math.min(...a);
    let result=a.reduce((i,j)=>{
        while (i%j != 0){
         t = i% j;
         i = j;
         j = t;
        }
        return j;
    })
    
    return result!=1;
};
```
