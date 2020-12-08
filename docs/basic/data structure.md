包括单链表，双链表，栈，队列，单调栈，单调队列，KMP，Trie，并查集，堆，哈希表等内容。

## 1.[__单链表__](https://www.acwing.com/problem/content/description/828/)

```cpp
#include <iostream>
using namespace std;
const int N = 100010;
// head 表示头结点的下标
// e[i] 表示节点i的值
// ne[i] 表示节点i的next指针是多少
// idx 存储当前已经用到了哪个点
int head, e[N], ne[N], idx;
// 初始化
void init()
{
    head = -1;
    idx = 0;
}
// 将x插到头结点
void add_to_head(int x)
{
    e[idx] = x, ne[idx] = head, head = idx ++ ;
}
// 将x插到下标是k的点后面
void add(int k, int x)
{
    e[idx] = x, ne[idx] = ne[k], ne[k] = idx ++ ;
}
// 将下标是k的点后面的点删掉
void remove(int k)
{
    ne[k] = ne[ne[k]];
}
int main()
{
    int m;
    cin >> m;
    init();
    while (m -- )
    {
        int k, x;
        char op;
        cin >> op;
        if (op == 'H')
        {
            cin >> x;
            add_to_head(x);
        }
        else if (op == 'D')
        {
            cin >> k;
            if (!k) head = ne[head];
            else remove(k - 1);
        }
        else
        {
            cin >> k >> x;
            add(k - 1, x);
        }
    }
    for (int i = head; i != -1; i = ne[i]) cout << e[i] << ' ';
    cout << endl;
    return 0;
}
```

稍微简写一点:

```cpp
#include<iostream>
using namespace std;
const int N = 1e5 + 10;
int head = -1, e[N], ne[N], idx = 0;
void add0(int x){
    e[idx] = x, ne[idx] = head, head = idx++;
}
void add(int k, int x){
    e[idx] = x, ne[idx] = ne[k - 1], ne[k - 1] = idx++;
}
void remove(int k){
    if(k == 0) {
		head = ne[head];
    }else {
        ne[k - 1] = ne[ne[k - 1]];
    }
}
int main(){
    int m, k, x;
    cin >> m;
    while(m--){
        char op;
        cin >> op;
        if(op == 'H'){
            cin >> x, add0(x);
        }else if(op == 'D'){
            cin >> k, remove(k);
        }else{
            cin >> k >> x, add(k, x);
        }
    }
    for(int i = head; i != -1; i = ne[i])
        cout << e[i] << " ";
    return 0;
}
```

## 2.[__双链表__](https://www.acwing.com/problem/content/829/)

```cpp
#include <iostream>
using namespace std;
const int N = 100010;
int m;
int e[N], l[N], r[N], idx;
// 在节点a的右边插入一个数x
void insert(int a, int x)
{
    e[idx] = x;
    l[idx] = a, r[idx] = r[a];
    l[r[a]] = idx, r[a] = idx ++ ;
}
// 删除节点a
void remove(int a)
{
    l[r[a]] = l[a];
    r[l[a]] = r[a];
}
int main()
{
    cin >> m;
    // 0是左端点，1是右端点
    r[0] = 1, l[1] = 0;
    idx = 2;
    while (m -- )
    {
        string op;
        cin >> op;
        int k, x;
        if (op == "L")
        {
            cin >> x;
            insert(0, x);
        }
        else if (op == "R")
        {
            cin >> x;
            insert(l[1], x);
        }
        else if (op == "D")
        {
            cin >> k;
            remove(k + 1);
        }
        else if (op == "IL")
        {
            cin >> k >> x;
            insert(l[k + 1], x);
        }
        else
        {
            cin >> k >> x;
            insert(k + 1, x);
        }
    }
    for (int i = r[0]; i != 1; i = r[i]) cout << e[i] << ' ';
    cout << endl;
    return 0;
}
```

单链表中 (k - 1)是因为idx从0开始，双链表(k + 1)是因为idx从2开始的

