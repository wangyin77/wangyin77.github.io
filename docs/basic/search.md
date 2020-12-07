包括DFS，BFS，树与图的深度优先遍历，树与图的广度优先遍历，拓扑排序，Dijkstra，bellman-ford，spfa，Floyd，Prim，Kruskal，染色法判定二分图，匈牙利算法等内容。

## 1.排列数字(锁了，可以参考[__数字排列__](https://www.acwing.com/problem/content/47/))

## 2.[__n-皇后问题__](https://www.acwing.com/problem/content/845/)

```cpp
//按行dfs
#include <iostream>
using namespace std;
const int N = 20;
int n;
char g[N][N];
bool col[N], dg[N], udg[N];
void dfs(int u)
{
    if (u == n)
    {
        for (int i = 0; i < n; i ++ ) puts(g[i]);
        puts("");
        return;
    }
    for (int i = 0; i < n; i ++ )
        if (!col[i] && !dg[u + i] && !udg[n - u + i])
        {
            g[u][i] = 'Q';
            col[i] = dg[u + i] = udg[n - u + i] = true;
            dfs(u + 1);
            col[i] = dg[u + i] = udg[n - u + i] = false;
            g[u][i] = '.';
        }
}
int main()
{
    cin >> n;
    for (int i = 0; i < n; i ++ )
        for (int j = 0; j < n; j ++ )
            g[i][j] = '.';
    dfs(0);
    return 0;
}
//按格dfs
#include <iostream>
using namespace std;
const int N = 10;
int n;
bool row[N], col[N], dg[N * 2], udg[N * 2];
char g[N][N];
void dfs(int x, int y, int s)
{
    if (s > n) return;
    if (y == n) y = 0, x ++ ;

    if (x == n)
    {
        if (s == n)
        {
            for (int i = 0; i < n; i ++ ) puts(g[i]);
            puts("");
        }
        return;
    }
    g[x][y] = '.';
    dfs(x, y + 1, s);
    if (!row[x] && !col[y] && !dg[x + y] && !udg[x - y + n])
    {
        row[x] = col[y] = dg[x + y] = udg[x - y + n] = true;
        g[x][y] = 'Q';
        dfs(x, y + 1, s + 1);
        g[x][y] = '.';
        row[x] = col[y] = dg[x + y] = udg[x - y + n] = false;
    }
}
int main()
{
    cin >> n;
    dfs(0, 0, 0);
    return 0;
}
```

## 3.走迷宫

进入：训练模式-搜索-最短路模型-844. 走迷宫

