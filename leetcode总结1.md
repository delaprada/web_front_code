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





## 环形链表

给定一个链表，判断链表中是否有环。

为了表示给定链表中的环，我们使用整数 pos 来表示链表尾连接到链表中的位置（索引从 0 开始）。 如果 pos 是 -1，则在该链表中没有环。

 ```
示例 1：

输入：head = [3,2,0,-4], pos = 1
输出：true
解释：链表中有一个环，其尾部连接到第二个节点。
 ```

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/07/circularlinkedlist.png)

```
示例 2：

输入：head = [1,2], pos = 0
输出：true
解释：链表中有一个环，其尾部连接到第一个节点。
```

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/07/circularlinkedlist_test2.png)



```
示例 3：

输入：head = [1], pos = -1
输出：false
解释：链表中没有环。
```

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/07/circularlinkedlist_test3.png)





思路：

- 环形链表题一般使用双指针法，这里可以使用快慢指针法。慢指针每次只前进一位，快指针每次前进两位，就像环形跑道上跑得快的选手必定能追上跑道上跑得慢的选手一样。如果能追上说明是环行链表。可以通过判断快指针和快指针的next指针是否为null来判断这是否为环行链表。
- 也可以用indexOf方法，像数组去重一样



题解：

```js
/**
 * Definition for singly-linked list.
 * function ListNode(val) {
 *     this.val = val;
 *     this.next = null;
 * }
 */

/**
 * @param {ListNode} head
 * @return {boolean}
 */
var hasCycle = function(head) {
    //快慢指针法，可以联想在环形跑道上快跑者始终会追上慢跑者
    if(head===null||head.next===null){
        return false;
    }
    // var res=[];
    // var cur=head;
    // while(cur){
    //     if(res.indexOf(cur)===-1){
    //         res.push(cur);
    //     }
    //     else{
    //         return true;
    //     }
    //     cur=cur.next;
    // }
    // return false;
    var slow=head;
    var fast=head.next;
    while(slow!==fast){
        if(fast===null||fast.next===null){
            return false;
        }
        slow=slow.next;
        fast=fast.next.next;
    }
    return true;
};
```





## 环形链表||

给定一个链表，返回链表开始入环的第一个节点。 如果链表无环，则返回 null。

为了表示给定链表中的环，我们使用整数 pos 来表示链表尾连接到链表中的位置（索引从 0 开始）。 如果 pos 是 -1，则在该链表中没有环。

说明：不允许修改给定的链表。

 ```
示例 1：

输入：head = [3,2,0,-4], pos = 1
输出：tail connects to node index 1
解释：链表中有一个环，其尾部连接到第二个节点。
 ```

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/07/circularlinkedlist.png)



```
示例 2：

输入：head = [1,2], pos = 0
输出：tail connects to node index 0
解释：链表中有一个环，其尾部连接到第一个节点。
```

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/07/circularlinkedlist_test2.png)



```
示例 3：

输入：head = [1], pos = -1
输出：no cycle
解释：链表中没有环。
```

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/07/circularlinkedlist_test3.png)





思路：

快慢指针是用于判断是否链表是环状，但是快指针和慢指针相遇的地方并不一定是链表的入口。所以可以使用indexOf方法，找到第一个重复的节点，就是入口。



题解：

```js
/**
 * Definition for singly-linked list.
 * function ListNode(val) {
 *     this.val = val;
 *     this.next = null;
 * }
 */

/**
 * @param {ListNode} head
 * @return {ListNode}
 */
var detectCycle = function(head) {
    if(head===null||head.next===null){
        return null;
    }
    var res=[];
    var cur=head;
    while(cur){
        if(res.indexOf(cur)===-1){
            res.push(cur);
        }
        else{
            return cur;
        }
        cur=cur.next;
    }
    return null;
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





## [面试题57 - II. 和为s的连续正数序列](https://leetcode-cn.com/problems/he-wei-sde-lian-xu-zheng-shu-xu-lie-lcof/)

输入一个正整数 target ，输出所有和为 target 的连续正整数序列（至少含有两个数）。

序列内的数字由小到大排列，不同序列按照首个数字从小到大排列。



示例 1：

```
输入：target = 9
输出：[[2,3,4],[4,5]]
```



示例 2：

```
输入：target = 15
输出：[[1,2,3,4,5],[4,5,6],[7,8]]
```



思路：

这种关于连续序列的题目的一个思路是可以用滑动窗口方式来解题。当前子序和如果比target要小的话，右指针++（也就是滑动窗口右结界向右移动），当前子序如果比target要大的话，左指针++（也就是滑动窗口左结界向右移动）。

需要注意的是，题目没有说这个正整数序列的范围，是需要我们去推断的。因为是求连续的和，而且至少含有两个数，那其中一个最大的数也只能是Math.ceil(target/2)。



题解：

```js
/**
 * @param {number} target
 * @return {number[][]}
 */
