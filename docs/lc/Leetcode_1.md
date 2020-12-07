**【原题链接】**

[1. 两数之和](https://leetcode-cn.com/problems/two-sum/)

**【题目描述】**

给定一个整数数组 `nums` 和一个目标值 `target`，请你在该数组中找出和为目标值的那 **两个** 整数，并返回他们的数组下标。

你可以假设每种输入只会对应一个答案。但是，数组中同一个元素不能使用两遍。

**示例:**

```text
给定 nums = [2, 7, 11, 15], target = 9
因为 nums[0] + nums[1] = 2 + 7 = 9
所以返回 [0, 1]
```

**【代码】**

```cpp
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        unordered_map<int,int> heap;
        for(int i = 0; i < nums.size(); i++){
            int r = target - nums[i];
            if(heap.count(r)) //等同 heap.find(r) != heap.end()
                return {heap[r], i};
            heap[nums[i]] = i;
        }
        return {};
    }
};
```

> 执行用时: **212 ms**

> 内存消耗: **52 MB**

**【时间复杂度】**

O(n) 

**【注意点】**

1. 其他解法：先排序再查找，需要O(nlogn)，直接暴搜O(n^2)

1. unordered_map查询元素是否存在的时间是O(1)的，而普通map是O(logn)的；

1. 函数末尾必须要有返回值

1. .count()

