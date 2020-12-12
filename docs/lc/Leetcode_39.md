**【原题链接】**

[39. 组合总和](https://leetcode-cn.com/problems/combination-sum/)

**【题目描述】**

给定一个**无重复元素**的数组 `candidates` 和一个目标数 `target` ，找出 `candidates` 中所有可以使数字和为 `target` 的组合。

`candidates` 中的数字可以无限制重复被选取。

**说明：**

- 所有数字（包括 `target`）都是正整数。
- 解集不能包含重复的组合。 

**示例 1：**

```
输入：candidates = [2,3,6,7], target = 7,
所求解集为：
[
  [7],
  [2,2,3]
]
```

**示例 2：**

```
输入：candidates = [2,3,5], target = 8,
所求解集为：
[
  [2,2,2,2],
  [2,3,3],
  [3,5]
]
```

**提示：**

- `1 <= candidates.length <= 30`
- `1 <= candidates[i] <= 200`
- `candidate` 中的每个元素都是独一无二的。
- `1 <= target <= 500`

**【代码】**

```cpp
class Solution {
public:
    vector<vector<int>> ans;
    vector<int> path;
    vector<vector<int>> combinationSum(vector<int>& c, int target) {
        dfs(c, 0, target);
        return ans;
    }
    void dfs(vector<int>& c, int u, int target){
        if(target == 0){
            ans.push_back(path);
            return;
        }
        if(u == c.size())   return;
        int i = 0;
        while(c[u] * i <= target){
            dfs(c, u + 1, target - c[u] * i);
            path.push_back(c[u]);
            ++ i;
        }
        while(i--)  path.pop_back();
        return;
    }
};
```

> 执行用时: **12 ms**
>
> 内存消耗: **11.5 MB**

**【时间复杂度】**

O(k^n)  

**【注意点】**

1. 其他解法：无
1. 按每个数u使用i次dfs