var findContinuousSequence = function(target) {
    var res=[];
    var ans=[];
    var count=1;
    for(var j=0;j<Math.ceil(target/2);j++){
        res[j]=count;
        count++;
    }
    var left=0;
    var right=1;
    var sum=0;
    while(left<right&&right<res.length){
        for(var i=left;i<=right;++i){
            sum+=res[i];
        }
        if(sum<target){
            right++;
        }
        else if(sum>target){
            left++;
        }
        else{
            ans.push(res.slice(left,right+1));
            right++;
        }
        temp=[];
        sum=0;
    }
    return ans;
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



## 回文数

判断一个整数是否是回文数。回文数是指正序（从左向右）和倒序（从右向左）读都是一样的整数。

```
示例 1:

输入: 121
输出: true

示例 2:

输入: -121
输出: false
解释: 从左向右读, 为 -121 。 从右向左读, 为 121- 。因此它不是一个回文数。

示例 3:

输入: 10
输出: false
解释: 从右向左读, 为 01 。因此它不是一个回文数。
```



思路：

回文数这题不难，主要是JavaScript中取模是和C++一样，但是做除法的时候JavaScript是可以有小数的，所以要使用Math.floor()。



题解：

```js
/**
 * @param {number} x
 * @return {boolean}
 */
var isPalindrome = function(x) {
    if(x<0){
        return false;
    }
    if(x==0){
        return true;
    }
    var res=[];
    var val=x;
    while(x){
        res.push(x%10);
        x=Math.floor(x/10);
    }
    res=Number(res.join(""));
    if(res==val){
        return true;
    }
    else{
        return false;
    }
};
```





## 删除排序数组的重复项

给定一个排序数组，你需要在原地删除重复出现的元素，使得每个元素只出现一次，返回移除后数组的新长度。

不要使用额外的数组空间，你必须在原地修改输入数组并在使用 O(1) 额外空间的条件下完成。

```
示例 1:

给定数组 nums = [1,1,2], 

函数应该返回新的长度 2, 并且原数组 nums 的前两个元素被修改为 1, 2。 

你不需要考虑数组中超出新长度后面的元素。
示例 2:

给定 nums = [0,0,1,1,1,2,2,3,3,4],

函数应该返回新的长度 5, 并且原数组 nums 的前五个元素被修改为 0, 1, 2, 3, 4。

你不需要考虑数组中超出新长度后面的元素。
```

说明:

为什么返回数值是整数，但输出的答案是数组呢?

请注意，输入数组是以“引用”方式传递的，这意味着在函数里修改输入数组对于调用者是可见的。



思路：

这题可以用splice方法。不过注意使用splice方法的时候数组的大小也会发生改变，所以i要--。



题解：

```js
/**
 * @param {number[]} nums
 * @return {number}
 */
var removeDuplicates = function(nums) {
    for(var i=1;i<nums.length;++i){
        if(nums[i]===nums[i-1]){
            nums.splice(i,1);
            i--;
        }
    }
    return nums.length;
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



## 最长公共前缀

编写一个函数来查找字符串数组中的最长公共前缀。

如果不存在公共前缀，返回空字符串 ""。

```
示例 1:

输入: ["flower","flow","flight"]
输出: "fl"
```



```
示例 2:

输入: ["dog","racecar","car"]
输出: ""
解释: 输入不存在公共前缀。
```



思路：

先求出数组中最短字符串的长度，因为最长公共前缀不会超过这个长度。然后两个for循环，依次比较。



题解：

```js
/**
 * @param {string[]} strs
 * @return {string}
 */
var longestCommonPrefix = function(strs) {
    if(strs.length===0){
        return "";
    }
    var res=[];
    var length=strs.length;
    var len1=strs[0].length;
    for(var i=0;i<length;++i){
        if(strs[i].length<len1){
            len1=strs[i].length;
        }
    }

    for(var i=0;i<len1;++i){
        res.push(strs[0][i]);
        for(var j=1;j<length;++j){
            if(strs[j][i]!==strs[0][i]){
                res.pop();
                return res.join("");
            }
        }
    }
    return res.join("");
};
```





## 二叉搜索树的最近公共祖先

给定一个二叉搜索树, 找到该树中两个指定节点的最近公共祖先。

百度百科中最近公共祖先的定义为：“对于有根树 T 的两个结点 p、q，最近公共祖先表示为一个结点 x，满足 x 是 p、q 的祖先且 x 的深度尽可能大（一个节点也可以是它自己的祖先）。”

例如，给定如下二叉搜索树:  root = [6,2,8,0,4,7,9,null,null,3,5]

 ![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/14/binarysearchtree_improved.png)

```
示例 1:

输入: root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 8
输出: 6 
解释: 节点 2 和节点 8 的最近公共祖先是 6。
```



```
示例 2:

输入: root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 4
输出: 2
解释: 节点 2 和节点 4 的最近公共祖先是 2, 因为根据定义最近公共祖先节点可以为节点本身。
```

 

说明:

    所有节点的值都是唯一的。
    p、q 为不同节点且均存在于给定的二叉搜索树中。



思路：

因为是二叉搜索树，所以如果两个节点的值中一个节点值比当前根节点的值要大，一个节点值比当前根节点的值要小，则当前根节点的值就是最近公共祖先节点。如果都比当前根节点小，则要去递归左节点（即都在左子树）；如果都比当前根节点大，则要去递归右节点（即都在右子树）。



题解：

```js
/**
 * Definition for a binary tree node.
 * function TreeNode(val) {
 *     this.val = val;
 *     this.left = this.right = null;
 * }
 */
/**
 * @param {TreeNode} root
 * @param {TreeNode} p
 * @param {TreeNode} q
 * @return {TreeNode}
 */
var lowestCommonAncestor = function F(root, p, q) {
    var parentVal=root.val;
    if((p.val<parentVal&&q.val>parentVal)||(p.val>parentVal&&q.val<parentVal)||p.val===parentVal||q.val===parentVal){
        return root;
    }
    if(p.val<parentVal&&q.val<parentVal){
        return F(root.left,p,q);
    }
    if(p.val>parentVal&&q.val>parentVal){
        return F(root.right,p,q);
    }
};
```





## 二叉树的最近公共祖先

给定一个二叉树, 找到该树中两个指定节点的最近公共祖先。

百度百科中最近公共祖先的定义为：“对于有根树 T 的两个结点 p、q，最近公共祖先表示为一个结点 x，满足 x 是 p、q 的祖先且 x 的深度尽可能大（一个节点也可以是它自己的祖先）。”

例如，给定如下二叉树:  root = [3,5,1,6,2,0,8,null,null,7,4]

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/15/binarytree.png)

 ```
示例 1:

输入: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 1
输出: 3
解释: 节点 5 和节点 1 的最近公共祖先是节点 3。
 ```



```
示例 2:

输入: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 4
输出: 5
解释: 节点 5 和节点 4 的最近公共祖先是节点 5。因为根据定义最近公共祖先节点可以为节点本身。
```



说明:

    所有节点的值都是唯一的。
    p、q 为不同节点且均存在于给定的二叉树中。



思路：

递归左右子树，如果在左右子树都有找到节点的话，说明当前结点就是最近公共祖先；如果只在左边找到节点，那说明都在左子树，递归左子树；如果只在右边找到节点，那说明都在右子树，递归右子树。



```js
/**
 * Definition for a binary tree node.
 * function TreeNode(val) {
 *     this.val = val;
 *     this.left = this.right = null;
 * }
 */
/**
 * @param {TreeNode} root
 * @param {TreeNode} p
 * @param {TreeNode} q
 * @return {TreeNode}
 */

var lowestCommonAncestor = function find(root, p, q) {
    if(root==null){
        return null;
    }
    if(p==root||q==root){
        return root;
    }
    var left=find(root.left,p,q);
    var right=find(root.right,p,q);
    if(left!==null&&right!==null){
        return root;
    }
    return left===null? right:left;
};
```



**注意这两题的不同点：第一题中是通过将两个节点的值与当前结点的值比较大小，来去判断是递归左节点还是右节点；第二题是通过递归去知道两个节点是在当前节点的左子树还是右子树，来去判断是递归左节点还是右节点。因为二叉搜索树可以直接通过值去知道两个节点在当前节点的左子树还是右子树，但是二叉树则不行，所以需要多做两次递归操作。**





## 全排列

给定一个没有重复数字的序列，返回其所有可能的全排列。

示例:

```
输入: [1,2,3]
输出:
[
  [1,2,3],
  [1,3,2],
  [2,1,3],
  [2,3,1],
  [3,1,2],
  [3,2,1]
]
```



思路：

通过循环+递归来实现。

<img src="C:\Users\alice\AppData\Roaming\Typora\typora-user-images\image-20200225105215378.png" alt="image-20200225105215378" style="zoom: 33%;" />

当你执行permutation(result,abc,0,3)的时候，回去递归permutation(result,abc,1,3)，执行permutation(result,abc,1,3)的时候，又会进入循环执行递归permutation(result,abc,2,3)，执行permutation(result,abc,2,3)的时候又会进入递归permutation(result,abc,3,3)，然后因为i++之后变为3,3不小于字符串的大小，所以就会将这个字符串push到结果当中。

整个算法的关键就是swap函数，将两个不同的位上的数进行交换。



```c++
class Solution {
public:
    void permutation(vector<vector<int>>& res, vector<int> nums, int index, int len){
        if(index==len){
            res.push_back(nums);
        }
        for(int i=index;i<len;++i){
            if(i!=index&&nums[i]==nums[index]) continue; //如果两位上的数是一样的话，就不用进行交换了
            swap(nums[i],nums[index]);
            permutation(res,nums,index+1,len);
        }
    }

    vector<vector<int>> permute(vector<int>& nums) {
        vector<vector<int>> res;
        int len=nums.size();
        permutation(res,nums,0,len);
        return res;
    }
    
};
```





## 搜索旋转的排序数组

假设按照升序排序的数组在预先未知的某个点上进行了旋转。

( 例如，数组 [0,1,2,4,5,6,7] 可能变为 [4,5,6,7,0,1,2] )。

搜索一个给定的目标值，如果数组中存在这个目标值，则返回它的索引，否则返回 -1 。

你可以假设数组中不存在重复的元素。

你的算法时间复杂度必须是 **O(log n)** 级别。

```
示例 1:
输入: nums = [4,5,6,7,0,1,2], target = 0
输出: 4

示例 2:
输入: nums = [4,5,6,7,0,1,2], target = 3
输出: -1
```



思路：

这题的难点在于时间复杂度要是O(logn)级别的，可以联想，在查找元素的时候什么时候会是O(logn)级别的呢？可以想到是二分查找（折半查找）。但是由于数组是部分有序的，所以我们不能简单的通过比较target与mid元素的大小来判断是查找左边还是右边，我们要先判断哪边是有序的。

判断有序的方式：比较nums[left]和nums[mid]的大小，如果nums[left]>nums[mid]也就是说左边是无序的，反之则有序。



题解：

```js
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number}
 */

