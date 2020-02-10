# 前端常考代码题

## 排序

### 冒泡排序

关键点：

- 每轮循环依次比较相邻元素。如果前面的元素比后面元素大，则交换两个元素的位置。每轮循环将一个最大元素压到数组底部

```c++
void bubbleSort(int a[], int len) {
	for (int i = 0; i<len; ++i) {
		for (int j = 0; j<len - i - 1; ++j) {
			if (a[j]>a[j + 1]) {
				swap(a[j], a[j + 1]);
			}
		}
	}
}

int main() {
	int num;
	cin >> num;
	int *a = new int[num];
	for (int i = 0; i<num; ++i) {
		cin >> a[i];
	}
	bubbleSort(a, num);
	for (int j = 0; j<num; ++j) {
		cout << a[j];
	}
	system("pause");
	return 0;
}
```

<br>



### 快速排序

关键点：

- 选出一个基准值（pivot）把数组分为两个部分，前面部分均比基准值小，后面部分均比基准值大。然后再分别对前面部分和后面部分进行划分。

```c++
int partition(int a[],int left,int right){
    int p=a[left];
    int i=left+1;
    int j=right;
    while(i<=j){
        //i<right和j>left也必须要有
        while(a[i]<p&&i<right){
            i+=1;
        }
        while(a[j]>p&&j>left){
            j-=1;
        }
        if(i<j){
            swap(a[i++],a[j--]);
        }
        //else这一步必须要有，不然i=j又会进入下一轮循环，无法正确排序
        else{
            i++;
        }
    }
    swap(a[left],a[j]);
    return j;
}

void quickSort(int a[],int left,int right){
    if(left>right){
        return;
    }
    int s=partition(a,left,right);
    quickSort(a,left,s-1);
    quickSort(a,s+1,right);
}

int main(){
    int a[8]={3,5,8,1,10,9,2,4};
    quickSort(a,0,7);
    for(int i=0;i<8;++i){
        cout<<a[i]<<" ";
    }
    return 0;
}
```



## 二叉树

### 二叉树的深度

```c++
#include<iostream>
using namespace std;

/*
struct TreeNode {
	int val;
	struct TreeNode *left;
	struct TreeNode *right;
	TreeNode(int x) :
			val(x), left(NULL), right(NULL) {
	}
};
*/

class Solution {
public:
    int TreeDepth(TreeNode* pRoot)
    {
        if(pRoot==nullptr)
            return 0;
        int left=TreeDepth(pRoot->left);
        int right=TreeDepth(pRoot->right);
        return left>right?left+1:right+1;
    }
};
```





### 二叉树镜像

关键点：

- 递归，结束条件

```c++
#include<iostream>
using namespace std;
/*
struct TreeNode {
	int val;
	struct TreeNode *left;
	struct TreeNode *right;
	TreeNode(int x) :
			val(x), left(NULL), right(NULL) {
	}
};
*/

class Solution {
public:
     void Mirror(TreeNode *pRoot) {
         if(!pRoot){
             return;
         }
         TreeNode* temp=pRoot->left;
         pRoot->left=pRoot->right;
         pRoot->right=temp;
         if(pRoot->left){
             Mirror(pRoot->left);
         }
         if(pRoot->right){
             Mirror(pRoot->right);
         }
     }
};
```





### 判断二叉树是否对称

关键点：

- 递归，对称的概念

```c++
#include<iostream>
using namespace std;

/*
struct TreeNode {
    int val;
    struct TreeNode *left;
    struct TreeNode *right;
    TreeNode(int x) :
            val(x), left(NULL), right(NULL) {
    }
};
*/

class Solution {
public:
bool isSymmetrical(TreeNode* pRoot)
    {
        return isSymmetrical(pRoot,pRoot);
    }

bool isSymmetrical(TreeNode* node1,TreeNode* node2){
    if(node1==NULL&&node2==NULL){
        return true;
    }
    else if(node1==NULL||node2==NULL){
        return false;
    }
    else if(node1->val!=node2->val){
        return false;
    }
    return isSymmetrical(node1->left,node2->right)&&isSymmetrical(node1->right,node2->left);
}
};

```





### 二叉树中和为某一值的路径

