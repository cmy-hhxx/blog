785 快速排序
```c++
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 1000010;
int a[N];

void quick_sort(int l, int r)
{
    if (l >= r) return ;
    
    int x = a[l + r >> 1], left = l - 1, right = r + 1;
    while (left < right)
    {
        do left ++; while (a[left] < x);
        do right --; while (a[right] > x);
        if (left < right) swap(a[left], a[right]);
    }
    
    quick_sort(l, right);
    quick_sort(right + 1, r);
}

int main()
{
    int n;
    cin >> n;
    for (int i = 0; i < n; i ++) scanf("%d", &a[i]);
    
    quick_sort(0, n - 1);
    for (int i = 0; i < n; i ++) cout << a[i] << ' ';
    
    return 0;
}
```

786 第k个数(快速选择)
```c++
#include <iostream>

using namespace std;

const int N = 100010;
int a[N];

int quick_sort(int l, int r, int k)
{
    if (l == r) return a[l];
    
    int x = a[l + r >> 1], left = l - 1, right = r + 1;
    while (left < right)
    {
        do left ++; while (a[left] < x);
        do right --; while (a[right] > x);
        if (left < right) swap(a[left], a[right]);
    }
    
    if (right - l + 1 >= k) return quick_sort(l, right, k);
    else return quick_sort(right + 1, r, k - (right - l + 1));
}

int main()
{
    int n, k;
    cin >> n >> k;
    for (int i = 0; i < n; i ++) scanf("%d", &a[i]);
    
    cout << quick_sort(0, n - 1, k) << endl;
    
    return 0;
}
```

787 归并排序
```c++
#include <iostream>

using namespace std;

const int N = 1000010;
int q[N], tmp[N];

void merge_sort(int l, int r)
{
    if (l == r) return ;
    int mid = l + r >> 1;
    merge_sort(l, mid);
    merge_sort(mid + 1, r);
    
    int left = l, right = mid + 1, k = 0;
    while (left <= mid && right <= r)
    {
        if (q[left] < q[right]) tmp[k ++] = q[left ++];
        else tmp[k ++] = q[right ++];
    }
    
    while (left <= mid) tmp[k ++] = q[left ++];
    while (right <= r) tmp[k ++] = q[right ++];
    
    for (int i = l, j = 0; i <= r; i ++, j ++) q[i] = tem[j];
}

int main()
{
    int n;
    cin >> n;
    for (int i = 0; i < n; i ++) scanf("%d", &q[i]);
    
    merge_sort(0, n - 1);
    
    for (int i = 0; i < n; i ++) cout << q[i] << ' ';
    
    return 0;
}

```

788 逆序对的数量
```c++
#include <iostream>

using namespace std;

const int N = 1000010;
int q[N], tmp[N];
typedef long long LL;

LL merge_sort(int l, int r)
{
    if (l == r) return 0;
    int mid = l + r >> 1;
    LL ans = merge_sort(l, mid) + merge_sort(mid + 1, r);
    
    int left = l, right = mid + 1, k = 0;
    while (left <= mid && right <= r)
    {
        if (q[left] <= q[right]) tmp[k ++] = q[left ++];
        else
        {
            ans += mid - left + 1;
            tmp[k ++] = q[right ++];
        }
    }
    
    while (left <= mid) tmp[k ++] = q[left ++];
    while (right <= r) tmp[k ++] = q[right ++];
    
    for(int i = l, j = 0; i <= r; i ++, j ++) q[i] = tmp[j];
    return ans;
}

int main()
{
    int n;
    cin >> n;
    for (int i = 0; i < n; i ++) scanf("%d",&q[i]);
    
    cout << merge_sort(0, n - 1) << endl;
    return 0;
}

```