//其实就是在二分法的基础上加上了判断左右升降序问题，因为数组现在是打乱了一下，直接判断是无法找到的
//题目中说明是log(n)复杂度算法而且是要找到某个数的话明显是二分法的思路
var search = function(nums, target) {
    var left=0;
    var right=nums.length-1;
    var mid=left+Math.floor((right-left)/2);
    while(left<=right){
        if(nums[mid]===target){
            return mid;
        }
        //左边如果是升序的话怎么判断
        if(nums[left]<=nums[mid]){
            if(target>=nums[left]&&target<=nums[mid]){
                right=mid-1;
            }
            else{
                left=mid+1;
            }
        }
        //右边如果是升序的话怎么判断
        else{
            if(target<=nums[right]&&target>=nums[mid]){
                left=mid+1;
            }
            else{
                right=mid-1;
            }
        }
        mid=Math.floor((right+left)/2);
    }
    return -1;
};
```



## 螺旋矩阵

给定一个包含 m x n 个元素的矩阵（m 行, n 列），请按照顺时针螺旋顺序，返回矩阵中的所有元素。

```
示例 1:
输入:
[
 [ 1, 2, 3 ],
 [ 4, 5, 6 ],
 [ 7, 8, 9 ]
]
输出: [1,2,3,6,9,8,7,4,5]

