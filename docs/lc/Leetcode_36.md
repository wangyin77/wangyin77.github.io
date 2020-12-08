**【原题链接】**

[36. 有效的数独](https://leetcode-cn.com/problems/valid-sudoku/)

**【题目描述】**

难度中等448收藏分享切换为英文接收动态反馈

判断一个 9x9 的数独是否有效。只需要**根据以下规则**，验证已经填入的数字是否有效即可。

1. 数字 `1-9` 在每一行只能出现一次。

1. 数字 `1-9` 在每一列只能出现一次。

1. 数字 `1-9` 在每一个以粗实线分隔的 `3x3` 宫内只能出现一次。

![](https://tcs.teambition.net/storage/3120240edecbc2aa39ad87711f73b2d37f9f?Signature=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJBcHBJRCI6IjU5Mzc3MGZmODM5NjMyMDAyZTAzNThmMSIsIl9hcHBJZCI6IjU5Mzc3MGZmODM5NjMyMDAyZTAzNThmMSIsIl9vcmdhbml6YXRpb25JZCI6IiIsImV4cCI6MTYwODAzOTk3NiwiaWF0IjoxNjA3NDM1MTc2LCJyZXNvdXJjZSI6Ii9zdG9yYWdlLzMxMjAyNDBlZGVjYmMyYWEzOWFkODc3MTFmNzNiMmQzN2Y5ZiJ9.l3IT1aWOVnb1k04fr5g9281CX-cijgm6a73w6YE_5dE&download=blob.png "")

上图是一个部分填充的有效的数独。

数独部分空格内已填入了数字，空白格用 `'.'` 表示。

**示例 1:**

```text
输入:
[
  ["5","3",".",".","7",".",".",".","."],
  ["6",".",".","1","9","5",".",".","."],
  [".","9","8",".",".",".",".","6","."],
  ["8",".",".",".","6",".",".",".","3"],
  ["4",".",".","8",".","3",".",".","1"],
  ["7",".",".",".","2",".",".",".","6"],
  [".","6",".",".",".",".","2","8","."],
  [".",".",".","4","1","9",".",".","5"],
  [".",".",".",".","8",".",".","7","9"]
]
输出: true
```

**示例 2:**

```text
输入:
[
  ["8","3",".",".","7",".",".",".","."],
  ["6",".",".","1","9","5",".",".","."],
  [".","9","8",".",".",".",".","6","."],
  ["8",".",".",".","6",".",".",".","3"],
  ["4",".",".","8",".","3",".",".","1"],
  ["7",".",".",".","2",".",".",".","6"],
  [".","6",".",".",".",".","2","8","."],
  [".",".",".","4","1","9",".",".","5"],
  [".",".",".",".","8",".",".","7","9"]
]
输出: false
解释: 除了第一行的第一个数字从 5 改为 8 以外，空格内其他数字均与 示例1 相同。
     但由于位于左上角的 3x3 宫内有两个 8 存在, 因此这个数独是无效的。
```

**说明:**

- 一个有效的数独（部分已被填充）不一定是可解的。

- 只需要根据以上规则，验证已经填入的数字是否有效即可。

- 给定数独序列只包含数字 `1-9` 和字符 `'.'` 。

- 给定数独永远是 `9x9` 形式的。

**【代码】**

```cpp
class Solution {
public:
    bool isValidSudoku(vector<vector<char>>& board) {
        bool st[9];
        for(int i = 0; i < 9; i ++){
            memset(st, 0, sizeof st);
            for(int j = 0; j < 9; j ++)
                if(board[i][j] != '.'){
                    int t = board[i][j] - '1';
                    if(st[t])   return false;
                    else st[t] = true;
                }
        }
        for(int i = 0; i < 9; i ++){
            memset(st, 0, sizeof st);
            for(int j = 0; j < 9; j ++)
                if(board[j][i] != '.'){
                    int t = board[j][i] - '1';
                    if(st[t]) return false;
                    else st[t] = true;
                }
        }
        for(int i = 0; i < 9; i += 3){
            for(int j = 0; j < 9; j += 3){
                memset(st, 0, sizeof st);
                for(int x = 0; x < 3; x ++)
                    for(int y = 0; y < 3; y ++)
                        if(board[i + x][j + y] != '.'){
                            int t = board[i + x][j + y] - '1';
                            if(st[t]) return false;
                            else st[t] = true;
                        }
            }
        }
        return true;
    }
};
```

> 执行用时: **32 ms**

> 内存消耗: **17.8 MB**

**【时间复杂度】**

O(n) n = 81， 也可以看作O(1)

**【注意点】**

1. 其他解法：可以把行列小方格合并起来写，都是O(n)的做法，但能有效减少代码量

```cpp
class Solution {
public:
    bool isValidSudoku(vector<vector<char>>& board) {
        bool row[9][9], col[9][9], cell[3][3][9];
        memset(row, 0, sizeof row);
        memset(col, 0, sizeof col);
        memset(cell, 0, sizeof cell);
        for(int i = 0; i < 9; i++)
            for(int j = 0; j < 9; j++)
                if(board[i][j] != '.'){
                    int t = board[i][j] - '1';
                    if(!row[i][t] && !col[j][t] 
                                  && !cell[i / 3][j / 3][t])
                        row[i][t] = col[j][t] 
                                  = cell[i / 3][j / 3][t] = true;
                    else return false;
                }
        return true;
    }
};
```