789 数的范围(整数二分，注意边界)
```c++
#include <iostream>

using namespace std;

const int N = 100010;
int a[N];

int main()
{
    int n, m;
    cin >> n >> m;
    for (int i = 0; i < n; i ++) cin >> a[i];
    
    while (m --)
    {
        int x;
        cin >> x;
        
        int l = 0, r = n - 1;
        while (l < r )
        {
            int mid = l + r >> 1;
            if (a[mid] >= x) r = mid;
            else l = mid + 1;
        }
        if (a[l] != x) cout << "-1 -1" << endl;
        else
        {
            cout << l << ' ';
            l = 0, r = n - 1;
            while (l < r)
            {
                int mid = l + r + 1 >> 1;
                if (a[mid] <= x) l = mid;
                else r = mid - 1;
            }
            cout << l << endl;
        }
    }
    return 0;
}
```

790 数的三次方根(实数二分)
```c++
#include <iostream>

using namespace std;

int main()
{
    double n;
    cin >> n;
    double l = -100, r = 100;
    while (r - l > 1e-8)
    {
        double mid = (l + r) / 2;
        if (mid * mid * mid >= n) r = mid;
        else l = mid;
    }
    printf("%.6lf\n", l);
    return 0;
}
```

791 高精度加法
```c++
#include <iostream>
#include <vector>

using namespace std;

vector<int> add(vector<int> &a, vector<int> &b)
{
    if (a.size() < b.size()) return add(b, a);
    vector<int> ans;
    int t = 0;
    for (int i = 0; i < a.size(); i ++)
    {
        t += a[i];
        if (i < b.size()) t += b[i];
        ans.push_back(t % 10);
        t /= 10;
    }
    if (t) ans.push_back(1);
    return ans;
}

int main()
{
    vector<int> A, B;
    string a, b;
    cin >> a >> b;
    
    for (int i = a.size() - 1; i >= 0; i --) A.push_back(a[i] - '0');
    for (int i = b.size() - 1; i >= 0; i --) B.push_back(b[i] - '0');
    
    auto c = add(A, B);
    
    for (int i = c.size() - 1; i >= 0; i --) cout << c[i];
    cout << endl;
    return 0;
}

```

792 高精度减法
```c++
#include <iostream>
#include <vector>

using namespace std;

bool cmp(vector<int> &a, vector<int> &b)
{
    if (a.size() != b.size()) return a.size() > b.size();
    for (int i = a.size() - 1; i >= 0; i --)
        if (a[i] != b[i]) 
            return a[i] > b[i];
    return true;
}

vector<int> sub(vector<int> &a, vector<int> &b)
{
    vector<int> ans;
    int t = 0;
    for (int i = 0; i < a.size(); i ++)
    {
        t = a[i] - t;
        if (i < b.size()) t -= b[i];
        ans.push_back((t + 10 ) % 10);
        if (t < 0) t = 1;
        else t = 0;
    }
    while (ans.size() > 1 && ans.back() == 0) ans.pop_back();
    return ans;
}

int main()
{
    vector<int> A, B;
    string a, b;
    cin >> a >> b;
    
    for (int i = a.size() - 1; i >= 0; i --) A.push_back(a[i] - '0');
    for (int i = b.size() - 1; i >= 0; i --) B.push_back(b[i] - '0');
    
    vector<int> c;
    if (cmp(A, B)) c = sub(A, B);
    else
    {
        cout << '-';
        c = sub(B, A);
    }
    
    for (int i = c.size() - 1; i >= 0; i --) cout << c[i];
    return 0;
}

```

793 高精度乘法
```c++
#include <iostream>
#include <vector>

using namespace std;

vector<int> mul(vector<int> &a, int b)
{
    vector<int> ans;
    int t = 0;
    for (int i = 0; i < a.size() || t; i ++)
    {
        if (i < a.size()) t += a[i] * b;
        ans.push_back(t % 10);
        t /= 10;
    }
    while (ans.size() > 1 && ans.back() == 0) ans.pop_back();
    return ans;
}

int main()
{
    vector<int> A;
    string a;
    int b;
    cin >> a >> b;
    
    for (int i = a.size() - 1; i >= 0; i --) A.push_back(a[i] - '0');
    
    auto c = mul(A, b);
    for (int i = c.size() - 1; i >= 0; i --) cout << c[i];
    
    return 0;
}

```

