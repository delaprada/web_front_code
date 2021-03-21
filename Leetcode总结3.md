# Leetcode3

## [200. 岛屿数量](https://leetcode-cn.com/problems/number-of-islands/)

给你一个由 '1'（陆地）和 '0'（水）组成的的二维网格，请你计算网格中岛屿的数量。

岛屿总是被水包围，并且每座岛屿只能由水平方向和/或竖直方向上相邻的陆地连接形成。

此外，你可以假设该网格的四条边均被水包围。

示例 1:

```
输入:
11110
11010
11000
00000
输出: 1
```



示例 2:

```
输入:
11000
11000
00100
00011
输出: 3
解释: 每座岛屿只能由水平和/或竖直方向上相邻的陆地连接而成。
```



思路：

是典型的`dfs`问题。每个点都会去探索它四个方向上的点，四个方向上的点又按照深度优先遍历的方式去探索它四个方向上的点，当一次递归完成后，岛屿数量就+1。而且遍历完的点要将它变为‘2’（除'1'以外的数），这样就不会重复判断了。



题解：

```js
/**
 * @param {character[][]} grid
 * @return {number}
 */
var numIslands = function(grid) {
    var landcnt=0;
    for(var i=0;i<grid.length;++i){
        for(var j=0;j<grid[0].length;++j){
            if(grid[i][j]=='1'){
                dfs(grid,i,j);
                landcnt++;
            }
        }
    }
    return landcnt;
};

function dfs(grid, i, j){
    if(i<0||i>=grid.length||j<0||j>=grid[0].length||grid[i][j]!='1'){
        return;
    }
    grid[i][j]='2';
    dfs(grid,i+1,j);
    dfs(grid,i-1,j);
    dfs(grid,i,j+1);
    dfs(grid,i,j-1);
}
```





## [1248. 统计「优美子数组」](https://leetcode-cn.com/problems/count-number-of-nice-subarrays/)

给你一个整数数组 nums 和一个整数 k。

如果某个 连续 子数组中恰好有 k 个奇数数字，我们就认为这个子数组是「优美子数组」。

请返回这个数组中「优美子数组」的数目。

 

示例 1：

```
输入：nums = [1,1,2,1,1], k = 3
输出：2
解释：包含 3 个奇数的子数组是 [1,1,2,1] 和 [1,2,1,1] 。
```

示例 2：

```
输入：nums = [2,4,6], k = 1
输出：0
解释：数列中不包含任何奇数，所以不存在优美子数组。
```

示例 3：

```
输入：nums = [2,2,2,1,2,2,1,2,2,2], k = 2
输出：16
```




提示：

```
1 <= nums.length <= 50000
1 <= nums[i] <= 10^5
1 <= k <= nums.length
```



思路：

用一个odd数组存储每个奇数对应的索引。例如odd[1]=1表示第一个奇数对应的索引为1。那odd[i]和odd[i+k]之间保证是有三个奇数的，且odd[i-1]和odd[i]索引之间在真实数组中都是偶数，odd[i+k-1]和odd[i+k]之间在真实数组中都是偶数。我们的目的就是找到所有的子数组[l,r]，l在odd[i-1]和odd[i]之间，r在odd[i+k-1]和odd[i+k]之间，所有组合搭配。



题解：

```js
 */
var numberOfSubarrays = function(nums, k) {
    var odd=[];
    var cnt=0;
    var res=0;
    for(var i=0;i<nums.length;++i){
        if(nums[i]&1){
            odd[++cnt]=i;
        }
    }
    odd[0]=-1;
    odd[++cnt]=nums.length;
    for(var j=1;j+k<=cnt;++j){
        res+=(odd[j]-odd[j-1])*(odd[j+k]-odd[j+k-1]);
    }
    return res;
};
```



## [23. 合并K个排序链表](https://leetcode-cn.com/problems/merge-k-sorted-lists/)

合并 k 个排序链表，返回合并后的排序链表。请分析和描述算法的复杂度。

示例:

```
输入:
[
  1->4->5,
  1->3->4,
  2->6
]
输出: 1->1->2->3->4->4->5->6
```



思路：

合并k个排序链表的思路，可以由合并两个排序链表演变过来。先把第一个链表和第二个链表合并，再将合并后的结果和第三个链表合并，以此类推。合并两个链表可以使用递归方法，看起来更加简洁一些。



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
 * @param {ListNode[]} lists
 * @return {ListNode}
 */
var mergeKLists = function(lists) {
    var size = lists.length;
    if(size===0) {
        return null;
    }
    var res = lists[0];
    for(var i=1; i<size; ++i){
        res = merge(res, lists[i]);
    }
    return res;
};

