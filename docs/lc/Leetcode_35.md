**【原题链接】**

[35. 搜索插入位置](https://leetcode-cn.com/problems/search-insert-position/)

**【题目描述】**

给定一个排序数组和一个目标值，在数组中找到目标值，并返回其索引。如果目标值不存在于数组中，返回它将会被按顺序插入的位置。

你可以假设数组中无重复元素。

**示例 1:**

```text
输入: [1,3,5,6], 5
输出: 2
```

**示例 2:**

```text
输入: [1,3,5,6], 2
输出: 1
```

**示例 3:**

```text
输入: [1,3,5,6], 7
输出: 4
```

**示例 4:**

```text
输入: [1,3,5,6], 0
输出: 0
```

**【代码】**

```cpp
class Solution {
public:
    int searchInsert(vector<int>& nums, int target) {
        int l = 0, r = nums.size();
        //第一次二分，目标为第一个大于等于target的值
        //目标及其右侧的性质: >= target
        while(l < r){
            int mid = l + r >> 1; // 这里是下取整，所以不会越界
            if(nums[mid] >= target) r = mid;
            else l = mid + 1;
        }
        return r;
    }
};
```

> 执行用时: **12 ms**

> 内存消耗: **9.8 MB**

**【时间复杂度】**

O(logn) 

**【注意点】**

1. 其他解法：遍历O(n)

1. 二分模板题

