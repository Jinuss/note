---
categories:
 - 算法
tags:
 - leetcode
 - js
---

### 题目 
 [https://leetcode.com/problems/maximize-sum-of-array-after-k-negations/](https://leetcode.com/problems/maximize-sum-of-array-after-k-negations/)

 - 难度
 *简单*
 - 说明
*Given an array A of integers, we must modify the array in the following way: we choose an i and replace A[i] with -A[i], and we repeat this process K times in total.  (We may choose the same index i multiple times.)*

*Return the largest possible sum of the array after modifying it in this way.*

 - 示例

``` bash
Input: A = [3,-1,0,2], K = 3
Output: 6
Explanation: Choose indices (1, 2, 2) and A becomes [3,1,0,2].
 ```

``` bash
Input: A = [4,2,3], K = 1
Output: 5
Explanation: Choose indices (1,) and A becomes [4,-2,3].
 ```
```bash
Input: A = [2,-3,-1,5,-4], K = 2
Output: 13
Explanation: Choose indices (1, 4) and A becomes [2,3,-1,5,4].
```
### 解题

``` js
/**
 * @param {number[]} A
 * @param {number} K
 * @return {number}
 */
var largestSumAfterKNegations = function(A, K) {
    A.sort((i,j)=>i-j)
    let count=0,n=A.indexOf(0);//小于0,等于0的索引
    for(let i=0;i<A.length;i++){
       if(A[i]<0){
          count++;
       } 
    }
    let total=0;
    if(count>0){//存在负数       
      if(n>-1){//存在0
         if(count<=K){
           total=A.reduce((i,j)=>{
                return Math.abs(i)+Math.abs(j)
           })
         }else{
           let s=0; 
           while(s<K){
             A[s]=-A[s];
             s++;  
           }  
           total=A.reduce((i,j)=>i+j) 
         }
       }else{//不存在0
         if(count==K){
            total=A.reduce((i,j)=>{
                return Math.abs(i)+Math.abs(j)
            })  
         }else if(count<K){     
             y=K-count;
             A.map((i,index,arr)=>{
                 arr[index]=Math.abs(i)
             })
             A.sort((i,j)=>i-j)  
             total=A.reduce((i,j)=>i+j) 
             if(y%2!=0){
                total-=2*A[0]
              }     
         }else{
           let t=0;
           while(t<K){
             A[t]=-A[t];
             t++;  
           }  
           total=A.reduce((i,j)=>i+j)
         }  
       }
    }else{
      if(n>-1||K%2==0){//存在0或者K为偶数
         total=A.reduce((i,j)=>i+j)
       }else{//不存在0
         A[0]=-A[0];
         total=A.reduce((i,j)=>i+j)
       }
    }
    return total;
};
```
