**【原题链接】**

[1712. 将数组分成三个子数组的方案数](https://leetcode-cn.com/problems/ways-to-split-array-into-three-subarrays/)

**【题目描述】**

我们称一个分割整数数组的方案是 **好的** ，当它满足：

- 数组被分成三个 **非空** 连续子数组，从左至右分别命名为 `left` ， `mid` ， `right` 。
- `left` 中元素和小于等于 `mid` 中元素和，`mid` 中元素和小于等于 `right` 中元素和。

给你一个 **非负** 整数数组 `nums` ，请你返回 **好的** 分割 `nums` 方案数目。由于答案可能会很大，请你将结果对 `109 + 7` 取余后返回。

**示例 1：**

```
输入：nums = [1,1,1]
输出：1
解释：唯一一种好的分割方案是将 nums 分成 [1] [1] [1] 。
```

**示例 2：**

```
输入：nums = [1,2,2,2,5,0]
输出：3
解释：nums 总共有 3 种好的分割方案：
[1] [2] [2,2,5,0]
[1] [2,2] [2,5,0]
[1,2] [2,2] [5,0]
```

**示例 3：**

```
输入：nums = [3,2,1]
输出：0
解释：没有好的分割方案。 
```

**提示：**

- `3 <= nums.length <= 105`
- `0 <= nums[i] <= 104`

**【代码】**

```cpp
class Solution {
public:
    int waysToSplit(vector<int>& nums) {
        int n = nums.size(), N = 1e9 + 7, res = 0;
        vector<int> s(n + 1);
        for(int i = 1; i <= n; i++) s[i] = s[i - 1] + nums[i - 1];
        for(int i = 1, l = 2, r = 2; i < n; i++){
            l = max(l, i + 1), r = max(r, i + 1);
            while(l < n && s[l] - s[i] < s[i]) l ++;
            while(r < n && s[n] - s[r] >= s[r] - s[i]) r ++;
            if(l <= r) res = (res + r - l) % N;
        }
        return res;
    }
};
```

> 执行用时: **332 ms**
>
> 内存消耗: **83.5 MB**

**【时间复杂度】**

O(n)  

**【注意点】**

1. 其他解法：二分查找

1. 采用三指针做法，重点在于条件判断

   ```cpp
   while(l < n && s[l] - s[i] < s[i]) l ++;
   while(r < n && s[n] - s[r] >= s[r] - s[i]) r ++;
   ```

   
