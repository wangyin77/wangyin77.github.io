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

![](https://tcs.teambition.net/storage/311z67b4fd6748e607470b876779454a26da?Signature=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJBcHBJRCI6IjU5Mzc3MGZmODM5NjMyMDAyZTAzNThmMSIsIl9hcHBJZCI6IjU5Mzc3MGZmODM5NjMyMDAyZTAzNThmMSIsIl9vcmdhbml6YXRpb25JZCI6IiIsImV4cCI6MTYwNzY3NTA2OSwiaWF0IjoxNjA3MDcwMjY5LCJyZXNvdXJjZSI6Ii9zdG9yYWdlLzMxMXo2N2I0ZmQ2NzQ4ZTYwNzQ3MGI4NzY3Nzk0NTRhMjZkYSJ9.XazcZK_5J8CF0vJYHO1-2Q9wQpjnxTneSpUWqLF6b0E&download=image.png "")

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

![](https://tcs.teambition.net/storage/311z1b3ed9320e278f885a1ebea0817b2f01?Signature=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJBcHBJRCI6IjU5Mzc3MGZmODM5NjMyMDAyZTAzNThmMSIsIl9hcHBJZCI6IjU5Mzc3MGZmODM5NjMyMDAyZTAzNThmMSIsIl9vcmdhbml6YXRpb25JZCI6IiIsImV4cCI6MTYwNzY3NTA2OSwiaWF0IjoxNjA3MDcwMjY5LCJyZXNvdXJjZSI6Ii9zdG9yYWdlLzMxMXoxYjNlZDkzMjBlMjc4Zjg4NWExZWJlYTA4MTdiMmYwMSJ9.Gw33SScIHT-o1Kd_A7NhlJs6e-a1TPmEFdOxmb-Mw0M&download=image.png "")

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

![](https://tcs.teambition.net/storage/311zca66e63aa989301e357a6f9cfbf047be?Signature=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJBcHBJRCI6IjU5Mzc3MGZmODM5NjMyMDAyZTAzNThmMSIsIl9hcHBJZCI6IjU5Mzc3MGZmODM5NjMyMDAyZTAzNThmMSIsIl9vcmdhbml6YXRpb25JZCI6IiIsImV4cCI6MTYwNzY3NTA2OSwiaWF0IjoxNjA3MDcwMjY5LCJyZXNvdXJjZSI6Ii9zdG9yYWdlLzMxMXpjYTY2ZTYzYWE5ODkzMDFlMzU3YTZmOWNmYmYwNDdiZSJ9.j0AG_bddTEVNje6mobwjBnHBTuhHQ-2GB_1-J40-4-0&download=image.png "")

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

题解页面：[__https://www.acwing.com/problem/content/solution/793/1/__](https://www.acwing.com/problem/content/solution/793/1/)

![](https://tcs.teambition.net/storage/311z118ad644708e86faee2df938b361f707?Signature=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJBcHBJRCI6IjU5Mzc3MGZmODM5NjMyMDAyZTAzNThmMSIsIl9hcHBJZCI6IjU5Mzc3MGZmODM5NjMyMDAyZTAzNThmMSIsIl9vcmdhbml6YXRpb25JZCI6IiIsImV4cCI6MTYwNzY3NTA2OSwiaWF0IjoxNjA3MDcwMjY5LCJyZXNvdXJjZSI6Ii9zdG9yYWdlLzMxMXoxMThhZDY0NDcwOGU4NmZhZWUyZGY5MzhiMzYxZjcwNyJ9.kyGoGR77bUdzjdULI2UCPRP9xJyw2MttRZLgdNtPC0Y&download=image.png "")

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

题解页面：[__https://www.acwing.com/problem/content/solution/794/1/__](https://www.acwing.com/problem/content/solution/794/1/)

![](https://tcs.teambition.net/storage/311zc46e06e1386c9dec93a3118f1e1dd84d?Signature=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJBcHBJRCI6IjU5Mzc3MGZmODM5NjMyMDAyZTAzNThmMSIsIl9hcHBJZCI6IjU5Mzc3MGZmODM5NjMyMDAyZTAzNThmMSIsIl9vcmdhbml6YXRpb25JZCI6IiIsImV4cCI6MTYwNzY3NTA2OSwiaWF0IjoxNjA3MDcwMjY5LCJyZXNvdXJjZSI6Ii9zdG9yYWdlLzMxMXpjNDZlMDZlMTM4NmM5ZGVjOTNhMzExOGYxZTFkZDg0ZCJ9.ltIRlWD8tlQSg1mAlacCJV8m7oAW7QTpOGY2cE-eabQ&download=image.png "")

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

![](https://tcs.teambition.net/storage/311z9a9597c8b2331335eb9e94c88d8649fb?Signature=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJBcHBJRCI6IjU5Mzc3MGZmODM5NjMyMDAyZTAzNThmMSIsIl9hcHBJZCI6IjU5Mzc3MGZmODM5NjMyMDAyZTAzNThmMSIsIl9vcmdhbml6YXRpb25JZCI6IiIsImV4cCI6MTYwNzY3NTA2OSwiaWF0IjoxNjA3MDcwMjY5LCJyZXNvdXJjZSI6Ii9zdG9yYWdlLzMxMXo5YTk1OTdjOGIyMzMxMzM1ZWI5ZTk0Yzg4ZDg2NDlmYiJ9.L-5-4PAFcGiLhWggIsyjjcDhles5WCKbgborT_G7cKw&download=image.png "")

图中bug，没有写a[i] **- '0'**

## 9.高精度乘法（普通模式已锁）

进入：训练模式-基础知识-高精度-793.高精度乘法

![](https://tcs.teambition.net/storage/311zbc833ac13b9cc065e34b4fd7faf397bf?Signature=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJBcHBJRCI6IjU5Mzc3MGZmODM5NjMyMDAyZTAzNThmMSIsIl9hcHBJZCI6IjU5Mzc3MGZmODM5NjMyMDAyZTAzNThmMSIsIl9vcmdhbml6YXRpb25JZCI6IiIsImV4cCI6MTYwNzY3NTA2OSwiaWF0IjoxNjA3MDcwMjY5LCJyZXNvdXJjZSI6Ii9zdG9yYWdlLzMxMXpiYzgzM2FjMTNiOWNjMDY1ZTM0YjRmZDdmYWYzOTdiZiJ9.10x1SmdBrD6iuYiMPafsCyg1uGRG4NLJ19IZDvL0lTU&download=image.png "")

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

![](https://tcs.teambition.net/storage/311z1b26bc0c938537cfed5ce4fc900e956f?Signature=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJBcHBJRCI6IjU5Mzc3MGZmODM5NjMyMDAyZTAzNThmMSIsIl9hcHBJZCI6IjU5Mzc3MGZmODM5NjMyMDAyZTAzNThmMSIsIl9vcmdhbml6YXRpb25JZCI6IiIsImV4cCI6MTYwNzY3NTA2OSwiaWF0IjoxNjA3MDcwMjY5LCJyZXNvdXJjZSI6Ii9zdG9yYWdlLzMxMXoxYjI2YmMwYzkzODUzN2NmZWQ1Y2U0ZmM5MDBlOTU2ZiJ9.6cHjQEfp7IstjyaUIw3zkr1sZkh4eC2e_Fj1kR3ukyE&download=image.png "")

## 10.高精度除法（普通模式已锁）

进入：训练模式-基础知识-高精度-794.高精度除法

![](https://tcs.teambition.net/storage/311z1501af272359c83e7e18cd7e82f7dc2e?Signature=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJBcHBJRCI6IjU5Mzc3MGZmODM5NjMyMDAyZTAzNThmMSIsIl9hcHBJZCI6IjU5Mzc3MGZmODM5NjMyMDAyZTAzNThmMSIsIl9vcmdhbml6YXRpb25JZCI6IiIsImV4cCI6MTYwNzY3NTA2OSwiaWF0IjoxNjA3MDcwMjY5LCJyZXNvdXJjZSI6Ii9zdG9yYWdlLzMxMXoxNTAxYWYyNzIzNTljODNlN2UxOGNkN2U4MmY3ZGMyZSJ9.Q9LFyrMzkWeJtkQAedSnDWniVWLojER3CX2Dmcem6v0&download=image.png "")

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

![](https://tcs.teambition.net/storage/311z8c3c7e6aff49fe921b92928ac3f514b8?Signature=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJBcHBJRCI6IjU5Mzc3MGZmODM5NjMyMDAyZTAzNThmMSIsIl9hcHBJZCI6IjU5Mzc3MGZmODM5NjMyMDAyZTAzNThmMSIsIl9vcmdhbml6YXRpb25JZCI6IiIsImV4cCI6MTYwNzY3NTA2OSwiaWF0IjoxNjA3MDcwMjY5LCJyZXNvdXJjZSI6Ii9zdG9yYWdlLzMxMXo4YzNjN2U2YWZmNDlmZTkyMWI5MjkyOGFjM2Y1MTRiOCJ9.AoTKWzm9PrkVYo93YH7-EQNE6kSqDml3D7Daa53tNVc&download=image.png "")

## 11.前缀和（普通模式已锁）

进入：训练模式-基础知识-前缀和与差分-795. 前缀和

![](https://tcs.teambition.net/storage/311z3747dfb465a179f3a7e2a87fa1902764?Signature=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJBcHBJRCI6IjU5Mzc3MGZmODM5NjMyMDAyZTAzNThmMSIsIl9hcHBJZCI6IjU5Mzc3MGZmODM5NjMyMDAyZTAzNThmMSIsIl9vcmdhbml6YXRpb25JZCI6IiIsImV4cCI6MTYwNzY3NTA2OSwiaWF0IjoxNjA3MDcwMjY5LCJyZXNvdXJjZSI6Ii9zdG9yYWdlLzMxMXozNzQ3ZGZiNDY1YTE3OWYzYTdlMmE4N2ZhMTkwMjc2NCJ9.cpbaJVoXobZYldUZkIUIMspfGWtcq5YU2MjZfuykIJQ&download=image.png "")

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

题解页面：[__https://www.acwing.com/problem/content/solution/798/1/__](https://www.acwing.com/problem/content/solution/798/1/)

![](https://tcs.teambition.net/storage/311z0251c15f3c42da87041eee0ef1c77e4b?Signature=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJBcHBJRCI6IjU5Mzc3MGZmODM5NjMyMDAyZTAzNThmMSIsIl9hcHBJZCI6IjU5Mzc3MGZmODM5NjMyMDAyZTAzNThmMSIsIl9vcmdhbml6YXRpb25JZCI6IiIsImV4cCI6MTYwNzY3NTA2OSwiaWF0IjoxNjA3MDcwMjY5LCJyZXNvdXJjZSI6Ii9zdG9yYWdlLzMxMXowMjUxYzE1ZjNjNDJkYTg3MDQxZWVlMGVmMWM3N2U0YiJ9.8G8NBtDquw1ZWTdDJUM9NelDZCVXe67OozrGsps5ykY&download=image.png "")

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

![](https://tcs.teambition.net/storage/311z0d63ec589b1b7822afa16ec2f3493e40?Signature=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJBcHBJRCI6IjU5Mzc3MGZmODM5NjMyMDAyZTAzNThmMSIsIl9hcHBJZCI6IjU5Mzc3MGZmODM5NjMyMDAyZTAzNThmMSIsIl9vcmdhbml6YXRpb25JZCI6IiIsImV4cCI6MTYwNzY3NTA2OSwiaWF0IjoxNjA3MDcwMjY5LCJyZXNvdXJjZSI6Ii9zdG9yYWdlLzMxMXowZDYzZWM1ODliMWI3ODIyYWZhMTZlYzJmMzQ5M2U0MCJ9.8SXwyo6cDL1SFRri2wI6bPVm16NTAoSHksyJFnVVLJc&download=sendpix3.jpg "")

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

![](https://tcs.teambition.net/storage/311z06eee3815cadf8f69b4f1b0c1bf15502?Signature=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJBcHBJRCI6IjU5Mzc3MGZmODM5NjMyMDAyZTAzNThmMSIsIl9hcHBJZCI6IjU5Mzc3MGZmODM5NjMyMDAyZTAzNThmMSIsIl9vcmdhbml6YXRpb25JZCI6IiIsImV4cCI6MTYwNzY3NTA2OSwiaWF0IjoxNjA3MDcwMjY5LCJyZXNvdXJjZSI6Ii9zdG9yYWdlLzMxMXowNmVlZTM4MTVjYWRmOGY2OWI0ZjFiMGMxYmYxNTUwMiJ9.nsEGmVaFKQJPbMlv3FLM5Fi6hXN2zRYQDIepZC8UuEw&download=image.png "")

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

![](https://tcs.teambition.net/storage/311zc93538ef51a0dff7e65cba36939eb396?Signature=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJBcHBJRCI6IjU5Mzc3MGZmODM5NjMyMDAyZTAzNThmMSIsIl9hcHBJZCI6IjU5Mzc3MGZmODM5NjMyMDAyZTAzNThmMSIsIl9vcmdhbml6YXRpb25JZCI6IiIsImV4cCI6MTYwNzY3NTA2OSwiaWF0IjoxNjA3MDcwMjY5LCJyZXNvdXJjZSI6Ii9zdG9yYWdlLzMxMXpjOTM1MzhlZjUxYTBkZmY3ZTY1Y2JhMzY5MzllYjM5NiJ9.DvmnfYKUXI1kfo-GVQl5PX6PpcBRz2rxShnP_70Ue_8&download=image.png "")

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

![](https://tcs.teambition.net/storage/311z4f6bf7415b7bdd935be98f5a6f72a9f1?Signature=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJBcHBJRCI6IjU5Mzc3MGZmODM5NjMyMDAyZTAzNThmMSIsIl9hcHBJZCI6IjU5Mzc3MGZmODM5NjMyMDAyZTAzNThmMSIsIl9vcmdhbml6YXRpb25JZCI6IiIsImV4cCI6MTYwNzY3NTA2OSwiaWF0IjoxNjA3MDcwMjY5LCJyZXNvdXJjZSI6Ii9zdG9yYWdlLzMxMXo0ZjZiZjc0MTViN2JkZDkzNWJlOThmNWE2ZjcyYTlmMSJ9.pW0fV_V56maQntX22sFeyTycZ_qyEuPXyFM5mKdfOt4&download=image.png "")

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

![](https://tcs.teambition.net/storage/311z1d0734b3fd8e29428dfab8bf964becd3?Signature=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJBcHBJRCI6IjU5Mzc3MGZmODM5NjMyMDAyZTAzNThmMSIsIl9hcHBJZCI6IjU5Mzc3MGZmODM5NjMyMDAyZTAzNThmMSIsIl9vcmdhbml6YXRpb25JZCI6IiIsImV4cCI6MTYwNzY3NTA2OSwiaWF0IjoxNjA3MDcwMjY5LCJyZXNvdXJjZSI6Ii9zdG9yYWdlLzMxMXoxZDA3MzRiM2ZkOGUyOTQyOGRmYWI4YmY5NjRiZWNkMyJ9.fIMhFhbqdcfaqZwvTgBefDXf6WXt0Qa_S7Zg1lcqds4&download=image.png "")

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

![](https://tcs.teambition.net/storage/311z932160070451e9be8bb088044fd85f57?Signature=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJBcHBJRCI6IjU5Mzc3MGZmODM5NjMyMDAyZTAzNThmMSIsIl9hcHBJZCI6IjU5Mzc3MGZmODM5NjMyMDAyZTAzNThmMSIsIl9vcmdhbml6YXRpb25JZCI6IiIsImV4cCI6MTYwNzY3NTA2OSwiaWF0IjoxNjA3MDcwMjY5LCJyZXNvdXJjZSI6Ii9zdG9yYWdlLzMxMXo5MzIxNjAwNzA0NTFlOWJlOGJiMDg4MDQ0ZmQ4NWY1NyJ9.bKEChp2rJDIvkXTzdqOrAYK2yl3Td-3jIFSYw4ohMkE&download=image.png "")

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

![](https://tcs.teambition.net/storage/311z8bc1583fd1fcc3b790a4b1f3d65a4e2f?Signature=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJBcHBJRCI6IjU5Mzc3MGZmODM5NjMyMDAyZTAzNThmMSIsIl9hcHBJZCI6IjU5Mzc3MGZmODM5NjMyMDAyZTAzNThmMSIsIl9vcmdhbml6YXRpb25JZCI6IiIsImV4cCI6MTYwNzY3NTA2OSwiaWF0IjoxNjA3MDcwMjY5LCJyZXNvdXJjZSI6Ii9zdG9yYWdlLzMxMXo4YmMxNTgzZmQxZmNjM2I3OTBhNGIxZjNkNjVhNGUyZiJ9.HfOXxcgLg7Yk0MifsBlUl8xdhm9qnZVCLKeHScs13BI&download=image.png "")

注意需要往下翻才能看到！

![](https://tcs.teambition.net/storage/311z613dde982ede9186ccc551df2b1c5fc9?Signature=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJBcHBJRCI6IjU5Mzc3MGZmODM5NjMyMDAyZTAzNThmMSIsIl9hcHBJZCI6IjU5Mzc3MGZmODM5NjMyMDAyZTAzNThmMSIsIl9vcmdhbml6YXRpb25JZCI6IiIsImV4cCI6MTYwNzY3NTA2OSwiaWF0IjoxNjA3MDcwMjY5LCJyZXNvdXJjZSI6Ii9zdG9yYWdlLzMxMXo2MTNkZGU5ODJlZGU5MTg2Y2NjNTUxZGYyYjFjNWZjOSJ9.QLxYJLRhsGuoOz_jUv-BBpMd8cY5uKWXEzrdqaPc7oI&download=image.png "")

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

![](https://tcs.teambition.net/storage/311z4d7613574108d950d96ea72d48fde180?Signature=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJBcHBJRCI6IjU5Mzc3MGZmODM5NjMyMDAyZTAzNThmMSIsIl9hcHBJZCI6IjU5Mzc3MGZmODM5NjMyMDAyZTAzNThmMSIsIl9vcmdhbml6YXRpb25JZCI6IiIsImV4cCI6MTYwNzY3NTA2OSwiaWF0IjoxNjA3MDcwMjY5LCJyZXNvdXJjZSI6Ii9zdG9yYWdlLzMxMXo0ZDc2MTM1NzQxMDhkOTUwZDk2ZWE3MmQ0OGZkZTE4MCJ9.-ZnM39YWLSRJzugAPKf9rMTE3kitn1-TzHC_7Xo_N7Q&download=image.png "")

手速要求比较高,可以让merge返回结果,而不是把结果区间全存下来,只记个数,如上图.