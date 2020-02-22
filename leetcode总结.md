# leetcode总结

## 三数之和

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



## 最接近的三数之和

给定一个包括 n 个整数的数组 nums 和 一个目标值 target。找出 nums 中的三个整数，使得它们的和与 target 最接近。返回这三个数的和。假定每组输入只存在唯一答案。

```
例如，给定数组 nums = [-1，2，1，-4], 和 target = 1.

与 target 最接近的三个数的和为 2. (-1 + 2 + 1 = 2).
```



思路：这题和上面的三数之和是同样的思路。也是使用双指针，只不过判断条件有些不同。如果三数之和与target的差小于0的话，首先要判断两者的差值是否比最小差值要小，如果小就替换，同时说明我需要left++来去逼近这个target值（即使得，三数之和更大）；如果三数之和与target的差大于0，首先要判断两者的差值是否比最小差值要小，如果小就替换，同时说明我需要right--来去逼近这个target值（即使得，三数之和更小）。

关键点：while(left<right)这个循环条件不要忘了呀



题解：

```js
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number}
 */
var threeSumClosest = function(nums, target) {
    var diff=99999999;
    nums.sort(function(a,b){
        return a-b;
    })
    var res;
    var len=nums.length;
    for(var i=0;i<len;++i){
        var left=i+1;
        var right=len-1;
        while(left<right){
            if(nums[i]+nums[left]+nums[right]-target<0){
                if(Math.abs(nums[i]+nums[left]+nums[right]-target)<diff){
                    diff=Math.abs(nums[i]+nums[left]+nums[right]-target);
                    res=nums[i]+nums[left]+nums[right];
                }
                left++;
            }
            else {
                if(nums[i]+nums[left]+nums[right]-target<diff){
                    diff=nums[i]+nums[left]+nums[right]-target;
                    res=nums[i]+nums[left]+nums[right];
                }
                right--;
            }
        } 
    }
    return res;
};
```





## 盛最多水的容器

给定 n 个非负整数 a1，a2，...，an，每个数代表坐标中的一个点 (i, ai) 。在坐标内画 n 条垂直线，垂直线 i 的两个端点分别为 (i, ai) 和 (i, 0)。找出其中的两条线，使得它们与 x 轴共同构成的容器可以容纳最多的水。

说明：你不能倾斜容器，且 n 的值至少为 2。

图中垂直线代表输入数组 [1,8,6,2,5,4,8,3,7]。在此情况下，容器能够容纳水（表示为蓝色部分）的最大值为 49。

 

![img](https://aliyun-lc-upload.oss-cn-hangzhou.aliyuncs.com/aliyun-lc-upload/uploads/2018/07/25/question_11.jpg)



```
示例:

输入: [1,8,6,2,5,4,8,3,7]
输出: 49
```



思路：也是使用双指针法。左右各设置一个指针。如果左边指针的值比右边指针的值要小，则左边指针++，因为要去寻找更大的乘积；右边也是同样的道理，如果右边指针的值比左边指针的值要小，则右边指针--。每一轮都会求一次乘积，如果乘积更大则替换最大值。



题解：

```js
/**
 * @param {number[]} height
 * @return {number}
 */
var maxArea = function(height) {
    var max=0;
    var res=0;
    var left=0;
    var right=height.length;
    var h;
    var w;
    while(left<right){
        var w=right-left;
        if(height[left]<height[right]){
            h=height[left];
            left++;
        }
        else{
            h=height[right];
            right--;
        }
        res=w*h;
        if(res>max){
            max=res;
        }
    }
    return max;
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





## 买卖股票的最佳时机||

给定一个数组，它的第 i 个元素是一支给定股票第 i 天的价格。

设计一个算法来计算你所能获取的最大利润。你可以尽可能地完成更多的交易（多次买卖一支股票）。

注意：你不能同时参与多笔交易（你必须在再次购买前出售掉之前的股票）。

示例 1:

```
输入: [7,1,5,3,6,4]
输出: 7
解释: 在第 2 天（股票价格 = 1）的时候买入，在第 3 天（股票价格 = 5）的时候卖出, 这笔交易所能获得利润 = 5-1 = 4 。
     随后，在第 4 天（股票价格 = 3）的时候买入，在第 5 天（股票价格 = 6）的时候卖出, 这笔交易所能获得利润 = 6-3 = 3 。
```

示例 2:

```
输入: [1,2,3,4,5]
输出: 4
解释: 在第 1 天（股票价格 = 1）的时候买入，在第 5 天 （股票价格 = 5）的时候卖出, 这笔交易所能获得利润 = 5-1 = 4 。
     注意你不能在第 1 天和第 2 天接连购买股票，之后再将它们卖出。
     因为这样属于同时参与了多笔交易，你必须在再次购买前出售掉之前的股票。
```

示例3：

```
输入: [7,6,4,3,1]
输出: 0
解释: 在这种情况下, 没有交易完成, 所以最大利润为 0。
```



```js
/**
 * @param {number[]} prices
 * @return {number}
 */
var maxProfit = function(prices) {
    //可以使用贪心算法，只要今天比昨天收益大，就卖出
    sum=0
    for(var i=1;i<prices.length;++i){
        if(prices[i]>prices[i-1]){
            sum+=prices[i]-prices[i-1];
        }
    }
    return sum;
};
```

可以使用贪心算法，只要今天比昨天收益大，就卖出。很简单的呃....





## 约瑟夫圆环

约瑟夫问题是个著名的问题：N个人围成一圈，第一个人从1开始报数，报M的将被杀掉，下一个人接着从1开始报。如此反复，最后剩下一个，求最后的胜利者。
 例如只有三个人，把他们叫做A、B、C，他们围成一圈，从A开始报数，假设报2的人被杀掉。

- 首先A开始报数，他报1。侥幸逃过一劫。
- 然后轮到B报数，他报2。非常惨，他被杀了
- C接着从1开始报数
- 接着轮到A报数，他报2。也被杀死了。
- 最终胜利者是C



思路：

最重要的一点是：最后留下来的人它的编号肯定是0。比如0,1,2,3,4,5，那第一个自杀的人是2（0报数1,1报数2,2报数3），那3就变为0的位置了，重新开始报数（报1）。

可以利用递归的思路。在人数为n时某个人的位置肯定是人数为n-1时的位置+3再取模n。可以使用上面的例子，2自杀后，1的序号就变为4，那它原本的位置可以由（4+3）%6=1得到。



题解：

```js
function yosef(n,m){
    if(n==1){
        //因为留下的最后一个人必定最后的位置是0
        return 0;
    }
    //原先的位置是之后的位置加三，因为例如012345，第一次2自杀了，那3就变为0了，那就是减了3。yosef(n)中的n表示的是人数为n时的位置，yosef(n-1)表示的是人数为n-1时的位置
    return (yosef(n-1,m)+m)%n;
}

var a=41;
var b=3;
console.log(yosef(41,3)+1);  //+1是因为0序号对应第一个人
```





