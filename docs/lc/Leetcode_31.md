**【原题链接】**

[31. 下一个排列](https://leetcode-cn.com/problems/next-permutation/)

**【题目描述】**

实现获取 **下一个排列** 的函数，算法需要将给定数字序列重新排列成字典序中下一个更大的排列。

如果不存在下一个更大的排列，则将数字重新排列成最小的排列（即升序排列）。

必须[** 原地 **](https://baike.baidu.com/item/%E5%8E%9F%E5%9C%B0%E7%AE%97%E6%B3%95)修改，只允许使用额外常数空间。

**示例 1：**

```text
输入：nums = [1,2,3]
输出：[1,3,2]
```

**示例 2：**

```text
输入：nums = [3,2,1]
输出：[1,2,3]
```

**示例 3：**

```text
输入：nums = [1,1,5]
输出：[1,5,1]
```

**示例 4：**

```text
输入：nums = [1]
输出：[1]
```

**提示：**

- `1 <= nums.length <= 100`

- `0 <= nums[i] <= 100`

**【代码】**

```cpp
class Solution {
public:
    void nextPermutation(vector<int>& nums) {
        int k = nums.size() - 1;
        while(k && nums[k] <= nums[k - 1]) --k;
        if(!k){
            reverse(nums.begin(), nums.end());
            return;
        } 
        int t = k;
        while(t < nums.size() && nums[t] > nums[k - 1]) ++t;
        swap(nums[k - 1], nums[t - 1]);
        reverse(nums.begin() + k, nums.end());
    }
};
```

> 执行用时: **12 ms**

> 内存消耗: **11.9 MB**

**【时间复杂度】**

O(n) 

**【注意点】**

1. 其他解法：无

1. **解题思路：从后往前找第一个非升序数，记作nums[k-1],之后在[k,end]区间内找到比nums[k-1]大的最小的数，swap这俩，再把[k,end]翻转。**

