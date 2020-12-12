**【原题链接】**

[40. 组合总和 II](https://leetcode-cn.com/problems/combination-sum-ii/)

**【题目描述】**

给定一个数组 `candidates` 和一个目标数 `target` ，找出 `candidates` 中所有可以使数字和为 `target` 的组合。

`candidates` 中的每个数字在每个组合中只能使用一次。

**说明：**

- 所有数字（包括目标数）都是正整数。
- 解集不能包含重复的组合。 

**示例 1:**

```
输入: candidates = [10,1,2,7,6,1,5], target = 8,
所求解集为:
[
  [1, 7],
  [1, 2, 5],
  [2, 6],
  [1, 1, 6]
]
```

**示例 2:**

```
输入: candidates = [2,5,2,1,2], target = 5,
所求解集为:
[
  [1,2,2],
  [5]
]
```

**【代码】**

```cpp
class Solution {
public:
    vector<vector<int>> ans;
    vector<int> path;
    vector<vector<int>> combinationSum2(vector<int>& c, int target) {
        sort(c.begin(), c.end());
        dfs(c, 0, target);
        return ans;
    }
    void dfs(vector<int>& c, int u, int target){
        if(target == 0){
            ans.push_back(path);
            return;
        }
        if(u == c.size())   return;
        int k = u + 1, i = 0;
        while(k < c.size() && c[k] == c[u]) k++;
        while(c[u] * i <= target && i <= k - u){
            dfs(c, k, target - c[u] * i);
            path.push_back(c[u]);
            i++;
        }
        while(i--)  path.pop_back();
        return;
    }
};
```

> 执行用时: **12 ms**
>
> 内存消耗: **11.3 MB**

**【时间复杂度】**

O(k^n)  

**【注意点】**

1. 其他解法：无
1. 和上一题相同，按每个数u使用i次dfs， i的次数有限制