function merge(p1, p2){
    if(p1===null){
        return p2;
    }
    if(p2===null){
        return p1;
    }
    if(p1.val<p2.val){
        p1.next = merge(p1.next, p2);
        return p1;
    }
    else{
        p2.next = merge(p1, p2.next);
        return p2;
    }
}
```



## [4. 寻找两个正序数组的中位数](https://leetcode-cn.com/problems/median-of-two-sorted-arrays/)

给定两个大小分别为 `m` 和 `n` 的正序（从小到大）数组 `nums1` 和 `nums2`。请你找出并返回这两个正序数组的 **中位数** 。

**示例 1：**

```
输入：nums1 = [1,3], nums2 = [2]
输出：2.00000
解释：合并数组 = [1,2,3] ，中位数 2
```

**示例 2：**

```
输入：nums1 = [1,2], nums2 = [3,4]
输出：2.50000
解释：合并数组 = [1,2,3,4] ，中位数 (2 + 3) / 2 = 2.5
```

**示例 3：**

```
输入：nums1 = [0,0], nums2 = [0,0]
输出：0.00000
```

**示例 4：**

```
输入：nums1 = [], nums2 = [1]
输出：1.00000
```

**示例 5：**

```
输入：nums1 = [2], nums2 = []
输出：2.00000
```



**进阶：**你能设计一个时间复杂度为 `O(log (m+n))` 的算法解决此问题吗？



题解：

这个问题一个常规的思路是：先将两个数组合并，然后排序，再分奇偶类型获得中位数。不过这个方法可能不能满足时间复杂度为O(log(m+n))，因为`js`的`sort`函数的解析方法是由各个引擎决定的。

```javascript
// 题解1
/**
 * @param {number[]} nums1
 * @param {number[]} nums2
 * @return {number}
 */
var findMedianSortedArrays = function(nums1, nums2) {
    const res = nums1.concat(nums2);
    res.sort((a, b) => (a - b));
    const len = res.length
    if(len % 2 === 0) {
        if(len !== 0) {
            const index = len / 2;
            return (res[index] + res[index - 1]) / 2;
        }
    } else {
        const index = Math.floor(len / 2);
        return res[index];
    }
};
```



第二个思路是：当我们看到它限制了时间复杂度为`O(log(m+n))`时，我们应该想到用二分法解决。所以这个题目可以归结为寻找第k大元素问题。思路如下：取两个数组中第`k/2`大的数进行比较，如果数组1的元素小于数组2的元素，则说明数组1中的前`k/2`个元素不可能成为第`k`个元素的候选，所以将数组1的前`k/2`个元素去掉，形成新的数组和数组2求第`k-k/2`大的元素。因为我们把前`k/2`个元素去掉了，所以相应的`k`值也应该减小。然后再注意一些边界条件：如其中一个数组可能为空或者k为1的情况。

```javascript
var findMedianSortedArrays = function(nums1, nums2) {
    const m = nums1.length;
    const n = nums2.length;
    const left = Math.floor((m + n + 1) / 2);
    const right = Math.floor((m + n + 2) / 2);
    
    // 分别找第(m + n + 1) / 2个和第(m + n + 2) / 2个元素以适配奇偶情况
    return (findKth(nums1, 0, nums2, 0, left) + findKth(nums1, 0, nums2, 0, right)) / 2;
};


// i和j分别用于标记两个数组的起始位置
const findKth = (nums1, i, nums2, j, k) => {
    // nums1为空数组
    if(i >= nums1.length) {
        return nums2[j + k - 1];
    }
    // nums2为空数组
    if(j >= nums2.length) {
        return nums1[i + k - 1];
    }
    if(k === 1) {
        return Math.min(nums1[i], nums2[j]);
    }
    
    // 取Number.MAX_VALUE的原因是当前这个数组的长度和另一个数组相比小很多，所以才会导致i + Math.floor(k / 2) - 1 < 	nums1.length，所以当前这个数组可以继续保留，直到其和另一个数组的长度相当时再进行比较，而且不会有影响。因为如果这个数组的值很小，那么下次比较的时候就会将这个数组前k/2个数据给排除
    const minVal1 = i + Math.floor(k / 2) - 1 < nums1.length ? nums1[i + Math.floor(k / 2) - 1] : Number.MAX_VALUE;
    const minVal2 = j + Math.floor(k / 2) - 1 < nums2.length ? nums2[j + Math.floor(k / 2) - 1] : Number.MAX_VALUE;

    if(minVal1 < minVal2) {
        return findKth(nums1, i + Math.floor(k / 2), nums2, j, k - Math.floor(k / 2));
    } else {
        return findKth(nums1, i, nums2, j + Math.floor(k / 2), k - Math.floor(k / 2));
    }
}
```



## [6. Z 字形变换](https://leetcode-cn.com/problems/zigzag-conversion/)

将一个给定字符串 s 根据给定的行数 numRows ，以从上往下、从左到右进行 Z 字形排列。

比如输入字符串为 "PAYPALISHIRING" 行数为 3 时，排列如下：

```
P   A   H   N
A P L S I I G
Y   I   R
```

之后，你的输出需要从左往右逐行读取，产生出一个新的字符串，比如：`"PAHNAPLSIIGYIR"`。

请你实现这个将字符串进行指定行数变换的函数：

```
string convert(string s, int numRows);
```



**示例 1：**

```
输入：s = "PAYPALISHIRING", numRows = 3
输出："PAHNAPLSIIGYIR"
```



**示例 2：**

```
输入：s = "PAYPALISHIRING", numRows = 4
输出："PINALSIGYAHRPI"
解释：
P     I    N
A   L S  I G
Y A   H R
P     I
```



**示例 3：**

```
输入：s = "A", numRows = 1
输出："A"
```



```javascript
/**
 * @param {string} s
 * @param {number} numRows
 * @return {string}
 */