关键点：

- 深度优先遍历

```c++
#include<iostream>
#include<vector>
using namespace std;
/*
struct TreeNode {
	int val;
	struct TreeNode *left;
	struct TreeNode *right;
	TreeNode(int x) :
			val(x), left(NULL), right(NULL) {
	}
};
*/

class Solution {
public:
    vector<vector<int> > FindPath(TreeNode* root,int expectNumber) {
        vector<vector<int>> res;
        vector<int> path;
        if(root==NULL){
            return res;
        }
        DFS(root,expectNumber,res,path);
        return res;
    }

    //记住深度优先遍历
    void DFS(TreeNode* root, int sum, vector<vector<int>>& res, vector<int>& path){
        path.push_back(root->val);
        if(!root->left&&!root->right){
            if(root->val==sum){
                res.push_back(path);
            }
        }
        if(root->left){
            DFS(root->left,sum-root->val,res,path);
        }
        if(root->right){
            DFS(root->right,sum-root->val,res,path);
        }
        path.pop_back();
    }
};
```





### 二叉树层次遍历

关键点：

- 使用queue

```c++
#include<iostream>
#include<vector>
#include<queue>
using namespace std;
/*
struct TreeNode {
	int val;
	struct TreeNode *left;
	struct TreeNode *right;
	TreeNode(int x) :
			val(x), left(NULL), right(NULL) {
	}
};
*/

class Solution {
public:
    vector<int> PrintFromTopToBottom(TreeNode* root) {
        vector<int> res;
        queue<TreeNode*> q;
        if(root==NULL){
            return res;
        }
        q.push(root);
        while(!q.empty()){
            TreeNode* cur=q.front();
            q.pop();
            res.push_back(cur->val);
            if(cur->left){
                q.push(cur->left);
            }
            if(cur->right){
                q.push(cur->right);
            }
        }
        return res;
    }
};
```





### 二叉树前序遍历

关键点：

- 要先将root压入stack中
- 循环当中，先压root->right，再压root->left。这样pop的时候就是先左后右的顺序

```c++
#include <iostream>
#include <stack>
#include <vector>
using namespace std;

/*
//Definition for a binary tree node.
struct TreeNode {
     int val;
     TreeNode *left;
     TreeNode *right;
     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 };
*/

class Solution {
public:
    vector<int> preorderTraversal(TreeNode* root) {
        stack<TreeNode*> temp;
        vector<int> res;
        if(root==NULL){
            return res;
        }
        temp.push(root);
        while(!temp.empty()){
            TreeNode* cur=temp.top();
            temp.pop();
            res.push_back(cur->val);
            if(cur->right){
                temp.push(cur->right);
            }
            if(cur->left){
                temp.push(cur->left);
            }
        }
        return res;
    }
};
```



### 二叉树中序遍历

关键点：

- 不需要先将root压入stack中
- 两层while

```c++
#include <iostream>
#include <stack>
#include <vector>
using namespace std;

/*
//Definition for a binary tree node.
struct TreeNode {
     int val;
     TreeNode *left;
     TreeNode *right;
     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 };
*/

class Solution {
public:
    vector<int> preorderTraversal(TreeNode* root) {
        stack<TreeNode*> temp;
        vector<int> res;
        //注意此处是或||运算
        while(root!=NULL||!temp.empty()){
            while(root!=NULL){
                temp.push(root);
                root=root->left;
            }
            root=temp.top();
            temp.pop();
            res.push_back(root->val);
            root=root->right;
        }
        return res;
    }
};
```





### 二叉树后序遍历

关键点：

- 要使用2个stack！

- 要先将root压入stack1

```c++
#include <iostream>
#include <stack>
#include <vector>
using namespace std;
/*
//Definition for a binary tree node.
struct TreeNode {
     int val;
     TreeNode *left;
     TreeNode *right;
     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 };
*/

class Solution {
public:
    vector<int> postorderTraversal(TreeNode* root) {
        vector<int> res;
        stack<TreeNode*> s1;
        stack<TreeNode*> s2;
        if(root==NULL){
            return res;
        }
        s1.push(root);
        while(!s1.empty()){
            TreeNode* cur=s1.top();
            s1.pop();
            s2.push(cur);
            if(cur->left){
                s1.push(cur->left);
            }
            if(cur->right){
                s1.push(cur->right);
            }
        }
        while(!s2.empty()){
            TreeNode* s=s2.top();
            s2.pop();
            res.push_back(s->val);
        }
        return res;
    }
};
```