示例 2:
输入:
[
  [1, 2, 3, 4],
  [5, 6, 7, 8],
  [9,10,11,12]
]
输出: [1,2,3,4,8,12,11,10,9,5,6,7]
```



思路：

需要维护四个变量left，right，top，down，执行四轮循环，将数据push到结果数组中。因为它是螺旋矩阵，所以我们每轮循环需要对其中一个变量进行++或者--，控制输出的数据。



题解：

```c++
class Solution {
public:
    vector<int> spiralOrder(vector<vector<int>>& matrix) {
        vector<int> res;
        //判断为空这个条件不能漏掉
        if(matrix.empty()||matrix[0].empty()){
            return res;
        }
        int left=0;
        int right=matrix[0].size()-1;
        int top=0;
        int down=matrix.size()-1;
        while(left<=right&&top<=down){
            for(int i=left;i<=right;i++){
                res.push_back(matrix[top][i]);
            }
            top++;  //++之后，那下一轮循环就会从第二行开始，防止重复输出
            if(top>down){
                break;
            }
            for(int j=top;j<=down;++j){
                res.push_back(matrix[j][right]);
            }
            right--;
            if(left>right){
                break;
            }
            for(int k=right;k>=left;k--){
                res.push_back(matrix[down][k]);
            }
            down--;
            if(top>down){
                break;
            }
            for(int t=down;t>=top;t--){
                res.push_back(matrix[t][left]);
            }
            left++;
            if(left>right){
                break;
            }
        }
        return res;
    }
};
```



## 螺旋矩阵||

给定一个正整数 n，生成一个包含 1 到 n2 所有元素，且元素按顺时针顺序螺旋排列的正方形矩阵。

```
示例:

