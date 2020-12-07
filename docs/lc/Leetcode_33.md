**【原题链接】**

[33. 搜索旋转排序数组](https://leetcode-cn.com/problems/search-in-rotated-sorted-array/)

**【题目描述】**

给你一个整数数组 `nums` ，和一个整数 `target` 。该整数数组原本是按升序排列，但输入时在预先未知的某个点上进行了旋转。（例如，数组 `[0,1,2,4,5,6,7]` 可能变为 `[4,5,6,7,0,1,2]` ）。请你在数组中搜索 `target` ，如果数组中存在这个目标值，则返回它的索引，否则返回 `-1` 。 

**示例 1：**

```text
输入：nums = [4,5,6,7,0,1,2], target = 0
输出：4
```

**示例 2：**

```text
输入：nums = [4,5,6,7,0,1,2], target = 3
输出：-1
```

**示例 3：**

```text
输入：nums = [1], target = 0
输出：-1
```

**提示：**

- `1 <= nums.length <= 5000`

- `-10^4 <= nums[i] <= 10^4`

- `nums` 中的每个值都 **独一无二**

- `nums` 肯定会在某个点上旋转

- `-10^4 <= target <= 10^4`

**【代码】**

```cpp
class Solution {
public:
    int search(vector<int>& nums, int target) {
        if(nums.empty()) return -1;

        //第一次二分：查找分界点位置,最小的那个数
        //处于分界点及其右侧性质： < nums[0]
        int l = 0, r = nums.size() - 1;
        while(l < r){
            int mid = l + r >> 1;
            if (nums[mid] < nums[0]) r = mid;
            else l = mid + 1;
        }
        //第二次二分在半段上查找target
        //处于target及其右侧性质： >= target
        if(target >= nums[0]) l = 0;
        else l = r, r = nums.size() - 1;

        while(l < r){
            int mid = l + r >> 1;
            if(nums[mid] >= target) r = mid;
            else l = mid + 1;
        }
        return nums[r] == target ? r : -1;
    }
};
```

> 执行用时: **8 ms**

> 内存消耗: **11.3 MB**

**【时间复杂度】**

O(logn) 

**【注意点】**

1. 其他解法：无

1. 二分模板题

