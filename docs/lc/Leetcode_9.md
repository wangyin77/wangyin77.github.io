**【原题链接】**

[9. 回文数](https://leetcode-cn.com/problems/palindrome-number/)

**【题目描述】**

判断一个整数是否是回文数。回文数是指正序（从左向右）和倒序（从右向左）读都是一样的整数。

**示例 1:**

```text
输入: 121
输出: true
```

**示例 2:**

```text
输入: -121
输出: false
解释: 从左向右读, 为 -121 。 从右向左读, 为 121- 。因此它不是一个回文数。
```

**示例 3:**

```text
输入: 10
输出: false
解释: 从右向左读, 为 01 。因此它不是一个回文数。
```

**进阶:**

你能不将整数转为字符串来解决这个问题吗？

**【代码】**

```cpp
class Solution {
public:
    bool isPalindrome(int x) {
        if(x < 0) return 0;
        string s = to_string(x);
        return s == string(s.rbegin(), s.rend());
    }
};
```

> 执行用时: **16 ms**

> 内存消耗: **6.4 MB**

**【时间复杂度】**

O(logx) 

**【注意点】**

1. 其他解法：数学方法，计算前后的数字O(logx) -> [__7.整数翻转__](https://www.teambition.com/project/5faf7e98dcef8aaa0ba28666/app/5eba5fba6a92214d420a3219/workspaces/5faf7e985ad2c700462acaf0/docs/5fb37cffeaa1190001d7e531) ;可以常数优化，根据奇偶不同，翻转一半复杂度还是O(logx)

1. 翻转字符串：`string(s.rbegin(), s.rend())`

