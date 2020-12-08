包括排序、二分、高精度、前缀和与差分、双指针算法、位运算、离散化、区间合并等内容。

## 1.[__快速排序__](https://www.acwing.com/problem/content/787/)

```cpp
#include <iostream>
using namespace std;
const int N = 100010;
int q[N];
void quick_sort(int q[], int l, int r)
{
    if (l >= r) return;
    int i = l - 1, j = r + 1, x = q[l + r >> 1];
    while (i < j)
    {
        do i ++ ; while (q[i] < x);
        do j -- ; while (q[j] > x);
        if (i < j) swap(q[i], q[j]);
    }
    quick_sort(q, l, j);
    quick_sort(q, j + 1, r);
}
int main()
{
    int n;
    scanf("%d", &n);
    for (int i = 0; i < n; i ++ ) scanf("%d", &q[i]);
    quick_sort(q, 0, n - 1);
    for (int i = 0; i < n; i ++ ) printf("%d ", q[i]);
    return 0;
}
```

## 2.[__第k个数__](https://www.acwing.com/problem/content/788/)

```cpp
#include <iostream>
using namespace std;
const int N = 100010;
int q[N];
int quick_sort(int q[], int l, int r, int k)
{
    if (l >= r) return q[l];
    int i = l - 1, j = r + 1, x = q[l + r >> 1];
    while (i < j)
    {
        do i ++ ; while (q[i] < x);
        do j -- ; while (q[j] > x);
        if (i < j) swap(q[i], q[j]);
    }
    if (j - l + 1 >= k) return quick_sort(q, l, j, k);
    else return quick_sort(q, j + 1, r, k - (j - l + 1));
}
int main()
{
    int n, k;
    scanf("%d%d", &n, &k);
    for (int i = 0; i < n; i ++ ) scanf("%d", &q[i]);
    cout << quick_sort(q, 0, n - 1, k) << endl;
    return 0;
}
```

## 3.[__归并排序__](https://www.acwing.com/problem/content/789/)

```cpp
#include <iostream>
using namespace std;
const int N = 1e5 + 10;
int a[N], tmp[N];
void merge_sort(int q[], int l, int r)
{
    if (l >= r) return;
    int mid = l + r >> 1;
    merge_sort(q, l, mid), merge_sort(q, mid + 1, r);
    int k = 0, i = l, j = mid + 1;
    while (i <= mid && j <= r)
        if (q[i] <= q[j]) tmp[k ++ ] = q[i ++ ];
        else tmp[k ++ ] = q[j ++ ];
    while (i <= mid) tmp[k ++ ] = q[i ++ ];
    while (j <= r) tmp[k ++ ] = q[j ++ ];
    for (i = l, j = 0; i <= r; i ++, j ++ ) q[i] = tmp[j];
}
int main()
{
    int n;
    scanf("%d", &n);
    for (int i = 0; i < n; i ++ ) scanf("%d", &a[i]);
    merge_sort(a, 0, n - 1);
    for (int i = 0; i < n; i ++ ) printf("%d ", a[i]);
    return 0;
}
```

## 4.逆序对的数量（普通模式已锁）

进入：训练模式-基础知识-归并排序-788.逆序对的数量

