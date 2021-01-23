### [第 177 场周赛](https://leetcode-cn.com/contest/weekly-contest-177/)

**题目列表**

- [日期之间隔几天](https://leetcode-cn.com/problems/number-of-days-between-two-dates/)**4**
- [验证二叉树](https://leetcode-cn.com/problems/validate-binary-tree-nodes/)**5**
- [最接近的因数](https://leetcode-cn.com/problems/closest-divisors/)**5**
- [形成三的最大倍数](https://leetcode-cn.com/problems/largest-multiple-of-three/)**6**

#### 1360. 日期之间隔几天

请你编写一个程序来计算两个日期之间隔了多少天。

日期以字符串形式给出，格式为 `YYYY-MM-DD`，如示例所示。

 **示例 1：**

```
输入：date1 = "2019-06-29", date2 = "2019-06-30"
输出：1
```

**示例 2：**

```
输入：date1 = "2020-01-15", date2 = "2019-12-31"
输出：15
```

 **提示：**

- 给定的日期是 `1971` 年到 `2100` 年之间的有效日期。

------

```c++
class Solution {
public:
    int daysBetweenDates(string date1, string date2) {
        return abs(fun(date1) - fun(date2));
    }
    int m_days[13] = {0, 31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31};
    bool is_leap(int year){
        return year % 100 && year % 4 == 0 || year % 400 == 0;
    }
    int fun(string date){
        int year, month, day;
        sscanf(date.c_str(), "%d-%d-%d", &year, &month, & day); // 重点
        for(int i = 1970; i < year; i++)
            day += 365 + is_leap(i);
        for(int i = 1; i < month; i++)
            if(i == 2) day += 28 + is_leap(year);
            else day += m_days[i];
        return day;
    }
};
```

#### 1361. 验证二叉树

二叉树上有 `n` 个节点，按从 `0` 到 `n - 1` 编号，其中节点 `i` 的两个子节点分别是 `leftChild[i]` 和 `rightChild[i]`。

只有 **所有** 节点能够形成且 **只** 形成 **一颗** 有效的二叉树时，返回 `true`；否则返回 `false`。

如果节点 `i` 没有左子节点，那么 `leftChild[i]` 就等于 `-1`。右子节点也符合该规则。

注意：节点没有值，本问题中仅仅使用节点编号。

 

**示例 1：**

**![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/02/23/1503_ex1.png)**

```
输入：n = 4, leftChild = [1,-1,3,-1], rightChild = [2,-1,-1,-1]
输出：true
```

**示例 2：**

**![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/02/23/1503_ex2.png)**

```
输入：n = 4, leftChild = [1,-1,3,-1], rightChild = [2,3,-1,-1]
输出：false
```

**示例 3：**

**![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/02/23/1503_ex3.png)**

```
输入：n = 2, leftChild = [1,0], rightChild = [-1,-1]
输出：false
```

**示例 4：**

**![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/02/23/1503_ex4.png)**

```
输入：n = 6, leftChild = [1,-1,-1,4,-1,-1], rightChild = [2,-1,-1,5,-1,-1]
输出：false
```

 **提示：**

- `1 <= n <= 10^4`
- `leftChild.length == rightChild.length == n`
- `-1 <= leftChild[i], rightChild[i] <= n - 1`

------

```c++
class Solution {
public:
    bool validateBinaryTreeNodes(int n, vector<int>& leftChild, vector<int>& rightChild) {
        // 一个错误解法：
        // 只判断入度只有一个入度0，其余入度均为1是不够的
        // 还有判断整个图的连通性
        // 【参考样例】
        // 4
        // [1,0,3,-1]
        // [-1,-1,-1,-1]

        // vector<int> d(n);
        // for(auto &x : leftChild) if(x != -1) d[x] ++;
        // for(auto &x : rightChild) if(x != -1) d[x] ++;
        // int cnt = 0;
        // for(auto &x : d)
        //     if(x > 1) return false;
        //     else if(x == 0) cnt ++;
        // return cnt == 1;
        vector<int> d(n);
        for(auto &x : leftChild) if(x != -1) d[x] ++;
        for(auto &x : rightChild) if(x != -1) d[x] ++;
        int cnt = 0, root;
        for(int i = 0; i < n; i++)
            if(d[i] > 1) return false;
            else if(d[i] == 0) cnt ++, root = i;
        if(cnt != 1) return false;
        queue<int> q;
        cnt = 0;
        q.push(root);
        while(q.size()){
            int p = q.front();
            q.pop();
            if(leftChild[p] != -1) q.push(leftChild[p]);
            if(rightChild[p] != -1) q.push(rightChild[p]);
            cnt ++;
        }
        return cnt == n;
    }
};
```

#### 1362. 最接近的因数

给你一个整数 `num`，请你找出同时满足下面全部要求的两个整数：

- 两数乘积等于  `num + 1` 或 `num + 2`
- 以绝对差进行度量，两数大小最接近

你可以按任意顺序返回这两个整数。

 **示例 1：**

```
输入：num = 8
输出：[3,3]
解释：对于 num + 1 = 9，最接近的两个因数是 3 & 3；对于 num + 2 = 10, 最接近的两个因数是 2 & 5，因此返回 3 & 3 。
```

**示例 2：**

```
输入：num = 123
输出：[5,25]
```

**示例 3：**

```
输入：num = 999
输出：[40,25]
```

 **提示：**

- `1 <= num <= 10^9`

------

```
class Solution {
public:
    vector<int> closestDivisors(int num) {
        for(int i = sqrt(num + 2); i; i--){
            if((num + 1) % i == 0) return {i, (num + 1) / i};
            if((num + 2) % i == 0) return {i, (num + 2) / i};
        }
        return {};
    }
};
```

#### 1363. 形成三的最大倍数

给你一个整数数组 `digits`，你可以通过按任意顺序连接其中某些数字来形成 **3** 的倍数，请你返回所能得到的最大的 3 的倍数。

由于答案可能不在整数数据类型范围内，请以字符串形式返回答案。

如果无法得到答案，请返回一个空字符串。

 **示例 1：**

```
输入：digits = [8,1,9]
输出："981"
```

**示例 2：**

```
输入：digits = [8,6,7,1,0]
输出："8760"
```

**示例 3：**

```
输入：digits = [1]
输出：""
```

**示例 4：**

```
输入：digits = [0,0,0,0,0,0]
输出："0"
```

 **提示：**

- `1 <= digits.length <= 10^4`
- `0 <= digits[i] <= 9`
- 返回的结果不应包含不必要的前导零。

------

```c++
class Solution {
public:
    string largestMultipleOfThree(vector<int>& digits) {
        int n = digits.size(), f[n + 1][3];
        memset(f[0], 0, sizeof f);
        f[0][1] = f[0][2] = -1e8;
        sort(digits.begin(), digits.end());
        for(int i = 1; i <= n; i++)
            for(int j = 0; j < 3; j++)
                f[i][j] = max(f[i - 1][j], f[i - 1][(j + 3 - digits[i - 1] % 3) % 3] + 1);
        if(f[n][0] <= 0)    return "";
        string res;
        for(int i = n, j = 0; i; i--)
            if(f[i][j] == f[i - 1][(j + 3 - digits[i - 1] % 3) % 3] + 1){
                res += to_string(digits[i - 1]);
                j = (j + 3 - digits[i - 1] % 3 ) % 3;
                if(res == "0") return res;
            }
        return res;
    }
};
```