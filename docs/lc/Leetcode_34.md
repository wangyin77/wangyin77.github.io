**【原题链接】**

[34. 在排序数组中查找元素的第一个和最后一个位置](https://leetcode-cn.com/problems/find-first-and-last-position-of-element-in-sorted-array/)

**【题目描述】**

给定一个按照升序排列的整数数组 `nums`，和一个目标值 `target`。找出给定目标值在数组中的开始位置和结束位置。

你的算法时间复杂度必须是 *O*(log *n*) 级别。

如果数组中不存在目标值，返回 `[-1, -1]`。

**示例 1:**

```text
输入: nums = [5,7,7,8,8,10], target = 8
输出: [3,4]
```

**示例 2:**

```text
输入: nums = [5,7,7,8,8,10], target = 6
输出: [-1,-1]
```

**【代码】**

```cpp
class Solution {
public:
    vector<int> searchRange(vector<int>& nums, int target) {
        if(nums.empty()) return {-1, -1};
        int L, R;
        //第一次二分：找到target值的左端点
        //目标点及其右侧点的性质： >= target
        int l = 0, r = nums.size() - 1;
        while(l < r){
            int mid = l + r >> 1;
            if(nums[mid] >= target) r = mid;
            else l = mid + 1;
        }
        if(nums[r] == target) L = r;
        else return {-1, -1};
        //第一次二分：找到target值的右端点
        //目标点及其左侧点的性质： <= target
        l = 0, r = nums.size() - 1;
        while(l < r){
            int mid = l + r + 1 >> 1;
            if(nums[mid] <= target) l = mid;
            else r = mid - 1;
        }
        R = r;
        return {L, R};
    }
};
```

> 

**【时间复杂度】**

O(logn) 

**【注意点】**

1. 其他解法：无

1. 二分模板题

