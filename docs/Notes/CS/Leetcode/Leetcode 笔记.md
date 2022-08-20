# Leetcode 笔记



## 数组

### 217.存在重复元素

```c++
给你一个整数数组 nums 。如果任一值在数组中出现 至少两次 ，返回 true ；如果数组中每个元素互不相同，返回 false 。


示例 1：

输入：nums = [1,2,3,1]
输出：true
示例 2：

输入：nums = [1,2,3,4]
输出：false
示例 3：

输入：nums = [1,1,1,3,3,4,3,2,4,2]
输出：true


提示：

1 <= nums.length <= 105
-109 <= nums[i] <= 109
```

#### 笔记

- 解题思路1:先排序,再遍历检查相邻两个元素是否相等,有相等则返回true

  ```c++
  class Solution {
  public:
      bool containsDuplicate(vector<int>& nums) {
          sort(nums.begin(), nums.end());
          int n = nums.size();
          for (int i = 0; i < n - 1; i++) {
              if (nums[i] == nums[i + 1]) {
                  return true;
              }
          }
          return false;
      }
  };
  ```

  -  `sort(nums.begin(), nums.end());`	vector数组使用算法排序



- 解题思路2:使用哈希表,依次插入数组元素,若发现要插入的元素已经存在,则返回true

  ```c++
  class Solution {
  public:
      bool containsDuplicate(vector<int>& nums) {
          unordered_set<int> s;
          for (int x: nums) {
              if (s.find(x) != s.end()) {
                  return true;
              }
              s.insert(x);
          }
          return false;
      }
  };
  ```

  - `unordered_set`无序关联式容器,底层是哈希表,不会自动排序,不能有重复元素,直接存储数据的值(可以理解为键和值相等)
  - 容器具体:http://c.biancheng.net/view/7250.html
  - `for (int x: nums)`   含义:使用迭代器遍历nums中所有元素,存储到x中
  - 为什么使用哈希表?
    - 哈希表相较于vector数组,**查找速度更快**,实测将unordered_set换成vector后耗时大大增加



- 错误思路:逐个比较两两元素是否存在相同,存在则返回true
  - 错误原因:**时间复杂度太高**







### 66.加一

```c++
给定一个由 整数 组成的 非空 数组所表示的非负整数，在该数的基础上加一。

最高位数字存放在数组的首位， 数组中每个元素只存储单个数字。

你可以假设除了整数 0 之外，这个整数不会以零开头。

 

示例 1：

输入：digits = [1,2,3]
输出：[1,2,4]
解释：输入数组表示数字 123。
示例 2：

输入：digits = [4,3,2,1]
输出：[4,3,2,2]
解释：输入数组表示数字 4321。
示例 3：

输入：digits = [0]
输出：[1]
 

提示：

1 <= digits.length <= 100
0 <= digits[i] <= 9

```



#### 笔记

- 解题思路:因为加一问题涉及到进位,因此将问题简化为:**逆序寻找第一个不为9的数**,会有以下三种情况:
  - 没有9,直接加一即可,如123
  - 末尾有一个或多个9,但有数字不是9,则**对第一个不为9的数字加一,后面的所有9都置为0**
  - 所有数字都是9,需要**创建一个新的长度加一的数组,并直接将第一个数置为1,其他为0**

```c++
class Solution {
public:
    vector<int> plusOne(vector<int>& digits) {
        for(int i=digits.size()-1;i>=0;i--)
        {
            if(digits[i]!=9)//找到第一个不为9的数
            {
                digits[i]++;
                for(int j=i+1;j<digits.size();j++)
                {
                    digits[j]=0;
                }
                return digits;
            }
        }
        //第三种情况
        vector<int> d(digits.size()+1);
        d[0]=1;
        return d;
    }
};
```

- `vector<int> d(num);`可以直接构造一个长度为num的全0数组

个人代码:

