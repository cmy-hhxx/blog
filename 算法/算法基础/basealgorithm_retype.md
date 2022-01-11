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
