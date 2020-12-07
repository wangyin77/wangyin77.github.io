**【原题链接】**

[5. 最长回文子串](https://leetcode-cn.com/problems/longest-palindromic-substring/)

**【题目描述】**

给定一个字符串 `s`，找到 `s` 中最长的回文子串。你可以假设 `s` 的最大长度为 1000。

**示例 1：**

```text
输入: "babad"
输出: "bab"
注意: "aba" 也是一个有效答案。
```

**示例 2：**

```text
输入: "cbbd"
输出: "bb"
```

**【代码】**

```cpp
class Solution {
public:
    string longestPalindrome(string s) {
        string res;
        for(int i = 0; i < s.size(); i++){
            int l = i - 1, r = i + 1;
            while(l >= 0 && r < s.size() && s[l] == s[r])
                l--, r++;
            if(res.size() < r - l - 1) 
                res = s.substr(l + 1, r - l - 1);
            l = i, r = i + 1;
            while(l >= 0 && r < s.size() && s[l] == s[r])
                l--, r++;
            if(res.size() < r - l - 1) 
                res = s.substr(l + 1, r - l - 1);
        }
        return res;
    }
};
```

> 执行用时: **44 ms**

> 内存消耗: **14.1 MB**

**【时间复杂度】**

O(n^2) 

**【注意点】**

1. 其他解法：马拉车O(n),特殊做法；哈希表+二分O(nlogn)

1. 哈希+二分的做法下次再补

