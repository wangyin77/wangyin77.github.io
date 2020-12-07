**【原题链接】**

[3. 无重复字符的最长子串](https://leetcode-cn.com/problems/longest-substring-without-repeating-characters/)

**【题目描述】**

给定一个字符串，请你找出其中不含有重复字符的 **最长子串 **的长度。

**示例 1:**

```text
输入: "abcabcbb"
输出: 3 
解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。
```

**示例 2:**

```text
输入: "bbbbb"
输出: 1
解释: 因为无重复字符的最长子串是 "b"，所以其长度为 1。
```

**示例 3:**

```text
输入: "pwwkew"
输出: 3
解释: 因为无重复字符的最长子串是 "wke"，所以其长度为 3。
     请注意，你的答案必须是 子串 的长度，"pwke" 是一个子序列，不是子串。
```

**【代码】**

```cpp
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        unordered_map<char, int> heap;
        int res = 0;
        for(int i = 0, j = 0; i < s.size(); i++){
            heap[s[i]] ++;
            while(heap[s[i]] > 1)
                heap[s[j++]]--;
            res = max(res, i - j +1);
        }
        return res;
    }
};
```

> 执行用时: **40 ms**

> 内存消耗: **8.6 MB**

**【时间复杂度】**

双指针O(n) 

**【注意点】**

1. 其他解法：暴力枚举O(n^2)、用集合存O(nlogn)

1. 注意这里面哈希集的操作，`heap[s[i]] ++;` 是将s[i]先加入到heap中，并自增1。自增1的目的是在后面判断s[i]是否在heap中，不需要用.count(),以及删除s[j]时不需要.erase(),只需要判断是否大于1即可。

1. 可以先判断map里有没有s[i]再把s[i]加进来，但是速度上会慢，主要原因就是上面说的，本来就要s[i]加进来，加后自增只需要再减成1就可以，不然需要先删再加，耗时增加。举例如下：

```cpp
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        unordered_map<char,int> heap;
        int res = 0;
        for(int i = 0, j = 0; i < s.size(); i++){
            while(heap.count(s[i]))
                heap.erase(s[j++]);//erase比--浪费时间
            res = max(res, i - j +1);
            heap[s[i]];
        }
        return res;
    }
};
```

> 执行用时: **68 ms**

> 内存消耗: **11.1 MB**

