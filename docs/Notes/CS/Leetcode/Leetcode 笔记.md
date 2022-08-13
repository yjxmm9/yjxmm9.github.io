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





