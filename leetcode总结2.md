# leetcode总结2

## [字符串解码](https://leetcode-cn.com/problems/decode-string/)

给定一个经过编码的字符串，返回它解码后的字符串。

编码规则为: k[encoded_string]，表示其中方括号内部的 encoded_string 正好重复 k 次。注意 k 保证为正整数。

你可以认为输入字符串总是有效的；输入字符串中没有额外的空格，且输入的方括号总是符合格式要求的。

此外，你可以认为原始数据不包含数字，所有的数字只表示重复的次数 k ，例如不会出现像 3a 或 2[4] 的输入。

示例:

```
s = "3[a]2[bc]", 返回 "aaabcbc".
s = "3[a2[c]]", 返回 "accaccacc".
s = "2[abc]3[cd]ef", 返回 "abcabccdcdcdef".
```



思路：

这种题目很容易会联想到递归。因为有可能会有嵌套，所以，遇到"["符号的时候递归子元素，然后内部如果还有“[”继续递归，直到得到结果后一个个返回。

用栈也可以实现这个方法，但要弄清楚栈当中要存放些什么。因为我们肯定是在遇到“[”符号的时候入栈，所以入栈的时候入的是前面的数字和当前字符串res。当遇到"]"的时候出栈，出的时候，栈顶元素中的数字就是当前字符串的重复次数，然后再在前面拼接上栈顶的字符串。



题解：

**递归：**

```js
/**
 * @param {string} s
 * @return {string}
 */
var decodeString = function(s) {
    s=s.split("");
    function dfs(s,i){
        var i;
        var tmp;
        var res="";
        var multi=0;
        while(i<s.length){
            if("0"<=s[i]&&s[i]<="9"){
                multi=multi*10+Number(s[i]);  //考虑数字大于10的情况
            }
            else if(s[i]=="["){
                [i,tmp]=dfs(s,i+1);
                while(multi--){
                    res+=tmp;
                }
                multi=0;
            }
            else if(s[i]=="]"){
                return [i,res];

            }
            else{
                res+=s[i];
            }
            i++;
        }
        return res;
    }
    return dfs(s,0);
};
```



**栈：**

```js
/**
 * @param {string} s
 * @return {string}
 */
var decodeString = function(s) {
    s=s.split("");
    var sta=[];
    var res="";
    var num=0;
    var last_num;
    var last_res;

    for(let i=0;i<s.length;++i){    
        if(s[i]==="["){
            sta.push([num,res]);
            res="";
            num=0;
        }
        else if(s[i]==="]"){
            var len=sta.length;
            [last_num,last_res]=sta[len-1].slice();     
            sta.pop();
            var temp=res;
            for(let j=0;j<last_num-1;++j){
                res+=temp;
            }
            res=last_res+res;
        }
        else if(s[i]>="0"&&s[i]<="9"){
            num=num*10+Number(s[i]);
        }
        else{
            res+=s[i];
        }
    }
    return res;
};
```



<img src="./img/1.jpg" alt="img" style="zoom:50%;" />



这里要注意的是！不要将两个for循环中的变量设为同一个！比如都是`var i`，因为for不是函数作用域，用var去定义是会相互影响的，最好使用两个不同的变量，同时使用let去声明。





## [接雨水](https://leetcode-cn.com/problems/trapping-rain-water/)

给定 *n* 个非负整数表示每个宽度为 1 的柱子的高度图，计算按此排列的柱子，下雨之后能接多少雨水。

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/10/22/rainwatertrap.png)

上面是由数组 [0,1,0,2,1,0,1,3,2,1,2,1] 表示的高度图，在这种情况下，可以接 6 个单位的雨水（蓝色部分表示雨水）。 感谢 Marcos 贡献此图。

**示例:**

```
输入: [0,1,0,2,1,0,1,3,2,1,2,1]
输出: 6
```



思路：

这题的思路就是：先找到最高点，分别从两端往最高点遍历，如果下一个元素的值比当前元素小就说明可以接到水。

怎么接雨水：

![img](./img/2.PNG)







题解：

```js
/**
 * @param {number[]} height
 * @return {number}
 */
var trap = function(height) {
    //先找出最高点，然后分别从两端开始向最高点遍历
    //当后一个值比当前值要小就说明可以接到水
    var max=0;
    for(var i=0;i<height.length;++i){
        if(height[i]>height[max]){
            max=i;
        }
    }
    var area=0;
    var tmp=height[0];
    for(var i=0;i<max;++i){
        if(height[i]>tmp){
            tmp=height[i];
        }
        else{
            area+=tmp-height[i];
        }
    }
    var len=height.length;
    tmp=height[len-1];
    for(var i=len-1;i>max;i--){
        if(height[i]>tmp){
            tmp=height[i];
        }
        else{
            area+=tmp-height[i];
        }
    }
    return area;
};
```





## [零钱兑换](https://leetcode-cn.com/problems/coin-change/)

给定不同面额的硬币 coins 和一个总金额 amount。编写一个函数来计算可以凑成总金额所需的最少的硬币个数。如果没有任何一种硬币组合能组成总金额，返回 -1。

```
示例 1:

输入: coins = [1, 2, 5], amount = 11
输出: 3 
解释: 11 = 5 + 5 + 1
示例 2:

输入: coins = [2], amount = 3
输出: -1
说明:
你可以认为每种硬币的数量是无限的。
```



思路：
背包问题。我们要凑成总金额amount的话，需要在总金额为amount-`dj`的一堆硬币中加入一个币值为`dj`的硬币。因此我们需要考虑所有的`dj`满足F(n-`dj`)+1最小就可以了。加1代表的是当前的`dj`，F(n-`dj`)就是满足金额`n-dj`最少的硬币数。

因为每种金额所用的硬币数不会超过amount，比如amount为11，那硬币数最多也是11个（11个一元硬币）。那我们就可以将所有的amount的初始硬币数设为amount+1（除了0），那后面如果有更小的硬币数搭配的话，就替换。所以，如果最后F[amount]不是amount+1的话，就说明有更少的硬币组合搭配，如果是amount+1的话，就说明没有，返回-1。这样就不用多设一个变量去监听，每种amount是否有可搭配方案了。



题解：

```js
/**
 * @param {number[]} coins
 * @param {number} amount
 * @return {number}
 */

function min(a,b){
    if(a<=b){
        return a;
    }
    else{
        return b;
    }
}

var coinChange = function(coins, amount) {
    if(amount==0){
        return 0;
    }
    coins.sort(function(a,b){
        return a-b;
    })
    var F=[];
    for(let k=0;k<=amount;++k){
        F[k]=amount+1;
    }
    F[0]=0;
    for(let i=1;i<=amount;++i){
        for(let j=0;j<coins.length;++j){
            if(coins[j]>i){
                break;
            }
            else{
                F[i]=min(F[i-coins[j]]+1,F[i]);
            }
        }
    }
    return F[amount]==amount+1?-1:F[amount];
};
```





## [子集](https://leetcode-cn.com/problems/subsets/)

给定一组不含重复元素的整数数组 nums，返回该数组所有可能的子集（幂集）。

说明：解集不能包含重复的子集。

示例:

```
输入: nums = [1,2,3]
输出:
[
  [3],
  [1],
  [2],
  [1,2,3],
  [1,3],
  [2,3],
  [1,2],
  []
]
```



思路：

构造子集的思路，就是将当前的元素添加到之前的子集当中，从而生成新的子集。比如初始阶段子集应为：[[]]，只有一个空集合（空集是任何集合的子集）。当遍历到元素1的时候，将其添加到之前已有的集合中，构成新的子集：[[],[1]]。



题解：

 ```js
/**
 * @param {number[]} nums
 * @return {number[][]}
 */
var subsets = function(nums) {
    var res=[];
    res.push([]);
    for(var i=0;i<nums.length;++i){
        var len=res.length;
        for(var j=0;j<len;++j){
            var temp=res[j].slice();
            temp.push(nums[i]);
            res.push(temp);
        }
    }
    return res;
};
 ```





## [从前序与中序遍历序列构造二叉树](https://leetcode-cn.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)

根据一棵树的前序遍历与中序遍历构造二叉树。

注意:
你可以假设树中没有重复的元素。

例如，给出

```
前序遍历 preorder = [3,9,20,15,7]
中序遍历 inorder = [9,3,15,20,7]
```


返回如下的二叉树：

    	3
       / \
      9  20
        /  \
       15   7


思路：

递归的思想。通过前序遍历我们知道当前数组的根节点是什么，通过中序遍历我们可以知道哪些节点是当前结点的左子树上的结点，哪些节点是当前结点右子树上的结点。



题解：

```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
        int len=preorder.size();
        if(len==0){
            return NULL;
        }
        vector<int> pre_left,pre_right,in_left,in_right;
        int root=preorder[0];
        TreeNode* res=new TreeNode(root);  //创建一个新的TreeNode结点必须要加上new关键字
        int p=0;
        while(inorder[p]!=root){
            p++; //找到根结点在中序遍历当中的位置
        }
        for(int i=0;i<p;++i){
            //获取左边数组前序遍历和中序遍历的结果
            in_left.push_back(inorder[i]);
            pre_left.push_back(preorder[i+1]);
        }
        for(int j=p+1;j<len;++j){
            //获取右边数组前序遍历和中序遍历的结果
            in_right.push_back(inorder[j]);
            pre_right.push_back(preorder[j]);
        }
        res->left=buildTree(pre_left,in_left);
        res->right=buildTree(pre_right,in_right);
        return res;
    }
};
```





## [用栈实现队列](https://leetcode-cn.com/problems/implement-queue-using-stacks/)

使用栈实现队列的下列操作：

push(x) -- 将一个元素放入队列的尾部。
pop() -- 从队列首部移除元素。
peek() -- 返回队列首部的元素。
empty() -- 返回队列是否为空。
示例:

```
MyQueue queue = new MyQueue();

queue.push(1);
queue.push(2);  
queue.peek();  // 返回 1
queue.pop();   // 返回 1
queue.empty(); // 返回 false
```



说明:

你只能使用标准的栈操作 -- 也就是只有 push to top, peek/pop from top, size, 和 is empty 操作是合法的。
你所使用的语言也许不支持栈。你可以使用 list 或者 deque（双端队列）来模拟一个栈，只要是标准的栈操作即可。
假设所有操作都是有效的 （例如，一个空的队列不会调用 pop 或者 peek 操作）。



思路：

push的话是正常的push进stack1，那pop的时候，如果stack2为空的话，要先将stack1的每个数push到stack再做pop操作。



题解：

```c++
class MyQueue {
private:
    stack<int> s1;
    stack<int> s2;

public:
    /** Initialize your data structure here. */
    MyQueue() {
    }
    
    /** Push element x to the back of queue. */
    void push(int x) {
        s1.push(x);
    }
    
    /** Removes the element from in front of queue and returns that element. */
    int pop() {
        if(s2.empty()){
            while(!s1.empty()){
                s2.push(s1.top());
                s1.pop();
            }
        }
        int res=s2.top();
        s2.pop();
        return res;
    }
    
    /** Get the front element. */
    int peek() {
        if(s2.empty()){
            while(!s1.empty()){
                s2.push(s1.top());
                s1.pop();
            }
        }
        int top_val=s2.top();
        return top_val;
    }
    
    /** Returns whether the queue is empty. */
    bool empty() {
        if(s1.size()==0&&s2.size()==0){
            return true;
        }
        return false;
    }
};

/**
 * Your MyQueue object will be instantiated and called as such:
 * MyQueue* obj = new MyQueue();
 * obj->push(x);
 * int param_2 = obj->pop();
 * int param_3 = obj->peek();
 * bool param_4 = obj->empty();
 */
```