```cpp
#include<iostream>
using namespace std;
const int N = 1e5 + 10;
int e[N], l[N], r[N], idx;
void insert(int k, int x){
    e[idx] = x, l[idx] = k, r[idx] = r[k], l[r[k]] = idx, r[k] = idx ++; 
}
void remove(int k){
    l[r[k]] = l[k], r[l[k]] = r[k];
}
int main(){
    int m, k, x;
    cin >> m;
    r[0] = 1, l[1] = 0, idx = 2;
    char op;
    while(m--){
        cin >> op;
        if(op == 'L'){
            cin >> x;
            insert(0, x);
        }else if(op == 'R'){
            cin >> x;
            insert(l[1], x);
        }else if(op == 'D'){
            cin >> k;
            remove(k + 1);
        }else{
            cin >> op >> k >> x;
            if(op == 'L'){
                insert(l[k + 1], x);
            }else{
                insert(k + 1, x);
            }
        }
    }
    for(int i = r[0]; i != 1; i = r[i])
        cout << e[i] << " ";
    return 0;
}
```

## 3.模拟栈（普通模式已锁）

进入：训练模式-数据结构-栈-828. 模拟栈

题解界面:[__https://www.acwing.com/problem/content/solution/830/1/__](https://www.acwing.com/problem/content/solution/830/1/)

```cpp
#include <iostream>
using namespace std;
const int N = 100010;
int m;
int stk[N], tt;
int main()
{
    cin >> m;
    while (m -- )
    {
        string op;
        int x;
        cin >> op;
        if (op == "push")
        {
            cin >> x;
            stk[ ++ tt] = x;
        }
        else if (op == "pop") tt -- ;
        else if (op == "empty") cout << (tt ? "NO" : "YES") << endl;
        else cout << stk[tt] << endl;
    }
    return 0;
}
```

## 4.模拟队列（普通模式已锁）

进入：训练模式-数据结构-队列-829. 模拟队列

题解页面:[__https://www.acwing.com/problem/content/solution/831/1/__](https://www.acwing.com/problem/content/solution/831/1/)

```cpp
#include <iostream>
using namespace std;
const int N = 100010;
int m;
int q[N], hh, tt = -1;
int main()
{
    cin >> m;
    while (m -- )
    {
        string op;
        int x;
        cin >> op;
        if (op == "push")
        {
            cin >> x;
            q[ ++ tt] = x;
        }
        else if (op == "pop") hh ++ ;
        else if (op == "empty") cout << (hh <= tt ? "NO" : "YES") << endl;
        else cout << q[hh] << endl;
    }
    return 0;
}
```

## 5. 单调栈（普通模式已锁）

进入：训练模式-数据结构- 单调栈-830. 单调栈

题解页面:[__https://www.acwing.com/problem/content/solution/832/1/__](https://www.acwing.com/problem/content/solution/832/1/)

```cpp
#include <iostream>
using namespace std;
const int N = 100010;
int stk[N], tt;
int main()
{
    int n;
    cin >> n;
    while (n -- )
    {
        int x;
        scanf("%d", &x);
        while (tt && stk[tt] >= x) tt -- ;
        if (!tt) printf("-1 ");
        else printf("%d ", stk[tt]);
        stk[ ++ tt] = x;
    }
    return 0;
}
```

## 6.[__滑动窗口__](https://www.acwing.com/problem/content/description/156/)

```cpp
#include <iostream>
using namespace std;
const int N = 1000010;
int a[N], q[N];
int main()
{
    int n, k;
    scanf("%d%d", &n, &k);
    for (int i = 0; i < n; i ++ ) scanf("%d", &a[i]);
    int hh = 0, tt = -1;
    for (int i = 0; i < n; i ++ )
    {
        if (hh <= tt && i - k + 1 > q[hh]) hh ++ ;
        while (hh <= tt && a[q[tt]] >= a[i]) tt -- ;
        q[ ++ tt] = i;
        if (i >= k - 1) printf("%d ", a[q[hh]]);
    }
    puts("");
    hh = 0, tt = -1;
    for (int i = 0; i < n; i ++ )
    {
        if (hh <= tt && i - k + 1 > q[hh]) hh ++ ;
        while (hh <= tt && a[q[tt]] <= a[i]) tt -- ;
        q[ ++ tt] = i;
        if (i >= k - 1) printf("%d ", a[q[hh]]);
    }
    puts("");
    return 0;
}
```

`if`那句中`hh <= tt`其实可以不需要

## 7.KMP字符串（普通模式已锁）

进入：训练模式-数据结构-KMP-831. KMP字符串

题解页面:[__https://www.acwing.com/problem/content/solution/833/1/__](https://www.acwing.com/problem/content/solution/833/1/)

```cpp
#include <iostream>
using namespace std;
const int N = 100010, M = 1000010;
int n, m;
int ne[N];
char s[M], p[N];
int main()
{
    cin >> n >> p + 1 >> m >> s + 1;
    for (int i = 2, j = 0; i <= n; i ++ )
    {
        while (j && p[i] != p[j + 1]) j = ne[j];
        if (p[i] == p[j + 1]) j ++ ;
        ne[i] = j;
    }
    for (int i = 1, j = 0; i <= m; i ++ )
    {
        while (j && s[i] != p[j + 1]) j = ne[j];
        if (s[i] == p[j + 1]) j ++ ;
        if (j == n)
        {
            printf("%d ", i - n);
            j = ne[j];
        }
    }
    return 0;
}
```

比较不好理解,重点在理解递归的过程, 即`j = ne[j]`这一步.

得到ne数组的过程是自己和自己求匹配的过程

## 8.Trie字符串统计（普通模式已锁）

进入：训练模式-数据结构-Tire-835. Trie字符串统计

题解页面:[__https://www.acwing.com/problem/content/solution/837/1/__](https://www.acwing.com/problem/content/solution/837/1/)

```cpp
#include <iostream>
using namespace std;
const int N = 100010;
int son[N][26], cnt[N], idx;
char str[N];
void insert(char *str)
{
    int p = 0;
    for (int i = 0; str[i]; i ++ )
    {
        int u = str[i] - 'a';
        if (!son[p][u]) son[p][u] = ++ idx;
        p = son[p][u];
    }
    cnt[p] ++ ;
}
int query(char *str)
{
    int p = 0;
    for (int i = 0; str[i]; i ++ )
    {
        int u = str[i] - 'a';
        if (!son[p][u]) return 0;
        p = son[p][u];
    }
    return cnt[p];
}
int main()
{
    int n;
    scanf("%d", &n);
    while (n -- )
    {
        char op[2];
        scanf("%s%s", op, str);
        if (*op == 'I') insert(str);
        else printf("%d\n", query(str));
    }
    return 0;
}
```

## 9.[__最大异或对__](https://www.acwing.com/problem/content/145/)

```cpp
#include <iostream>
#include <algorithm>
using namespace std;
const int N = 100010, M = 3100010;
int n;
int a[N], son[M][2], idx;
void insert(int x)
{
    int p = 0;
    for (int i = 30; i >= 0; i -- )
    {
        int &s = son[p][x >> i & 1];
        if (!s) s = ++ idx;
        p = s;
    }
}
int search(int x)
{
    int p = 0, res = 0;
    for (int i = 30; i >= 0; i -- )
    {
        int s = x >> i & 1;
        if (son[p][!s])
        {
            res += 1 << i;
            p = son[p][!s];
        }
        else p = son[p][s];
    }
    return res;
}
int main()
{
    scanf("%d", &n);
    for (int i = 0; i < n; i ++ )
    {
        scanf("%d", &a[i]);
        insert(a[i]);
    }
    int res = 0;
    for (int i = 0; i < n; i ++ ) res = max(res, search(a[i]));
    printf("%d\n", res);
    return 0;
}
```

`quary`函数返回的是结果还是拿来抑或的数都是可以的,可以参考我的:

```cpp
#include<iostream>
using namespace std;
const int N = 1e5 + 10, M = 31 * N;
int son[M][2], idx;
void insert(int x){
    int p = 0;
    for(int i = 30; i >= 0; i--){
        int u = x >> i & 1;
        if(son[p][u] == 0) son[p][u] = ++idx;
        p = son[p][u];
    }
}
int quary(int x){
    int p = 0, res = 0;
    for(int i = 30; i >= 0; i--){
        int u = x >> i & 1;
        if(son[p][!u] != 0){
            res = res * 2 + !u;
            p = son[p][!u];
        }else{
            res = res * 2 + u;
            p = son[p][u];
        }
    }
    return res;
}
int main(){
    int n, tmp, res = 0;
    cin >> n;
    while(n--){
        cin >> tmp;
        insert(tmp);
        res = max(res, tmp ^ quary(tmp));
    }
    cout << res;
    return 0;
}
```

## 10.[__合并集合__](https://www.acwing.com/problem/content/838/)

```cpp
#include <iostream>
using namespace std;
const int N = 100010;
int p[N];
int find(int x)
{
    if (p[x] != x) p[x] = find(p[x]);
    return p[x];
}
int main()
{
    int n, m;
    scanf("%d%d", &n, &m);
    for (int i = 1; i <= n; i ++ ) p[i] = i;
    while (m -- )
    {
        char op[2];
        int a, b;
        scanf("%s%d%d", op, &a, &b);
        if (*op == 'M') p[find(a)] = find(b);
        else
        {
            if (find(a) == find(b)) puts("Yes");
            else puts("No");
        }
    }
    return 0;
}
```

注意:`find`里面是`if`操作不是`while`操作,找了很久的错,不然会TLE.

第二个是视频里说的,用`char数组`读入`op`

## 11.连通块中点的数量（普通模式已锁）

进入：训练模式-数据结构-并查集-837. 连通块中点的数量

注意: 这个题有个前置节点`1250.格子游戏`,并且也是锁的

题面如下

![](https://tcs.teambition.net/storage/311zc84f37889cb0c2321c61e72156b6500d?Signature=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJBcHBJRCI6IjU5Mzc3MGZmODM5NjMyMDAyZTAzNThmMSIsIl9hcHBJZCI6IjU5Mzc3MGZmODM5NjMyMDAyZTAzNThmMSIsIl9vcmdhbml6YXRpb25JZCI6IiIsImV4cCI6MTYwNzY3NTI2NCwiaWF0IjoxNjA3MDcwNDY0LCJyZXNvdXJjZSI6Ii9zdG9yYWdlLzMxMXpjODRmMzc4ODljYjBjMjMyMWM2MWU3MjE1NmI2NTAwZCJ9.kLRgQMQCqjgNarcPM8njPmyRhxIOgTNos68oaIGjqZI&download=image.png "")

![](https://tcs.teambition.net/storage/311za4b8d92cf23928bf3978156db3d2bb46?Signature=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJBcHBJRCI6IjU5Mzc3MGZmODM5NjMyMDAyZTAzNThmMSIsIl9hcHBJZCI6IjU5Mzc3MGZmODM5NjMyMDAyZTAzNThmMSIsIl9vcmdhbml6YXRpb25JZCI6IiIsImV4cCI6MTYwNzY3NTI2NCwiaWF0IjoxNjA3MDcwNDY0LCJyZXNvdXJjZSI6Ii9zdG9yYWdlLzMxMXphNGI4ZDkyY2YyMzkyOGJmMzk3ODE1NmRiM2QyYmI0NiJ9.OX0CjjD6SdjwSCiAxGsMjlkuvGv_kDKM3frKmZHe-OM&download=image.png "")

来自**算法提高课**4.1-第一题

```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>
using namespace std;
const int N = 40010;
int n, m;
int p[N];
int get(int x, int y)
{
    return x * n + y;
}
int find(int x)
{
    if (p[x] != x) p[x] = find(p[x]);
    return p[x];
}
int main()
{
    cin >> n >> m;
    for (int i = 0; i < n * n; i ++ ) p[i] = i;
    int res = 0;
    for (int i = 1; i <= m; i ++ )
    {
        int x, y;
        char d;
        cin >> x >> y >> d;
        x --, y -- ;
        int a = get(x, y);
        int b;
        if (d == 'D') b = get(x + 1, y);
        else b = get(x, y + 1);

        int pa = find(a), pb = find(b);
        if (pa == pb)
        {
            res = i;
            break;
        }
        p[pa] = pb;
    }
    if (!res) puts("draw");
    else cout << res << endl;
    return 0;
}
```

数据很水,调试没过,提交过了...很神奇

题解界面:[__https://www.acwing.com/problem/content/solution/839/1/__](https://www.acwing.com/problem/content/solution/839/1/)

```cpp
#include <iostream>
using namespace std;
const int N = 100010;
int n, m;
int p[N], cnt[N];
int find(int x)
{
    if (p[x] != x) p[x] = find(p[x]);
    return p[x];
}
int main()
{
    cin >> n >> m;
    for (int i = 1; i <= n; i ++ )
    {
        p[i] = i;
        cnt[i] = 1;
    }
    while (m -- )
    {
        string op;
        int a, b;
        cin >> op;
        if (op == "C")
        {
            cin >> a >> b;
            a = find(a), b = find(b);
            if (a != b)
            {
                p[a] = b;
                cnt[b] += cnt[a];
            }
        }
        else if (op == "Q1")
        {
            cin >> a >> b;
            if (find(a) == find(b)) puts("Yes");
            else puts("No");
        }
        else
        {
            cin >> a;
            cout << cnt[find(a)] << endl;
        }
    }
    return 0;
}
```

## 12.[__食物链__](https://www.acwing.com/problem/content/description/242/)

这题挺难的，来自POJ1182，可以看看 [__这个题解__](https://blog.csdn.net/m0_37579232/article/details/79920785?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.control&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.control)

```cpp
#include <iostream>
using namespace std;
const int N = 50010;
int n, m;
int p[N], d[N];
int find(int x)
{
    if (p[x] != x)
    {
        int t = find(p[x]);
        d[x] += d[p[x]];
        p[x] = t;
    }
    return p[x];
}
int main()
{
    scanf("%d%d", &n, &m);
    for (int i = 1; i <= n; i ++ ) p[i] = i;
    int res = 0;
    while (m -- )
    {
        int t, x, y;
        scanf("%d%d%d", &t, &x, &y);
        if (x > n || y > n) res ++ ;
        else
        {
            int px = find(x), py = find(y);
            if (t == 1)
            {
                if (px == py && (d[x] - d[y]) % 3) res ++ ;
                else if (px != py)
                {
                    p[px] = py;
                    d[px] = d[y] - d[x];
                }
            }
            else
            {
                if (px == py && (d[x] - d[y] - 1) % 3) res ++ ;
                else if (px != py)
                {
                    p[px] = py;
                    d[px] = d[y] + 1 - d[x];
                }
            }
        }
    }
    printf("%d\n", res);
    return 0;
}
```

下面是自己写的，比较简洁

```cpp
#include<iostream>
using namespace std;
const int N = 5e5 + 10;
int p[N], d[N];
int n, m, res, op, x, y;
int find(int x){
    if(p[x] == x)   return x;
    int tmp = p[x];
    p[x] = find(p[x]);
    d[x] += d[tmp];
    return p[x];
}
int main(){
    cin >> n >> m;
    for(int i = 1; i <= n; i++) p[i] = i;
    while(m--){
        cin >> op >> x >> y;
        --op;
        if(x > n || y > n) {
            res++; continue;
        }
        int px = find(x), py = find(y);
        if(px == py){
            if((d[x] - d[y] - op) % 3) res ++;
        }else{
            p[px] = py, d[px] = d[y] - d[x] + op;
        }
    }
    cout << res;
    return 0;
}
```

## 13.[__堆排序__](https://www.acwing.com/problem/content/840/)

```cpp
#include <iostream>
#include <algorithm>
using namespace std;
const int N = 100010;
int n, m;
int h[N], cnt;
void down(int u)
{
    int t = u;
    if (u * 2 <= cnt && h[u * 2] < h[t]) t = u * 2;
    if (u * 2 + 1 <= cnt && h[u * 2 + 1] < h[t]) t = u * 2 + 1;
    if (u != t)
    {
        swap(h[u], h[t]);
        down(t);
    }
}
int main()
{
    scanf("%d%d", &n, &m);
    for (int i = 1; i <= n; i ++ ) scanf("%d", &h[i]);
    cnt = n;
    for (int i = n / 2; i; i -- ) down(i);
    while (m -- )
    {
        printf("%d ", h[1]);
        h[1] = h[cnt -- ];
        down(1);
    }
    puts("");
    return 0;
}
```

## 14.模拟堆

进入：训练模式-数据结构-堆-839. 模拟堆

题解页面：[__https://www.acwing.com/problem/content/solution/841/1/__](https://www.acwing.com/problem/content/solution/841/1/)

```cpp
#include <iostream>
#include <algorithm>
#include <string.h>
using namespace std;
const int N = 100010;
int h[N], ph[N], hp[N], cnt;
void heap_swap(int a, int b)
{
    swap(ph[hp[a]],ph[hp[b]]);
    swap(hp[a], hp[b]);
    swap(h[a], h[b]);
}
void down(int u)
{
    int t = u;
    if (u * 2 <= cnt && h[u * 2] < h[t]) t = u * 2;
    if (u * 2 + 1 <= cnt && h[u * 2 + 1] < h[t]) t = u * 2 + 1;
    if (u != t)
    {
        heap_swap(u, t);
        down(t);
    }
}
void up(int u)
{
    while (u / 2 && h[u] < h[u / 2])
    {
        heap_swap(u, u / 2);
        u >>= 1;
    }
}
int main()
{
    int n, m = 0;
    scanf("%d", &n);
    while (n -- )
    {
        char op[5];
        int k, x;
        scanf("%s", op);
        if (!strcmp(op, "I"))
        {
            scanf("%d", &x);
            cnt ++ ;
            m ++ ;
            ph[m] = cnt, hp[cnt] = m;
            h[cnt] = x;
            up(cnt);
        }
        else if (!strcmp(op, "PM")) printf("%d\n", h[1]);
        else if (!strcmp(op, "DM"))
        {
            heap_swap(1, cnt);
            cnt -- ;
            down(1);
        }
        else if (!strcmp(op, "D"))
        {
            scanf("%d", &k);
            k = ph[k];
            heap_swap(k, cnt);
            cnt -- ;
            up(k);
            down(k);
        }
        else
        {
            scanf("%d%d", &k, &x);
            k = ph[k];
            h[k] = x;
            up(k);
            down(k);
        }
    }
    return 0;
}
```

## 15.模拟散列表

进入：训练模式-数据结构-哈希表-840. 模拟散列表

题解页面：[__https://www.acwing.com/problem/content/solution/842/1/__](https://www.acwing.com/problem/content/solution/842/1/)

```cpp
//开放寻址法
#include <cstring>
#include <iostream>
using namespace std;
const int N = 200003, null = 0x3f3f3f3f;
int h[N];
int find(int x)
{
    int t = (x % N + N) % N;
    while (h[t] != null && h[t] != x)
    {
        t ++ ;
        if (t == N) t = 0;
    }
    return t;
}
int main()
{
    memset(h, 0x3f, sizeof h);
    int n;
    scanf("%d", &n);
    while (n -- )
    {
        char op[2];
        int x;
        scanf("%s%d", op, &x);
        if (*op == 'I') h[find(x)] = x;
        else
        {
            if (h[find(x)] == null) puts("No");
            else puts("Yes");
        }
    }
    return 0;
}

//拉链法
#include <cstring>
#include <iostream>
using namespace std;
const int N = 100003;
int h[N], e[N], ne[N], idx;
void insert(int x)
{
    int k = (x % N + N) % N;
    e[idx] = x;
    ne[idx] = h[k];
    h[k] = idx ++ ;
}
bool find(int x)
{
    int k = (x % N + N) % N;
    for (int i = h[k]; i != -1; i = ne[i])
        if (e[i] == x)
            return true;

    return false;
}
int main()
{
    int n;
    scanf("%d", &n);
    memset(h, -1, sizeof h);
    while (n -- )
    {
        char op[2];
        int x;
        scanf("%s%d", op, &x);
        if (*op == 'I') insert(x);
        else
        {
            if (find(x)) puts("Yes");
            else puts("No");
        }
    }
    return 0;
}
```

## 16.字符串哈希

进入：训练模式-数据结构-哈希表-841. 字符串哈希

题解页面：[__https://www.acwing.com/problem/content/solution/843/1/__](https://www.acwing.com/problem/content/solution/843/1/)

```cpp
#include <iostream>
#include <algorithm>
using namespace std;
typedef unsigned long long ULL;
const int N = 100010, P = 131;
int n, m;
char str[N];
ULL h[N], p[N];
ULL get(int l, int r)
{
    return h[r] - h[l - 1] * p[r - l + 1];
}
int main()
{
    scanf("%d%d", &n, &m);
    scanf("%s", str + 1);
    p[0] = 1;
    for (int i = 1; i <= n; i ++ )
    {
        h[i] = h[i - 1] * P + str[i];
        p[i] = p[i - 1] * P;
    }
    while (m -- )
    {
        int l1, r1, l2, r2;
        scanf("%d%d%d%d", &l1, &r1, &l2, &r2);
        if (get(l1, r1) == get(l2, r2)) puts("Yes");
        else puts("No");
    }
    return 0;
}
```