## 链表

### 翻转链表

关键点：

- 三个指针
- 以p2作为while循环条件，最后将p1的值赋值给pHead

```c++
#include<iostream>
using namespace std;
/*
struct ListNode {
	int val;
	struct ListNode *next;
	ListNode(int x) :
			val(x), next(NULL) {
	}
};
*/

class Solution {
public:
    ListNode* ReverseList(ListNode* pHead) {
        if(pHead==NULL||pHead->next==NULL){
            return pHead;
        }
        ListNode* p1=pHead;
        ListNode* p2=pHead->next;
        ListNode* p3=p2->next;
        while(p2){
            p2->next=p1;
            p1=p2;
            p2=p3;
            if(p3){
                p3=p3->next;
            }
        }
        pHead->next=NULL;
        pHead=p1;
        return pHead;
    }
};
```





### 两数相加

题目描述：

给出两个 非空 的链表用来表示两个非负的整数。其中，它们各自的位数是按照 逆序 的方式存储的，并且它们的每个节点只能存储 一位 数字。

如果，我们将这两个数相加起来，则会返回一个新的链表来表示它们的和。

您可以假设除了数字 0 之外，这两个数都不会以 0 开头。

```
输入：(2 -> 4 -> 3) + (5 -> 6 -> 4)
输出：7 -> 0 -> 8
原因：342 + 465 = 807
```



关键点：

- 需要创建一个新的链表
- 需要记录进位

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
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        ListNode* head=new ListNode(0);
        ListNode* p1=l1;
        ListNode* p2=l2; 
        ListNode* cur=head;
        int carry=0;
        while(p1!=NULL||p2!=NULL){
            int x=(p1!=NULL)?p1->val:0;
            int y=(p2!=NULL)?p2->val:0;
            int sum=x+y+carry;
            cur->next=new ListNode(sum%10);
            carry=sum/10;
            cur=cur->next;
            if(p1!=NULL){
                p1=p1->next;
            }
            if(p2!=NULL){
                p2=p2->next;
            }
        }
        if(carry>0){
            cur->next=new ListNode(carry);
        }
        return head->next;
    }
};
```





## 大数相加

关键点：

- 使用JavaScript
- `~~`：相当于`Math.floor()`，具体不同见博客<https://delaprada.com/>

```js
const a = '123456789';
const b = '11111111111111111111111111';
function bigNumAdd(a,b){
    var temp = 0;
    var res = ""
    a = a.split("");
    b = b.split("");
    while (a.length || b.length || temp) {
        temp += ~~(a.pop()) + ~~(b.pop());
        res = (temp % 10) + res;
        temp = temp > 9  //若想更好理解可修改为 temp=temp>9?1:0
    }
    return res
}

console.log(bigNumAdd(a,b));
```





## 二分查找

```c++
#include<iostream>
#include<vector>
using namespace std;
/*
对于一个有序数组，我们通常采用二分查找的方式来定位某一元素，请编写二分查找的算法，在数组中查找指定元素。
给定一个整数数组A及它的大小n，同时给定要查找的元素val，请返回它在数组中的位置(从0开始)，若不存在该元素，返回-1。若该元素出现多次，请返回第一次出现的位置。
*/
class BinarySearch {
public:
    int getPos(vector<int> A, int n, int val) {
        // write code here
        int left=0;
        int right=n-1;
        int mid=0;
        while(left<=right){
            int mid=(left+right)/2;
            //是第一次出现的话就返回mid
            if(A[mid]==val&&(mid==0||A[mid-1]<A[mid])){
                return mid;
            }
            //不是第一次出现或者所要找的值比当前值小
            else if((A[mid]==val&&A[mid-1]==A[mid])||val<A[mid]){
                right=mid-1;
            }
            //所要找的值比当前值大
            else{
                left=mid+1;
            }
        }
        return -1;
    }
};
```



