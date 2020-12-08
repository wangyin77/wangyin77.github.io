**【原题链接】**

[37. 解数独](https://leetcode-cn.com/problems/sudoku-solver/)

**【题目描述】**

编写一个程序，通过填充空格来解决数独问题。

一个数独的解法需**遵循如下规则**：

1. 数字 `1-9` 在每一行只能出现一次。

1. 数字 `1-9` 在每一列只能出现一次。

1. 数字 `1-9` 在每一个以粗实线分隔的 `3x3` 宫内只能出现一次。

空白格用 `'.'` 表示。

![](http://upload.wikimedia.org/wikipedia/commons/thumb/f/ff/Sudoku-by-L2G-20050714.svg/250px-Sudoku-by-L2G-20050714.svg.png "")

一个数独。

![](http://upload.wikimedia.org/wikipedia/commons/thumb/3/31/Sudoku-by-L2G-20050714_solution.svg/250px-Sudoku-by-L2G-20050714_solution.svg.png "")

答案被标成红色。

**提示：**

- 给定的数独序列只包含数字 `1-9` 和字符 `'.'` 。

- 你可以假设给定的数独只有唯一解。

- 给定数独永远是 `9x9` 形式的。

**【代码】**

```cpp
class Solution {
public:
    bool row[9][9], col[9][9], cell[3][3][9];
    void solveSudoku(vector<vector<char>>& board) {
        memset(row, 0, sizeof row);
        memset(col, 0, sizeof col);
        memset(cell, 0, sizeof cell);
        for(int i = 0; i < 9; i++)
            for(int j = 0; j < 9; j++)
                if(board[i][j] != '.'){
                    int t = board[i][j] - '1';
                    row[i][t] = col[j][t] = cell[i / 3][j / 3][t] = true;
                }
        dfs(board, 0, 0);
    }
    bool dfs(vector<vector<char>>& board, int i, int j){
        if(j == 9) i ++, j = 0;
        if(i == 9) return true;
        if(board[i][j] != '.') return dfs(board, i, ++j);
        for(int t = 0; t < 9; t ++){
            if(!row[i][t] && !col[j][t] && !cell[i / 3][j / 3][t]){
                board[i][j] = '1' + t;
                row[i][t] = col[j][t] = cell[i /3][j /3][t] = true;
                if(dfs(board, i, j + 1)) return true;
                row[i][t] = col[j][t] = cell[i /3][j /3][t] = false;
                board[i][j] = '.';
            }
        }
        return false;
    }
};
```

> 执行用时: **4 ms**

> 内存消耗: **6.8 MB**

**【时间复杂度】**

O(9^n) n为需要填的格数  

**【注意点】**

1. 其他解法：无

1. 保证有唯一解，不需要剪枝，直接dfs一遍即可。