![](https://tcs.teambition.net/storage/3120b41cfd8b7d1cd2b67e9c05c308bd1943?Signature=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJBcHBJRCI6IjU5Mzc3MGZmODM5NjMyMDAyZTAzNThmMSIsIl9hcHBJZCI6IjU5Mzc3MGZmODM5NjMyMDAyZTAzNThmMSIsIl9vcmdhbml6YXRpb25JZCI6IiIsImV4cCI6MTYwNzY3NTI3NiwiaWF0IjoxNjA3MDcwNDc2LCJyZXNvdXJjZSI6Ii9zdG9yYWdlLzMxMjBiNDFjZmQ4YjdkMWNkMmI2N2U5YzA1YzMwOGJkMTk0MyJ9.97J2hFgVDbbOYyGuNsaY5glVUpU7HDtpCUGGdtZYu2I&download=image.png "")

题解页面：[__https://www.acwing.com/activity/content/problem/content/1650/1/__](https://www.acwing.com/activity/content/problem/content/1650/1/)

```cpp
#include <cstring>
#include <iostream>
#include <algorithm>
#include <queue>
using namespace std;
typedef pair<int, int> PII;
const int N = 110;
int n, m;
int g[N][N], d[N][N];
int bfs()
{
    queue<PII> q;
    memset(d, -1, sizeof d);
    d[0][0] = 0;
    q.push({0, 0});
    int dx[4] = {-1, 0, 1, 0}, dy[4] = {0, 1, 0, -1};
    while (q.size())
    {
        auto t = q.front();
        q.pop();
        for (int i = 0; i < 4; i ++ )
        {
            int x = t.first + dx[i], y = t.second + dy[i];
            if (x >= 0 && x < n && y >= 0 && y < m && g[x][y] == 0 && d[x][y] == -1)
            {
                d[x][y] = d[t.first][t.second] + 1;
                q.push({x, y});
            }
        }
    }
    return d[n - 1][m - 1];
}
int main()
{
    cin >> n >> m;
    for (int i = 0; i < n; i ++ )
        for (int j = 0; j < m; j ++ )
            cin >> g[i][j];
    cout << bfs() << endl;
    return 0;
}
```

## 4.[__八数码__](https://www.acwing.com/problem/content/847/)

```text

```

## 5.树的重心（锁了没找到）

看题解：[__https://www.acwing.com/activity/content/problem/content/1652/1/__](https://www.acwing.com/activity/content/problem/content/1652/1/)

## 6.图中点的层次（锁了没找到）

看题解：[__https://www.acwing.com/activity/content/problem/content/1653/1/__](https://www.acwing.com/activity/content/problem/content/1653/1/)

这两道题也是dfs/bfs的模板题，跳过问题不大



**【最短路问题】**

![](https://tcs.teambition.net/storage/3120b889cc4733492fe178d48e78546da40f?Signature=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJBcHBJRCI6IjU5Mzc3MGZmODM5NjMyMDAyZTAzNThmMSIsIl9hcHBJZCI6IjU5Mzc3MGZmODM5NjMyMDAyZTAzNThmMSIsIl9vcmdhbml6YXRpb25JZCI6IiIsImV4cCI6MTYwNzY3NTI3NiwiaWF0IjoxNjA3MDcwNDc2LCJyZXNvdXJjZSI6Ii9zdG9yYWdlLzMxMjBiODg5Y2M0NzMzNDkyZmUxNzhkNDhlNzg1NDZkYTQwZiJ9.SARVXTxJwalwR_BkSxzE2x6lapRdK90sLU3KrTeBpzk&download=image.png "")

## 7.[__Dijkstra求最短路 I__](https://www.acwing.com/problem/content/851/)

```cpp
#include <cstring>
#include <iostream>
#include <algorithm>
using namespace std;
const int N = 510;
int n, m;
int g[N][N];
int dist[N];
bool st[N];
int dijkstra()
{
    memset(dist, 0x3f, sizeof dist);
    dist[1] = 0;
    for (int i = 0; i < n - 1; i ++ )
    {
        int t = -1;
        for (int j = 1; j <= n; j ++ )
            if (!st[j] && (t == -1 || dist[t] > dist[j]))
                t = j;
		if(u == n) break; // 这里可以剪枝
        for (int j = 1; j <= n; j ++ )
            dist[j] = min(dist[j], dist[t] + g[t][j]);

        st[t] = true;
    }
    if (dist[n] == 0x3f3f3f3f) return -1;
    return dist[n];
}
int main()
{
    scanf("%d%d", &n, &m);
    memset(g, 0x3f, sizeof g);
    while (m -- )
    {
        int a, b, c;
        scanf("%d%d%d", &a, &b, &c);
        g[a][b] = min(g[a][b], c);
    }
    printf("%d\n", dijkstra());
    return 0;
}
```

这个板子确实比较好

## 8.[__Dijkstra求最短路 II__](https://www.acwing.com/problem/content/852/)

```cpp
#include <cstring>
#include <iostream>
#include <algorithm>
#include <queue>
using namespace std;
typedef pair<int, int> PII;
const int N = 1e6 + 10;
int n, m;
int h[N], w[N], e[N], ne[N], idx;
int dist[N];
bool st[N]
void add(int a, int b, int c)
{
    e[idx] = b, w[idx] = c, ne[idx] = h[a], h[a] = idx ++ ;
}
int dijkstra()
{
    memset(dist, 0x3f, sizeof dist);
    dist[1] = 0;
    priority_queue<PII, vector<PII>, greater<PII>> heap;
    heap.push({0, 1});
    while (heap.size())
    {
        auto t = heap.top();
        heap.pop();
        int ver = t.second, distance = t.first;//distance其实没用
        if (st[ver]) continue;
        st[ver] = true;
        for (int i = h[ver]; i != -1; i = ne[i])
        {
            int j = e[i];
            if (dist[j] > dist[ver] + w[i])
            {
                dist[j] = dist[ver] + w[i];
                heap.push({dist[j], j});
            }
        }
    }
    if (dist[n] == 0x3f3f3f3f) return -1;
    return dist[n];
}
int main()
{
    scanf("%d%d", &n, &m);
    memset(h, -1, sizeof h);
    while (m -- )
    {
        int a, b, c;
        scanf("%d%d%d", &a, &b, &c);
        add(a, b, c);
    }
    cout << dijkstra() << endl;
    return 0;
}
```

## 9.[__有边数限制的最短路__](https://www.acwing.com/problem/content/855/)

```cpp
#include <cstring>
#include <iostream>
#include <algorithm>
using namespace std;
const int N = 510, M = 10010;
struct Edge
{
    int a, b, c;
}edges[M];
int n, m, k;
int dist[N];
int last[N];
void bellman_ford()
{
    memset(dist, 0x3f, sizeof dist);
    dist[1] = 0;
    for (int i = 0; i < k; i ++ )
    {
        memcpy(last, dist, sizeof dist);
        for (int j = 0; j < m; j ++ )
        {
            auto e = edges[j];
            dist[e.b] = min(dist[e.b], last[e.a] + e.c);
        }
    }
}
int main()
{
    scanf("%d%d%d", &n, &m, &k);
    for (int i = 0; i < m; i ++ )
    {
        int a, b, c;
        scanf("%d%d%d", &a, &b, &c);
        edges[i] = {a, b, c};
    }
    bellman_ford();
    if (dist[n] > 0x3f3f3f3f / 2) puts("impossible");
    else printf("%d\n", dist[n]);
    return 0;
}
```

## 10.[__spfa求最短路__](https://www.acwing.com/problem/content/853/)

```text

```



## 11.

```text

```



## 12.

```text

```



## 13.

```text

```



## 14.

```text

```



## 15.

```text

```



## 16.

```text

```



## 17.

```text

```



## 18.

```text

```



## 19.

```text

```



## 20.

```text

```



