**【原题链接】**

[6. Z 字形变换](https://leetcode-cn.com/problems/zigzag-conversion/)

**【题目描述】**

将一个给定字符串根据给定的行数，以从上往下、从左到右进行 Z 字形排列。

比如输入字符串为 `"LEETCODEISHIRING"` 行数为 3 时，排列如下：

```text
L   C   I   R
E T O E S I I G
E   D   H   N
```

之后，你的输出需要从左往右逐行读取，产生出一个新的字符串，比如：`"LCIRETOESIIGEDHN"`。

请你实现这个将字符串进行指定行数变换的函数：

```text
string convert(string s, int numRows);
```

**示例 1:**

```text
输入: s = "LEETCODEISHIRING", numRows = 3
输出: "LCIRETOESIIGEDHN"
```

**示例 2:**

```text
输入: s = "LEETCODEISHIRING", numRows = 4
输出: "LDREOEIIECIHNTSG"
解释:
L     D     R
E   O E   I I
E C   I H   N
T     S     G
```

**【代码】**

```cpp
class Solution {
public:
    string convert(string s, int n) {
        if(n == 1) return s;
        string res;
        for(int i = 0; i < n; i++){
            if(i == 0|| i == n - 1){
                for(int j = i; j < s.size(); j += 2 * n - 2)
                    res += s[j];
            }else{
                for(int j = i; j < s.size(); j += 2 * n - 2){
                    res += s[j];
                    if(j + 2 * n - 2 - 2 * i < s.size())
                        res += s[j + 2 * n - 2 - 2 * i];
                }
            }
        }
        return res;
    }
};
```

> 执行用时: **16 ms**

> 内存消耗: **8.3 MB**

**【时间复杂度】**

O(n) 

**【注意点】**

1. 其他解法：定义两个方向，类似蛇形矩阵，得到下标值O(n),

1. 画出下标对应的图，找规律，每行下标都是公差为2N-2的等差数列。直接索引输出即可。

1. 自己的做法：开n个数组把对应下标的字母存进去， O(n)

```cpp
class Solution {
public:
    string convert(string s, int n) {
        if(n == 1) return s;
        int m = 2 * n - 2, idx = 0;
        string a[n], res;
        for(auto x : s){
            int i = idx ++ % m;
            if(i < n)   a[i] += x;
            else a[m - i] += x;
        }
        for(int i = 0; i < n; i++)
            res += a[i];
        return res;
    }
};
//余数分布
0      --
1      2n - 3
2      .
.      .
.      n
n - 1  --
//
```