输入: 3
输出:
[
 [ 1, 2, 3 ],
 [ 8, 9, 4 ],
 [ 7, 6, 5 ]
]
```



思路：

这题的思路和上面一体一样，重点是结果数组的初始化。vector如何声明二维数组：

```
vector<vector<int>> res(n,vector<int>(n));
```

二维数组，n行，每行是一个大小为n的vector。



题解：

```c++
#include <iostream>
using namespace std;

class Solution {
public:
    vector<vector<int>> generateMatrix(int n) {
        vector<vector<int>> res(n,vector<int>(n));
        int count=1;
        int left=0;
        int right=n-1;
        int top=0;
        int down=n-1;
        while(left<=right&&top<=down){
            for(int i=left;i<=right;++i){
                res[top][i]=count++;
            }
            top++;
            for(int j=top;j<=down;j++){
                res[j][right]=count++;
            }
            right--;
            for(int k=right;k>=left;k--){
                res[down][k]=count++;
            }
            down--;
            for(int t=down;t>=top;t--){
                res[t][left]=count++;
            }
            left++;
        }
        return res;
    }
};
```



## 旋转链表

给定一个链表，旋转链表，将链表每个节点向右移动 k 个位置，其中 k 是非负数。

```js
示例 1:
输入: 1->2->3->4->5->NULL, k = 2
输出: 4->5->1->2->3->NULL
解释:
向右旋转 1 步: 5->1->2->3->4->NULL
向右旋转 2 步: 4->5->1->2->3->NULL

示例 2:
输入: 0->1->2->NULL, k = 4
输出: 2->0->1->NULL
解释:
向右旋转 1 步: 2->0->1->NULL
向右旋转 2 步: 1->2->0->NULL
向右旋转 3 步: 0->1->2->NULL
向右旋转 4 步: 2->0->1->NULL
```



思路：

做题时很容易想到的做法是，每轮都找到最末尾的节点，然后将这个节点之前的节点指向null，然后将这个节点指向头结点。但是如果k很大，那就做了很多无用功，因为会执行很多重复的操作。我们只需要求出整个链表最终的结果就可以了。

我们的做法是：先将最末尾的节点指向头结点，形成环形链表，因为最后的头结点应该是第(n-k%n)个结点，最后的尾结点应该是第(n-k%n-1)个结点。



题解：

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* rotateRight(ListNode* head, int k) {
        if(head==NULL||head->next==NULL){
            return head;
        }
        ListNode* cur=head;
        int n=1;  //求链表的长度
        while(cur->next!=NULL){
            cur=cur->next;
            n++;
        }
        ListNode* temp=cur;
        temp->next=head;  //形成环形链表
        ListNode* new_tail=head;
        for(int i=0;i<(n-k%n-1);i++){
            new_tail=new_tail->next;
        }
        ListNode* new_head=new_tail->next;
        new_tail->next=NULL;
        return new_head;

        // 超时方法
        // while(k--){
        //     ListNode* cur=head;
        //     while(cur->next->next!=NULL){
        //         cur=cur->next;
        //     }
        //     ListNode* temp=cur->next;
        //     cur->next=NULL;
        //     temp->next=head;
        //     head=temp;
        // }
        // return head;
    }
};
```