```c++
class Solution {
public:
    vector<int> plusOne(vector<int>& digits) {
        digits[digits.size()-1]++;
        vector<int> d(digits.size()+1);
        for(int i=digits.size()-1;i>=0;i--)
        {
            if(digits[i]==10)
            {
                if(i==0)
                {
                    d[0]=1;
                    break;
                }
                digits[i]=0;
                digits[i-1]++;
            }
            else
            return digits;
        }
        return d;
    }
};
```

- 正常的进位过程思路
- 卡壳原因:没有考虑到全部进位可以直接将数组第一个数置为1其他为0,导致考虑复制数组并右移一位的复杂解法





### 88. 合并两个有序数组

```c++
给你两个按 非递减顺序 排列的整数数组 nums1 和 nums2，另有两个整数 m 和 n ，分别表示 nums1 和 nums2 中的元素数目。

请你 合并 nums2 到 nums1 中，使合并后的数组同样按 非递减顺序 排列。

注意：最终，合并后数组不应由函数返回，而是存储在数组 nums1 中。为了应对这种情况，nums1 的初始长度为 m + n，其中前 m 个元素表示应合并的元素，后 n 个元素为 0 ，应忽略。nums2 的长度为 n 。

 

示例 1：

输入：nums1 = [1,2,3,0,0,0], m = 3, nums2 = [2,5,6], n = 3
输出：[1,2,2,3,5,6]
解释：需要合并 [1,2,3] 和 [2,5,6] 。
合并结果是 [1,2,2,3,5,6] ，其中斜体加粗标注的为 nums1 中的元素。
示例 2：

输入：nums1 = [1], m = 1, nums2 = [], n = 0
输出：[1]
解释：需要合并 [1] 和 [] 。
合并结果是 [1] 。
示例 3：

输入：nums1 = [0], m = 0, nums2 = [1], n = 1
输出：[1]
解释：需要合并的数组是 [] 和 [1] 。
合并结果是 [1] 。
注意，因为 m = 0 ，所以 nums1 中没有元素。nums1 中仅存的 0 仅仅是为了确保合并结果可以顺利存放到 nums1 中。
 

提示：

nums1.length == m + n
nums2.length == n
0 <= m, n <= 200
1 <= m + n <= 200
-109 <= nums1[i], nums2[j] <= 109
 

进阶：你可以设计实现一个时间复杂度为 O(m + n) 的算法解决此问题吗？

```



#### 笔记

- 解法一(个人解法):直接合并数组后排序

```c++
class Solution {
public:
    void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {
        for (int i = 0; i != n; ++i) {
            nums1[m + i] = nums2[i];
        }
        sort(nums1.begin(), nums1.end());
    }
};
```

- 时间复杂度O((m+n)log(m+n)),没有到O(m+n)要求,可以继续优化





解法二:归并排序,双指针

- 对两个数组采用归并排序,并存储到临时数组中

```c++
class Solution {
public:
    void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {
        int res[m+n];
        int p1=0,p2=0;
        for(int i=0;i<m+n;i++)
        {
            if(p1==m)
                res[i]=nums2[p2++];
            else if(p2==n)
                res[i]=nums1[p1++];
            else if(nums1[p1]<=nums2[p2])
                res[i]=nums1[p1++];
            else
                res[i]=nums2[p2++];
        }
        for(int i=0;i<m+n;i++)
            nums1[i]=res[i];
    }
};
```

- 注意:归并排序时,**要先判断两个归并段还有没有需要归并排序的数据,防止比较大小时与归并段范围外的数字进行比较**
- 每次归并都需要递增指针
- 需要用到临时数组作为结果,空间复杂度还可以再优化





解法三:逆向归并排序

