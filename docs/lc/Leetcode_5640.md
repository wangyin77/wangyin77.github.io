**【原题链接】**

[5640. 与数组中元素的最大异或值](https://leetcode-cn.com/problems/maximum-xor-with-an-element-from-array/)

**【题目描述】**

给你一个由非负整数组成的数组 `nums` 。另有一个查询数组 `queries` ，其中 `queries[i] = [xi, mi]` 。

第 `i` 个查询的答案是 `xi` 和任何 `nums` 数组中不超过 `mi` 的元素按位异或（`XOR`）得到的最大值。换句话说，答案是 `max(nums[j] XOR xi)` ，其中所有 `j` 均满足 `nums[j] <= mi` 。如果 `nums` 中的所有元素都大于 `mi`，最终答案就是 `-1` 。

返回一个整数数组 `answer` 作为查询的答案，其中 `answer.length == queries.length` 且 `answer[i]` 是第 `i` 个查询的答案。

 **示例 1：**

```
输入：nums = [0,1,2,3,4], queries = [[3,1],[1,3],[5,6]]
输出：[3,3,7]
解释：
1) 0 和 1 是仅有的两个不超过 1 的整数。0 XOR 3 = 3 而 1 XOR 3 = 2 。二者中的更大值是 3 。
2) 1 XOR 2 = 3.
3) 5 XOR 2 = 7.
```

**示例 2：**

```
输入：nums = [5,2,4,6,6,3], queries = [[12,4],[8,1],[6,3]]
输出：[15,-1,5]
```

 **提示：**

- `1 <= nums.length, queries.length <= 105`
- `queries[i].length == 2`
- `0 <= nums[j], xi, mi <= 109`

**【代码】**

```cpp
class Solution {
public:
    int trie[3100000][2], idx = 0;// 1e5个数字，每个31位2^31(10^9)
    void insert(int x){
        int p = 0;
        for(int i = 30; i >= 0; i--){
            int u = x >> i & 1;
            if(!trie[p][u])  trie[p][u] = ++idx;
            p = trie[p][u];
        }
    }
    int search(int x){
        int res = 0, p = 0;
        for(int i = 30; i >= 0; i--){
            int u = x >> i & 1;
            if(trie[p][!u]) p = trie[p][!u], res += (1 << i);
            else p = trie[p][u];
        }
        return res;
    }
    vector<int> maximizeXor(vector<int>& nums, vector<vector<int>>& Q) {
        memset(trie, 0, sizeof trie);
        vector<pair<pair<int, int>, int>> q;
        int n = nums.size(), m = Q.size();
        vector<int> res(m);
        for(int i = 0; i < m; i++)
            q.push_back(make_pair(make_pair(Q[i][1], Q[i][0]), i));
        sort(q.begin(), q.end());
        sort(nums.begin(), nums.end());
        int i = 0;
        for(auto x : q){
            while(i < n && nums[i] <= x.first.first)
                insert(nums[i++]);
            res[x.second] = idx ? search(x.first.second) : -1;
        }
        return res;
    }
};
```

> 执行用时: **1080 ms**
>
> 内存消耗: **118.2 MB**

**【时间复杂度】**

O(31*(n+m))  

**【注意点】**

1. 其他解法：无

1. Trie树，在421题上加了<m_1的限制，所以需要离线处理，将query按m从小到大排序。之后search之前将对应的数插入。

1. 需要注意的是，如何对query排序。对于`vector<vector<>>`类型的数据结构直接排序是会超时的，所以要把它换成简单的结构再排序。有几种处理手段

   1)存成`vector<pair<pair<int, int>, int>>`形式，好处是不需要写比较函数；

   2)存成`struct{x, m, k;}`需要写比较函数，eg.

   ```cpp
   struct Node {
       int x, m, k;
       bool operator< (const Node& t) const
       {
           return m < t.m;
       }
   }q[100010];
   ```

   3)存成`vector<vector<int>(3)> q`,再手写比较函数，不太好。

1. Trie树模板函数有两个，insert 和 search：

   ```cpp
   int trie[3100000][2], idx = 0;// 1e5个数字，每个31位2^31(10^9)
   void insert(int x){
       int p = 0;
       for(int i = 30; i >= 0; i--){
           int u = x >> i & 1;
           if(!trie[p][u])  trie[p][u] = ++idx;
           p = trie[p][u];
       }
   }
   int search(int x){
       int res = 0, p = 0;
       for(int i = 30; i >= 0; i--){
           int u = x >> i & 1;
           if(trie[p][!u]) p = trie[p][!u], res += (1 << i);
           else p = trie[p][u];
           // 或写成
           //if(trie[p][!u]) p = trie[p][!u], res = res * 2 + 1;
           //else p = trie[p][u], res = res * 2;
      }
       return res;
   }
   ```

1. 
