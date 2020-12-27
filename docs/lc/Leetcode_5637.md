**【原题链接】**

[5637. 判断字符串的两半是否相似](https://leetcode-cn.com/problems/determine-if-string-halves-are-alike/)

**【题目描述】**

给你一个偶数长度的字符串 `s` 。将其拆分成长度相同的两半，前一半为 `a` ，后一半为 `b` 。

两个字符串 **相似** 的前提是它们都含有相同数目的元音（`'a'`，`'e'`，`'i'`，`'o'`，`'u'`，`'A'`，`'E'`，`'I'`，`'O'`，`'U'`）。注意，`s` 可能同时含有大写和小写字母。

如果 `a` 和 `b` 相似，返回 `true` ；否则，返回 `false` 。

 **示例 1：**

```
输入：s = "book"
输出：true
解释：a = "bo" 且 b = "ok" 。a 中有 1 个元音，b 也有 1 个元音。所以，a 和 b 相似。
```

**示例 2：**

```
输入：s = "textbook"
输出：false
解释：a = "text" 且 b = "book" 。a 中有 1 个元音，b 中有 2 个元音。因此，a 和 b 不相似。
注意，元音 o 在 b 中出现两次，记为 2 个。
```

**示例 3：**

```
输入：s = "MerryChristmas"
输出：false
```

**示例 4：**

```
输入：s = "AbCdEfGh"
输出：true
```

 **提示：**

- `2 <= s.length <= 1000`
- `s.length` 是偶数
- `s` 由 **大写和小写** 字母组成

**【代码】**

```cpp
class Solution {
public:
    bool halvesAreAlike(string s) {
        int n = s.size(), l = 0, r = 0;
        for(int i = 0; i < n; i++)
            for(auto x : "aeiouAEIOU")
                if(x == s[i])
                    if(i < n / 2)   l ++;
                    else r ++;
        return l == r;
    }
};
```

> 执行用时: **0 ms**
>
> 内存消耗: **6.9 MB**

**【时间复杂度】**

O(10*n)  

**【注意点】**

1. 其他解法：无
1. 一次遍历记录元音字母数
1. **判断一个字母是否在一个集合内**，可以用set存，也可以用for(x : "字符串")自己遍历（时间复杂度会低一点点）