794 高精度除法
```c++
#include <iostream>
#include <algorithm>
#include <vector>

using namespace std;

vector<int> div(vector<int> &a, int b, int &r)
{
    vector<int> ans;
    r = 0;
    for (int i = a.size() - 1; i >= 0; i --)
    {
        r = r * 10 + a[i];
        ans.push_back(r / b);
        r %= b;
    }
    reverse(ans.begin(), ans.end());
    while (ans.size() > 1 && ans.back() == 0) ans.pop_back();
    return ans;
}

int main()
{
    vector<int> A;
    string a;
    int b;
    cin >> a >> b;
    
    for (int i = a.size() - 1; i >= 0; i --) A.push_back(a[i] - '0');
    
    int r;
    auto c = div(A, b, r);
    for (int i = c.size() -1 ; i >= 0; i --) cout << c[i];
    cout << endl << r << endl;
    
    return 0;
}

```

795 前缀和
```c++
#include <iostream>

using namespace std;

const int N = 100010;
int a[N], s[N];

int main()
{
    int n, m;
    cin >> n >> m;
    
    for (int i = 1; i <= n; i ++)
    {
        cin >> a[i];
        s[i] = s[i - 1] + a[i];
    }
    
    while (m --)
    {
        int l, r;
        cin >> l >> r;
        cout << s[r] - s[l - 1] << endl;
    }
    return 0;
}

```

796 子矩阵的和(二维前缀和)
```c++
#include <iostream>

using namespace std;

const int N = 1010;
int a[N][N], s[N][N];

int main()
{
    int n, m, q;
    cin >> n >> m >> q;
    for (int i = 1; i <= n; i ++ )
        for (int j = 1; j <= m; j ++ )
        {
            cin >> a[i][j];
            s[i][j] = s[i - 1][j] + s[i][j - 1] - s[i - 1][j - 1] + a[i][j];
        }
    while (q --)
    {
        int x1, y1, x2, y2;
        cin >> x1 >> y1 >> x2 >> y2;
        cout << s[x2][y2] - s[x1 - 1][y2] - s[x2][y1 - 1] + s[x1 - 1][y1 - 1] << endl;
    }
    return 0;
}

```

797 差分
```c++
#include <iostream>

using namespace std;

const int N = 100010;
int a[N], b[N];

void insert(int l, int r, int c)
{
    b[l] += c;
    b[r + 1] -= c;
}

int main()
{
    int n, m;
    cin >> n >> m;
    for (int i = 1; i <= n; i ++ )
    {
        cin >> a[i];
        insert(i, i, a[i]);
    }
    
    while (m --)
    {
        int l, r, c;
        cin >> l >> r >> c;
        insert(l, r, c);
    }
    
    for (int i = 1; i <= n; i ++ )
    {
        a[i] = a[i - 1] + b[i];
        cout << a[i] << ' ';
    }
    return 0;
}

```

798 差分矩阵(二维差分)
```c++
#include <iostream>

using namespace std;

const int N = 1010;
int a[N][N], b[N][N];

void insert(int x1, int y1, int x2, int y2, int c)
{
    b[x1][y1] += c;
    b[x1][y2 + 1] -= c;
    b[x2 + 1][y1] -= c;
    b[x2 + 1][y2 + 1] += c;
}

int main()
{
    int n, m, q;
    cin >> n >> m >> q;
    for (int i = 1; i <= n; i ++ )
        for (int j = 1; j <= m; j ++ )
        {
            cin >> a[i][j];
            insert(i, j, i, j, a[i][j]);
        }
    while (q --)
    {
        int x1, y1, x2, y2, c;
        cin >> x1 >> y1 >> x2 >> y2 >> c;
        insert(x1, y1, x2, y2, c);
    }
    
    for (int i = 1; i <= n; i ++)
    {
        for (int j = 1; j <= m; j ++ )
        {
            a[i][j] = a[i - 1][j] + a[i][j - 1] - a[i - 1][j - 1] + b[i][j];
            cout << a[i][j] << ' ';
        }
        cout << endl;
    }
    
    return 0;
}

```