题解页面：[__https://www.acwing.com/problem/content/solution/790/1/__](https://www.acwing.com/problem/content/solution/790/1/)

```cpp
#include <iostream>
using namespace std;
typedef long long LL;
const int N = 1e5 + 10;
int a[N], tmp[N];
LL merge_sort(int q[], int l, int r)
{
    if (l >= r) return 0;
    int mid = l + r >> 1;
    LL res = merge_sort(q, l, mid) + merge_sort(q, mid + 1, r);
    int k = 0, i = l, j = mid + 1;
    while (i <= mid && j <= r)
        if (q[i] <= q[j]) tmp[k ++ ] = q[i ++ ];
        else
        {
            res += mid - i + 1;
            tmp[k ++ ] = q[j ++ ];
        }
    while (i <= mid) tmp[k ++ ] = q[i ++ ];
    while (j <= r) tmp[k ++ ] = q[j ++ ];
    for (i = l, j = 0; i <= r; i ++, j ++ ) q[i] = tmp[j];
    return res;
}
int main()
{
    int n;
    scanf("%d", &n);
    for (int i = 0; i < n; i ++ ) scanf("%d", &a[i]);
    cout << merge_sort(a, 0, n - 1) << endl;
    return 0;
}
```

## 5.[__数的范围__](https://www.acwing.com/problem/content/description/791/)

进入：训练模式-基础知识-二分-789.数的范围

```cpp
#include <iostream>
using namespace std;
const int N = 100010;
int n, m;
int q[N];
int main()
{
    scanf("%d%d", &n, &m);
    for (int i = 0; i < n; i ++ ) scanf("%d", &q[i]);
    while (m -- )
    {
        int x;
        scanf("%d", &x);
        int l = 0, r = n - 1;
        while (l < r)
        {
            int mid = l + r >> 1;
            if (q[mid] >= x) r = mid;
            else l = mid + 1;
        }
        if (q[l] != x) cout << "-1 -1" << endl;
        else
        {
            cout << l << ' ';
            int l = 0, r = n - 1;
            while (l < r)
            {
                int mid = l + r + 1 >> 1;
                if (q[mid] <= x) l = mid;
                else r = mid - 1;
            }
            cout << l << endl;
        }
    }
    return 0;
}
```

## 6.[__数的三次方根__](https://www.acwing.com/problem/content/description/792/)

进入：训练模式-基础知识-二分-790.数的三次方根

```cpp
#include<iostream>
using namespace std;
int main(){
    double n;
    cin >> n;
    double l = -100, r = 100;
    while(r - l > 1e-8){
        double mid = (r + l ) / 2;
        if(mid * mid *mid >= n) r = mid;
        else l = mid;
    }
    printf("%.6lf", l);
    return 0;
}
```

## 7.高精度加法（普通模式已锁）

进入：训练模式-基础知识-高精度-791.高精度加法

题解页面：[__https://www.acwing.com/problem/content/solution/793/1/

```cpp
#include <iostream>
#include <vector>
using namespace std;
vector<int> add(vector<int> &A, vector<int> &B)
{
    if (A.size() < B.size()) return add(B, A);
    vector<int> C;
    int t = 0;
    for (int i = 0; i < A.size(); i ++ )
    {
        t += A[i];
        if (i < B.size()) t += B[i];
        C.push_back(t % 10);
        t /= 10;
    }
    if (t) C.push_back(t);
    return C;
}
int main()
{
    string a, b;
    vector<int> A, B;
    cin >> a >> b;
    for (int i = a.size() - 1; i >= 0; i -- ) A.push_back(a[i] - '0');
    for (int i = b.size() - 1; i >= 0; i -- ) B.push_back(b[i] - '0');
    auto C = add(A, B);
    for (int i = C.size() - 1; i >= 0; i -- ) cout << C[i];
    cout << endl;
    return 0;
}
```

注：压9位算法，即每次直接算9位数相加（int_max可以认为是99999999），把t从1位变成9位。效率比较高，没必要掌握：

```cpp
#include <iostream>
#include <vector>
using namespace std;
const int base = 1000000000;
vector<int> add(vector<int> &A, vector<int> &B)
{
    if (A.size() < B.size()) return add(B, A);

    vector<int> C;
    int t = 0;
    for (int i = 0; i < A.size(); i ++ )
    {
        t += A[i];
        if (i < B.size()) t += B[i];
        C.push_back(t % base);
        t /= base;
    }

    if (t) C.push_back(t);
    return C;
}
int main()
{
    string a, b;
    vector<int> A, B;
    cin >> a >> b;
    for (int i = a.size() - 1, s = 0, j = 0, t = 1; i >= 0; i -- )
    {
        s += (a[i] - '0') * t;
        j ++, t *= 10;
        if (j == 9 || i == 0)
        {
            A.push_back(s);
            s = j = 0;
            t = 1;
        }
    }
    for (int i = b.size() - 1, s = 0, j = 0, t = 1; i >= 0; i -- )
    {
        s += (b[i] - '0') * t;
        j ++, t *= 10;
        if (j == 9 || i == 0)
        {
            B.push_back(s);
            s = j = 0;
            t = 1;
        }
    }
    auto C = add(A, B);

    cout << C.back();
    for (int i = C.size() - 2; i >= 0; i -- ) printf("%09d", C[i]);
    cout << endl;
    return 0;
}
```

## 8.高精度减法（普通模式已锁）

进入：训练模式-基础知识-高精度-792.高精度减法

题解页面：[__https://www.acwing.com/problem/content/solution/794/1/

```cpp
#include <iostream>
#include <vector>
using namespace std;
bool cmp(vector<int> &A, vector<int> &B)
{
    if (A.size() != B.size()) return A.size() > B.size();
    for (int i = A.size() - 1; i >= 0; i -- )//注意从高到低
        if (A[i] != B[i])
            return A[i] > B[i];
    return true;
}
vector<int> sub(vector<int> &A, vector<int> &B)
{
    vector<int> C;
    for (int i = 0, t = 0; i < A.size(); i ++ )
    {
        t = A[i] - t;
        if (i < B.size()) t -= B[i];
        C.push_back((t + 10) % 10);
        if (t < 0) t = 1;
        else t = 0;
    }
    while (C.size() > 1 && C.back() == 0) C.pop_back();
    return C;
}
int main()
{
    string a, b;
    vector<int> A, B;
    cin >> a >> b;
    for (int i = a.size() - 1; i >= 0; i -- ) A.push_back(a[i] - '0');
    for (int i = b.size() - 1; i >= 0; i -- ) B.push_back(b[i] - '0');
    vector<int> C;
    if (cmp(A, B)) C = sub(A, B);
    else C = sub(B, A), cout << '-';
    for (int i = C.size() - 1; i >= 0; i -- ) cout << C[i];
    cout << endl;
    return 0;
}
```

图中bug，没有写a[i] **- '0'**

## 9.高精度乘法（普通模式已锁）

进入：训练模式-基础知识-高精度-793.高精度乘法

题解页面：[__https://www.acwing.com/problem/content/solution/795/1/__](https://www.acwing.com/problem/content/solution/795/1/)

```cpp
#include <iostream>
#include <vector>
using namespace std;
vector<int> mul(vector<int> &A, int b)
{
    vector<int> C;
    int t = 0;
    for (int i = 0; i < A.size() || t; i ++ )
    {
        if (i < A.size()) t += A[i] * b;
        C.push_back(t % 10);
        t /= 10;
    }
    while (C.size() > 1 && C.back() == 0) C.pop_back();
	//这行可以不用判断，直接如下图中，在输入b时候特判是否为0也可以
    return C;
}
int main()
{
    string a;
    int b;
    cin >> a >> b;
    vector<int> A;
    for (int i = a.size() - 1; i >= 0; i -- ) A.push_back(a[i] - '0');
    auto C = mul(A, b);
    for (int i = C.size() - 1; i >= 0; i -- ) printf("%d", C[i]);
    return 0;
}
```

## 10.高精度除法（普通模式已锁）

进入：训练模式-基础知识-高精度-794.高精度除法

题解页面：[__https://www.acwing.com/problem/content/solution/796/1/__](https://www.acwing.com/problem/content/solution/796/1/)

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;
vector<int> div(vector<int> &A, int b, int &r)
{
    vector<int> C;
    r = 0;
    for (int i = A.size() - 1; i >= 0; i -- )
    {
        r = r * 10 + A[i];
        C.push_back(r / b);
        r %= b;
    }
    reverse(C.begin(), C.end());
    while (C.size() > 1 && C.back() == 0) C.pop_back();
    return C;
}
int main()
{
    string a;
    vector<int> A;
    int B;
    cin >> a >> B;
    for (int i = a.size() - 1; i >= 0; i -- ) A.push_back(a[i] - '0');
    int r;
    auto C = div(A, B, r);
    for (int i = C.size() - 1; i >= 0; i -- ) cout << C[i];
    cout << endl << r << endl;
    return 0;
}
```

## 11.前缀和（普通模式已锁）

进入：训练模式-基础知识-前缀和与差分-795. 前缀和

题解页面：[__https://www.acwing.com/problem/content/solution/797/1/__](https://www.acwing.com/problem/content/solution/797/1/)

```cpp
#include <iostream>
using namespace std;
const int N = 100010;
int n, m;
int a[N], s[N];
int main()
{
    scanf("%d%d", &n, &m);
    for (int i = 1; i <= n; i ++ ) scanf("%d", &a[i]);
    for (int i = 1; i <= n; i ++ ) s[i] = s[i - 1] + a[i]; // 前缀和初始化
    while (m -- )
    {
        int l, r;
        scanf("%d%d", &l, &r);
        printf("%d\n", s[r] - s[l - 1]); // 区间和的计算
    }
    return 0;
}
```

全局数组若不初始化，编译器将其初始化为零。局部数组若不初始化，内容为随机值！！

## 12.子矩阵的和（普通模式已锁）

进入：训练模式-基础知识-前缀和与差分-796. 子矩阵的和

题解页面：[__https://www.acwing.com/problem/content/solution/798/1/

```cpp
#include <iostream>
using namespace std;
const int N = 1010;
int n, m, q;
int s[N][N];
int main()
{
    scanf("%d%d%d", &n, &m, &q);
    for (int i = 1; i <= n; i ++ )
        for (int j = 1; j <= m; j ++ )
            scanf("%d", &s[i][j]);
    for (int i = 1; i <= n; i ++ )
        for (int j = 1; j <= m; j ++ )
            s[i][j] += s[i - 1][j] + s[i][j - 1] - s[i - 1][j - 1];
    while (q -- )
    {
        int x1, y1, x2, y2;
        scanf("%d%d%d%d", &x1, &y1, &x2, &y2);
        printf("%d\n", s[x2][y2] - s[x1 - 1][y2] - s[x2][y1 - 1] + s[x1 - 1][y1 - 1]);
    }
    return 0;
}
```

## 13.[__差分__](https://www.acwing.com/problem/content/799/)

```cpp
#include <iostream>
using namespace std;
const int N = 100010;
int n, m;
int a[N], b[N];
void insert(int l, int r, int c)
{
    b[l] += c;
    b[r + 1] -= c;
}
int main()
{
    scanf("%d%d", &n, &m);
    for (int i = 1; i <= n; i ++ ) scanf("%d", &a[i]);
    for (int i = 1; i <= n; i ++ ) insert(i, i, a[i]);
    while (m -- )
    {
        int l, r, c;
        scanf("%d%d%d", &l, &r, &c);
        insert(l, r, c);
    }
    for (int i = 1; i <= n; i ++ ) b[i] += b[i - 1];
    for (int i = 1; i <= n; i ++ ) printf("%d ", b[i]);
    return 0;
}
```

```cpp
//稍微简化一下
#include<iostream>
using namespace std;
const int N = 1e5 + 10;
int a[N];
void insert(int l, int r, int c) {a[l] += c, a[r + 1] -= c;}
int main(){
    int n, m, tmp;
    cin >>n >>m;
    for(int i = 1; i <= n; i++)
        cin >> tmp, insert(i, i, tmp);
    while(m--){
        int l, r, c;
        cin >> l >> r >> c;
        insert(l, r, c);
    }
    for(int i = 1; i <= n; i++){
        a[i] += a[i-1], cout << a[i] << " ";
    }
}
```

## 14.差分矩阵（普通模式已锁）

进入：训练模式-基础知识-前缀和与差分-798. 差分矩阵

题解界面：[__https://www.acwing.com/problem/content/solution/800/1/__](https://www.acwing.com/problem/content/solution/800/1/)

```cpp
#include <iostream>
using namespace std;
const int N = 1010;
int n, m, q;
int a[N][N], b[N][N];
void insert(int x1, int y1, int x2, int y2, int c)
{
    b[x1][y1] += c;
    b[x2 + 1][y1] -= c;
    b[x1][y2 + 1] -= c;
    b[x2 + 1][y2 + 1] += c;
}
int main()
{
    scanf("%d%d%d", &n, &m, &q);
    for (int i = 1; i <= n; i ++ )
        for (int j = 1; j <= m; j ++ )
            scanf("%d", &a[i][j]);
    for (int i = 1; i <= n; i ++ )
        for (int j = 1; j <= m; j ++ )
            insert(i, j, i, j, a[i][j]);
    while (q -- )
    {
        int x1, y1, x2, y2, c;
        cin >> x1 >> y1 >> x2 >> y2 >> c;
        insert(x1, y1, x2, y2, c);
    }
    for (int i = 1; i <= n; i ++ )
        for (int j = 1; j <= m; j ++ )
            b[i][j] += b[i - 1][j] + b[i][j - 1] - b[i - 1][j - 1];
    for (int i = 1; i <= n; i ++ )
    {
        for (int j = 1; j <= m; j ++ ) printf("%d ", b[i][j]);
        puts("");
    }
    return 0;
}
```

前两个和后两个循环其实可以合在一起写，能节省点代码量

## 15.最长连续不重复子序列（普通模式已锁）

进入：训练模式-基础知识-双指针算法-799. 最长连续不重复子序列

题解页面：[__https://www.acwing.com/problem/content/solution/801/1/__](https://www.acwing.com/problem/content/solution/801/1/)

```cpp
#include <iostream>
using namespace std;
const int N = 100010;
int n;
int q[N], s[N];
int main()
{
    scanf("%d", &n);
    for (int i = 0; i < n; i ++ ) scanf("%d", &q[i]);
    int res = 0;
    for (int i = 0, j = 0; i < n; i ++ )
    {
        s[q[i]] ++ ;
        while (j < i && s[q[i]] > 1) s[q[j ++ ]] -- ;
        res = max(res, i - j + 1);
    }
    cout << res << endl;
    return 0;
}
```

## 16.数组元素的目标和（普通模式已锁）

进入：训练模式-基础知识-双指针算法-800. 数组元素的目标和

题解页面：[__https://www.acwing.com/problem/content/solution/802/1/__](https://www.acwing.com/problem/content/solution/802/1/)

```cpp
#include <iostream>
using namespace std;
const int N = 1e5 + 10;
int n, m, x;
int a[N], b[N];
int main()
{
    scanf("%d%d%d", &n, &m, &x);
    for (int i = 0; i < n; i ++ ) scanf("%d", &a[i]);
    for (int i = 0; i < m; i ++ ) scanf("%d", &b[i]);
    for (int i = 0, j = m - 1; i < n; i ++ )
    {
        while (j >= 0 && a[i] + b[j] > x) j -- ;
        if (j >= 0 && a[i] + b[j] == x) cout << i << ' ' << j << endl;
    }
    return 0;
}
```

## 17.二进制中1的个数（普通模式已锁）

进入：训练模式-基础知识-位运算-801. 二进制中1的个数

题解页面：[__https://www.acwing.com/problem/content/solution/803/1/__](https://www.acwing.com/problem/content/solution/803/1/)

```cpp
#include <iostream>
using namespace std;
int main()
{
    int n;
    scanf("%d", &n);
    while (n -- )
    {
        int x, s = 0;
        scanf("%d", &x);
        for (int i = x; i; i -= i & -i) s ++ ;
        printf("%d ", s);
    }
    return 0;
}
```

其实不用cincout前两行也不用写，，

## 18.区间和（普通模式已锁）

进入：训练模式-基础知识-离散化-802. 离散化

[__https://www.acwing.com/problem/content/solution/804/1/__](https://www.acwing.com/problem/content/solution/804/1/)

```cpp
#include <iostream>
#include <vector>
#include <algorithm>//sort()函数需要
using namespace std;
typedef pair<int, int> PII;
const int N = 300010;//x,l,r各100000
int n, m;
int a[N], s[N];
vector<int> alls;
vector<PII> add, query;
int find(int x)
{
    int l = 0, r = alls.size() - 1;
    while (l < r)
    {
        int mid = l + r >> 1;
        if (alls[mid] >= x) r = mid;
        else l = mid + 1;
    }
    return r + 1;//因为前缀和数组要从1开始
}
vector<int>::iterator unique(vector<int> &a)
{
    int j = 0;
    for (int i = 0; i < a.size(); i ++ )
        if (!i || a[i] != a[i - 1])
            a[j ++ ] = a[i];
    // a[0] ~ a[j - 1] 所有a中不重复的数
    return a.begin() + j;
}//不需要手写
int main()
{
    cin >> n >> m;
    for (int i = 0; i < n; i ++ )
    {
        int x, c;
        cin >> x >> c;
        add.push_back({x, c});

        alls.push_back(x);
    }
    for (int i = 0; i < m; i ++ )
    {
        int l, r;
        cin >> l >> r;
        query.push_back({l, r});

        alls.push_back(l);
        alls.push_back(r);
    }
    // 去重
    sort(alls.begin(), alls.end());
    alls.erase(unique(alls), alls.end());
	//调用库函数
	//alls.erase(unique(alls.begin(), alls.end()), alls.end());
    // 处理插入
    for (auto item : add)
    {
        int x = find(item.first);
        a[x] += item.second;
    }
    // 预处理前缀和
    for (int i = 1; i <= alls.size(); i ++ ) s[i] = s[i - 1] + a[i];
    // 处理询问
    for (auto item : query)
    {
        int l = find(item.first), r = find(item.second);
        cout << s[r] - s[l - 1] << endl;
    }
    return 0;
}
```

## 19.区间合并（普通模式已锁）

进入：训练模式-基础知识-区间合并-803. 区间合并

注意需要往下翻才能看到！

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;
typedef pair<int, int> PII;
void merge(vector<PII> &segs)
{
    vector<PII> res;
    sort(segs.begin(), segs.end());
    int st = -2e9, ed = -2e9;
    for (auto seg : segs)
        if (ed < seg.first)
        {
            if (st != -2e9) res.push_back({st, ed});
            st = seg.first, ed = seg.second;
        }
        else ed = max(ed, seg.second);
    if (st != -2e9) res.push_back({st, ed});
    segs = res;
}
int main()
{
    int n;
    scanf("%d", &n);
    vector<PII> segs;
    for (int i = 0; i < n; i ++ )
    {
        int l, r;
        scanf("%d%d", &l, &r);
        segs.push_back({l, r});
    }
    merge(segs);
    cout << segs.size() << endl;
    return 0;
}
```

手速要求比较高,可以让merge返回结果,而不是把结果区间全存下来,只记个数,如上图.