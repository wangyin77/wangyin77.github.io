**【原题链接】**

[32. 最长有效括号](https://leetcode-cn.com/problems/longest-valid-parentheses/)

**【题目描述】**

给定一个只包含 `'('` 和 `')'` 的字符串，找出最长的包含有效括号的子串的长度。

**示例 1:**

```text
输入: "(()"
输出: 2
解释: 最长有效括号子串为 "()"
```

**示例 2:**

```text
输入: ")()())"
输出: 4
解释: 最长有效括号子串为 "()()"
```

**【代码】**

```cpp
class Solution {
public:
    int longestValidParentheses(string s) {
        stack<int> stk;
        int res = 0, start = -1;
        for(int i = 0; i < s.size(); i++){
            if(s[i] == '('){
                stk.push(i);
            }else{
                if(stk.size()){
                    stk.pop();
                    if(!stk.size()){
                        res = max(res, i - start);
                    }else{
                        res = max(res, i - stk.top());//必须要实时更新，eg")((()())("
                    }
                }else{
                    start = i;
                }
            }
        }
        return res;
    }
};

```

> 执行用时: **8 ms**

> 内存消耗: **7.5 MB**

**【时间复杂度】**

O(n) 

**【注意点】**

1. 其他解法：无

1. 比较难的

1. 解题思路：首先要分段，从左往右遍历，如果一个序列左括号小于右括号，则包含这段序列的有效子序列最多就这么长了；之后统计有效长度，采用栈存储左括号的id，每次遇到右括号弹出匹配的左括号并计算更新右括号到栈顶元素或者区间最左侧（不是它对应的那个左括号）的距离。

1. 证明分段处理正确：

![](https://tcs.teambition.net/storage/311zccabe244eeda0978f3f76d2a889c716a?Signature=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJBcHBJRCI6IjU5Mzc3MGZmODM5NjMyMDAyZTAzNThmMSIsIl9hcHBJZCI6IjU5Mzc3MGZmODM5NjMyMDAyZTAzNThmMSIsIl9vcmdhbml6YXRpb25JZCI6IiIsImV4cCI6MTYwNzkzMzcxMCwiaWF0IjoxNjA3MzI4OTEwLCJyZXNvdXJjZSI6Ii9zdG9yYWdlLzMxMXpjY2FiZTI0NGVlZGEwOTc4ZjNmNzZkMmE4ODljNzE2YSJ9.39SLjWcnddNHlLrXweUxw68srylSziZKE5-OTCImoXU&download=sendpix0.jpg "")