- 从后向前由大到小进行归并排序,将归并好的数据存储在num1后预留的空间中

  ```c++
  class Solution {
  public:
      void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {
          int p1 = m - 1, p2 = n - 1;
          int tail = m + n - 1;
          int cur;
          while (p1 >= 0 || p2 >= 0) {
              if (p1 == -1) {
                  cur = nums2[p2--];
              } else if (p2 == -1) {
                  cur = nums1[p1--];
              } else if (nums1[p1] > nums2[p2]) {
                  cur = nums1[p1--];
              } else {
                  cur = nums2[p2--];
              }
              nums1[tail--] = cur;
          }
      }
  };
  ```

  - 可证明,逆向归并排序时不会覆盖掉num1中p1前的有效数据
  - 节省了空间,优化了空间复杂度







## 二叉树



### 94. 二叉树的中序遍历

(https://leetcode.cn/problems/binary-tree-inorder-traversal/)

```c++
给定一个二叉树的根节点 root ，返回 它的 中序 遍历 。

 

示例 1：


输入：root = [1,null,2,3]
输出：[1,3,2]
示例 2：

输入：root = []
输出：[]
示例 3：

输入：root = [1]
输出：[1]
 

提示：

树中节点数目在范围 [0, 100] 内
-100 <= Node.val <= 100
 

进阶: 递归算法很简单，你可以通过迭代算法完成吗？

```



#### 笔记

解法一:递归算法

```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    void inorder(TreeNode* root,vector<int> &res)
    {
        if(root==nullptr)
        {
            return;
        }
        inorder(root->left,res);
        res.push_back(root->val);
        inorder(root->right,res);
    }

    vector<int> inorderTraversal(TreeNode* root) {
        vector<int> res;
        inorder(root,res);
        return res;
    }
};
```

- 由于需要存储到数组中,因此**额外写一个函数用于递归调用并通过引用将数据写到相同的数组中**
- **每次递归都要首先判断遍历的指针是否为空,为空则直接跳出**
- 时间复杂度O(n),每个结点被访问一次
- 空间复杂度O(n)



==**解法二:迭代法**==

- ==先中后序遍历的迭代法思路核心==:**将递归中隐式的函数栈显示的表现出来**

```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root) {
        vector<int> res;
        stack<TreeNode*> stk;
        TreeNode* p=root;
        while(p!=nullptr||!stk.empty())
        {
            while(p!=nullptr)
            {
                stk.push(p);
                p=p->left;
            }
            p=stk.top();
            res.push_back(p->val);
            stk.pop();
            p=p->right;
        }
        return res;
    }
};
```

- 根据迭代的思路核心可以得到:需要接用栈来记录需要之后访问的结点
- ==中序遍历迭代具体思路==:(**同理也需要类推理解先,后序的迭代思路**)
  - 每一次迭代调用**相当于**一次递归
  - 每次递归都先递归左子树到最左下角的结点,**相当于**迭代中先遍历到最左下角的结点,并将经过的结点放入栈中
  - 左侧递归到底后是访问当前节点,**相当于**从栈中取出top元素并访问
  - 访问后再对该节点的右子树递归,**相当于**将指针指向该节点的右结点并循环迭代过程

- 时间复杂度O(n)
- 空间复杂度O(n)





解法三:Morris中序遍历

- 算法思路:
  - 1.判断当前结点是否有左孩子,没有左孩子到2,有左孩子到3
  - 2.没有左孩子则记录当前结点数据(访问),并将指针移向右孩子
  - 3.有左孩子则先找到当前结点左子树最右下角的结点(即**当前结点的前驱**),再判断最右下角结点是否有右孩子,无右孩子到4,有右孩子到5
  - 4.没有右孩子说明该左子树还没有被访问过,将最右下角结点的右孩子设为当前结点,并将指针移向当前结点的左孩子
  - 5.有右孩子说明该左子树已经遍历完成,因此记录当前结点数据(访问),并将指针移向当前结点的右孩子
  - 6.回到1,直至全部遍历完成

```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root) {
        vector<int> res;
        TreeNode* p=root,*pre;
        while(p!=nullptr)
        {
            if(p->left==nullptr)//左孩子为空
            {
                res.push_back(p->val);
                p=p->right;
            }
            else//左孩子不为空
            {
                pre=p->left;
                while(pre->right!=nullptr&&pre->right!=p)
                    //找到左子树最右下角的结点存在pre中,注意第二个条件保证已经连接的后驱不会导致查找出现循环
                {
                    pre=pre->right;
                }
                if(pre->right==nullptr)//左子树最右下角的结点的右子树为空,左子树还没有被访问完
                {
                    pre->right=p;
                    p=p->left;//左子树没有被访问完,指针向左子树移动
                }
                else//左子树最右下角的结点的右子树不为空,左子树已经被访问完
                {
                    res.push_back(pre->right->val);
                    pre->right=nullptr;
                    p=p->right;//左子树已经被访问完,指针向右子树移动
                }
            }
        }
        return res;
    }
};
```

- 该算法省去了栈的空间开销
- 时间复杂度O(n),每个结点被访问两次O(2n)=O(n)
- 空间复杂度O(1)





### 100. 相同的树

(https://leetcode.cn/problems/same-tree/)

```c++
给你两棵二叉树的根节点 p 和 q ，编写一个函数来检验这两棵树是否相同。

如果两个树在结构上相同，并且节点具有相同的值，则认为它们是相同的。

 

示例 1：


输入：p = [1,2,3], q = [1,2,3]
输出：true
示例 2：


输入：p = [1,2], q = [1,null,2]
输出：false
示例 3：


输入：p = [1,2,1], q = [1,1,2]
输出：false
 

提示：

两棵树上的节点数目都在范围 [0, 100] 内
-104 <= Node.val <= 104
```





#### 笔记

解法一:深度优先算法

- 类似树的先序遍历

```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:   
    bool isSameTree(TreeNode* p, TreeNode* q) {
        if(p==nullptr&&q==nullptr)
        {
            return true;
        }
        else if(p==nullptr||q==nullptr)
        {
            return false;
        }
        else if(p->val!=q->val)
        {
            return false;
        }
        else
            return isSameTree(p->left,q->left)&&isSameTree(p->right,q->right);
    }
};
```

- 注意考虑两种特殊情况:
  - 如果两个树相同位置结点都是空指针,则这两个结点相同
  - 如果两个树相同位置结点只有一个是空指针,则两棵树一定不相同

- 个人错误原因:
  - 两种特殊情况和一般情况判断值是否相同在同一逻辑层级上,即可以直接判断两棵树是否不相同,不必分开
  - 理解递归调用的整体性和统一性,每一次递归都在做相同的事情(还需要实践不断理解)



解法二:广度优先算法

- 广度优先就是利用队列进行遍历,类似于层序遍历

```c++
class Solution {
public:
    bool isSameTree(TreeNode* p, TreeNode* q) {
        if (p == nullptr && q == nullptr) {
            return true;
        } else if (p == nullptr || q == nullptr) {
            return false;
        }
        queue <TreeNode*> queue1, queue2;
        queue1.push(p);
        queue2.push(q);
        while (!queue1.empty() && !queue2.empty()) {
            auto node1 = queue1.front();
            queue1.pop();
            auto node2 = queue2.front();
            queue2.pop();
            if (node1->val != node2->val) {
                return false;
            }
            auto left1 = node1->left, right1 = node1->right, left2 = node2->left, right2 = node2->right;
            if ((left1 == nullptr) ^ (left2 == nullptr)) {
                return false;
            }
            if ((right1 == nullptr) ^ (right2 == nullptr)) {
                return false;
            }
            if (left1 != nullptr) {
                queue1.push(left1);
            }
            if (right1 != nullptr) {
                queue1.push(right1);
```

- 1.仍然需要先判断两种特殊情况
- 2.首先判断根节点是否相同
- 3.接着判断根节点的两个子树位置是否相同,如有不同直接返回
- 4.如果根节点的两个子树位置相同,则按照先左后右的顺序将子节点放入队列
- 重复2~4