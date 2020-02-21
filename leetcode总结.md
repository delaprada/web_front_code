# leetcode总结

### 三数之和

给定一个包含 n 个整数的数组 nums，判断 nums 中是否存在三个元素 a，b，c ，使得 a + b + c = 0 ？找出所有满足条件且不重复的三元组。

注意：答案中不可以包含重复的三元组。

 ```
示例：

给定数组 nums = [-1, 0, 1, 2, -1, -4]，

满足要求的三元组集合为：
[
  [-1, 0, 1],
  [-1, -1, 2]
]
 ```

思路：

双指针法，先将数组排序。一个for循环，每轮将nums[i]作为sum中的第一个数，两个指针分别指向nums[i]后一个数和最后一个数，然后通过比较大小，对指针的位置进行调节。

联想：

如果题目为两数之和，并且返回的是数的话，也可以用双指针法（先将数组排序），不用两个for循环，时间复杂度上会降低。



题解：

```js
/**
 * @param {number[]} nums
 * @return {number[][]}
 */

var threeSum = function(nums) {
    nums.sort(function(a,b){
        return a-b;
    });//这个sort!!!
    var arr=nums;
    var len=nums.length;
    var res=[];
    if(len<3){
        return res;
    }
    for(var i=0;i<arr.length;++i){
        if(arr[i]>0){
            return res;
        }
        if(i>0&&arr[i]==arr[i-1]){
            continue;
        }
        var left=i+1;
        var right=len-1;
        while(left<right){
            if((arr[i]+arr[left]+arr[right])==0){
               res.push([arr[i],arr[left],arr[right]]);
               while(arr[left]==arr[left+1]&&left<right){
                   left++;
               } 
               while(arr[right]==arr[right-1]&&right>left){
                   right--;
               }
                left++;
                right--;
            }
            else if(arr[i]+arr[left]+arr[right]>0){
                right--;
            }
            else{
                left++;
            } 
        }
    }
    return res;
};
```







## 两数之和

给定一个整数数组 nums 和一个目标值 target，请你在该数组中找出和为目标值的那 两个 整数，并返回他们的数组下标。

你可以假设每种输入只会对应一个答案。但是，你不能重复利用这个数组中同样的元素。

```
示例:

给定 nums = [2, 7, 11, 15], target = 9

因为 nums[0] + nums[1] = 2 + 7 = 9
所以返回 [0, 1]
```

思路：

这个两数之和因为要返回的是索引，而不是值，如果预排序的话就会将数组里元素的顺序打乱，不能得到原本的索引。所以这里使用的是hashmap的方法，因为JavaScript的数组使用起来更加灵活，所以可以实现一个hashmap的方法。将数值和索引对应起来。

循环一个数组，遍历到当前元素的时候，看target和当前元素的差值是否已经在结果数组中有值了，如果有，则说明找到了两个数的和是target值。





题解：

```js
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number[]}
 */
var twoSum = function(nums, target) {
    var res=[];
    //一个循环
    for(let i=0;i<nums.length;++i){
        diff=target-nums[i];
        if(res[diff]!=undefined){
            return[res[diff],i];
        }
        res[nums[i]]=i;
    }    
};
```





## 无重复字符的最长子串

给定一个字符串，请你找出其中不含有重复字符的 最长子串 的长度。

```
示例 1:

输入: "abcabcbb"
输出: 3 
解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。
示例 2:

输入: "bbbbb"
输出: 1
解释: 因为无重复字符的最长子串是 "b"，所以其长度为 1。
示例 3:

输入: "pwwkew"
输出: 3
解释: 因为无重复字符的最长子串是 "wke"，所以其长度为 3。
     请注意，你的答案必须是 子串 的长度，"pwke" 是一个子序列，不是子串。
```



思路：

这道题属于滑动窗口类型的题目。滑动窗口类型一般一层循环就可以了。如果用原始的思路可能需要两层循环，比如遇到bcaa这种情况，当我们遇到第二个a的时候就停止判断了，但是下一轮循环从c开始又重复判断了caa，这就做了无用功。所以滑动窗口它的用途是将窗口的左边滑动到这个字符串没有重复字符的位置。比如baca的话，那窗口左侧会滑动到ca，再进行判断。



题解：

```js
/**
 * @param {string} s
 * @return {number}
 */
var lengthOfLongestSubstring = function(s) {
    var res=[];
    if(s===""){
        return 0;
    }
    s=s.split("");
    var left=0;
    var cur=0;
    var max=0;
    for(var i=0;i<s.length;++i){
        cur++;
        while(res.indexOf(s[i])!==-1){
            res.shift(s[left]);
            left++;
            cur--;
        }
        if(cur>max){
            max=cur;
        }
        res.push(s[i]);
    }
    return max;
};
```





## 最大子序和

给定一个整数数组 nums ，找到一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。

```
示例:

输入: [-2,1,-3,4,-1,2,1,-5,4],
输出: 6
解释: 连续子数组 [4,-1,2,1] 的和最大，为 6。
```



思路：

类似滑动窗口的思路。用一个变量来记录当前的和，如果某个数的值比当前的和要大的话，那就不需要考虑之前的值了，把当前的和记录为这个数的值，然后再进行后续的计算。



```js
/**
 * @param {number[]} nums
 * @return {number}
 */
var maxSubArray = function(nums) {
    if(nums.length===1){
        return nums[0];
    }
    var ans=nums[0];
    var sum=nums[0];
    for(var i=1;i<nums.length;++i){
        sum+=nums[i];
        if(nums[i]>sum){
            sum=nums[i];
        }
        if(sum>ans){
            ans=sum;
        }
    }
    return ans;
};
```





## 最长回文子串

给定一个字符串 `s`，找到 `s` 中最长的回文子串。你可以假设 `s` 的最大长度为 1000。

```
示例 1：

输入: "babad"
输出: "bab"
注意: "aba" 也是一个有效答案。
示例 2：

输入: "cbbd"
输出: "bb"
```



思路：

首先要找到一个pivot值，它的前后元素是相等的（或者它和后面的元素是相等的），然后进行回文的判断。



题解：

```js
/**
 * @param {string} s
 * @return {string}
 */
var longestPalindrome = function(s) {
    var max=1;
    s=s.split("");
    var res=[];
    res.push(s[0]);
    var cur;
    var p;
    var q;
    for(var i=0;i<s.length;++i){
        if(s[i]===s[i+1]){
            p=i;
            q=i+1;
            while(s[p]===s[q]&&p>=0&&q<s.length){
                p--;
                q++;
            }
            p++;
            cur=s.slice(p,q);
            if(cur.length>max){
                max=cur.length;
                res=cur;
            }
        }
        if(i>0&&s[i-1]===s[i+1]){
            p=i-1;
            q=i+1;
            while(s[p]===s[q]&&p>=0&&q<s.length){
                p--;
                q++;
            }
            p++;
            cur=s.slice(p,q);
            if(cur.length>max){
                max=cur.length;
                res=cur;
            }
        }
    }
    return res.join("");
};
```