// 找规律
var convert = function(s, numRows) {
    let temp = [];
    let res = "";
    for(let i = 0; i < numRows; ++i) {
        temp[i] = new Array();
    }

    if(s.length === 0 || numRows < 1) {
        return res;
    }

    if(numRows === 1) {
        return s;
    }

    for(let j = 0; j < s.length; ++j) {
        let ans = Math.floor(j / (numRows - 1));
        let cur = j % (numRows - 1);

        if(ans % 2 === 0) {
            temp[cur].push(s[j]); // 按余数正序保存
        }

        if(ans % 2 !== 0) {
            temp[numRows - 1 - cur].push(s[j]); // 按余数倒序保存
        }
    }
    for(let k = 0; k < temp.length; ++k) {
        res += temp[k].join('');
    }
    return res;
};
```

![](https://delaprada-1301716802.cos.ap-guangzhou.myqcloud.com/20210316101241.png)\





## [10. 正则表达式匹配](https://leetcode-cn.com/problems/regular-expression-matching/)

给你一个字符串 s 和一个字符规律 p，请你来实现一个支持 '.' 和 '*' 的正则表达式匹配。

'.' 匹配任意单个字符
'*' 匹配零个或多个前面的那一个元素
所谓匹配，是要涵盖 整个 字符串 s的，而不是部分字符串。



**示例 1：**

```
输入：s = "aa" p = "a"
输出：false
解释："a" 无法匹配 "aa" 整个字符串。
```

**示例 2:**

```
输入：s = "aa" p = "a*"
输出：true
解释：因为 '*' 代表可以匹配零个或多个前面的那一个元素, 在这里前面的元素就是 'a'。因此，字符串 "aa" 可被视为 'a' 重复了一次。
```

**示例 3：**

```
输入：s = "ab" p = ".*"
输出：true
解释：".*" 表示可匹配零个或多个（'*'）任意字符（'.'）。
```

**示例 4：**

```
输入：s = "aab" p = "c*a*b"
输出：true
解释：因为 '*' 表示零个或多个，这里 'c' 为 0 个, 'a' 被重复一次。因此可以匹配字符串 "aab"。
```

**示例 5：**

```
输入：s = "mississippi" p = "mis*is*p*."
输出：false
```



提示：

```
0 <= s.length <= 20
0 <= p.length <= 30
s 可能为空，且只包含从 a-z 的小写字母。
p 可能为空，且只包含从 a-z 的小写字母，以及字符 . 和 *。
保证每次出现字符 * 时，前面都匹配到有效的字符
```



题解：

这题可以用简单的`js`正则匹配实现`new RegExp(p).text(s)`，但如果想锻炼思维则应该思考其他的做法。

这种匹配类型的题目经常用到**递归**的思想，比如判断最长的回文子串，也是匹配字符串的变式。

在判断最长回文子串的例子中，我们通过遍历字符串的每个字符，获取以该字符作为中心的回文子串，再通过比较返回最长的子串。

这题里我们可以先判断第一个字符是否匹配正则，然后再递归除去第一个字符后的子串。

带`*`的情况需要分情况考虑：

1. 形如s为`aab`，p为`c*a*b`：s不匹配`c*`而是匹配后面的`a*b`
2. 形如s为`aa`，p为`a*`：s直接匹配p的`a*`



```javascript
/**
 * @param {string} s
 * @param {string} p
 * @return {boolean}
 */
var isMatch = function(s, p) {
    if(p.length === 0) {
        return s.length === 0;
    }

    const firstMatch = s.length !== 0 && (s[0] === p[0] || p[0] === '.');

    if(p.length >= 2 && p[1] === '*') {
        return isMatch(s, p.slice(2)) || (firstMatch && isMatch(s.slice(1), p));
    } else {
        return firstMatch && isMatch(s.slice(1), p.slice(1));
    }
};
```

