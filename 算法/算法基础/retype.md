826 单链表
```c++
#include <iostream>

using namespace std;

const int N = 100010;

int head, e[N], ne[N], idx;

void init()
{
    head = -1;
    idx = 0;
}

void add_to_head(int x)
{
    e[idx] = x;
    ne[idx] = head;
    head = idx;
    idx ++;
}

void add(int k, int x)
{
    e[idx] = x;
    ne[idx] = ne[k];
    ne[k] = idx;
    idx ++;
}

void remove(int k)
{
    ne[k] = ne[ne[k]];
}

int main()
{
    int m;
    cin >> m;
    
    init();
    
    while (m --)
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
            cin >> k ;
            if (!k) head = ne[head];
            remove(k - 1);
        }
        else 
        {
            cin >> k >> x;
            add(k - 1, x);
        }
    }
    for(int i = head; i != -1; i = ne[i]) cout << e[i] << ' ';
    cout << endl;
    
    return 0;
}
```

827 双链表
```c++
#include <iostream>

using namespace std;

const int N = 100010;

int e[N], l[N], r[N], idx;

void init()
{
    r[0] = 1;
    l[1] = 0;
    idx = 2;
}

void add(int k, int x)
{
    e[idx] = x;
    l[idx] = k;
    r[idx] = r[k];
    l[r[k]] = idx;
    r[k] = idx;
    idx ++;
}

void remove(int k)
{
    l[r[k]] = l[k];
    r[l[k]] = r[k];
}

int main()
{
    int m;
    cin >> m;
    init();
    
    while (m --)
    {
        string op;
        int k, x;
        
        cin >> op;
        
        if (op == "R")
        {
            cin >> x;
            add(l[1], x);
        }
        else if (op == "L")
        {
            cin >> x;
            add(0, x);
        }
        else if (op == "IL")
        {
            cin >> k >> x;
            add(l[k + 1], x);
        }
        else if (op == "IR")
        {
            cin >> k >> x;
            add(k + 1   , x);
        }
        else
        {
            cin >> k;
            remove(k + 1);
        }
    }
    for(int i = r[0]; i != 1; i = r[i]) cout << e[i] << ' ';
    
    return 0;
}
```

828 模拟栈
```c++
#include <iostream>

using namespace std;

const int N = 100010;

int tt, stk[N];

void push(int x)
{
    stk[ ++ tt] = x;
}

void pop()
{
    tt --;
}

void empty()
{
    if (!tt) cout << "not empty" << endl;
    else cout << "empyt" << endl;
}

void query()
{
    cout << stk[tt] << endl;
}

int main() 
{
    int m;
    cin >> m;
    
    while (m --)
    {
        string op;
        int x;
        cin >> op;
        
        if (op == "push")
        {
            cin >> x;
            push(x);
        }
        else if (op == "pop") pop();
        else if (op == "query") query();
        else empty();
    }
    
    return 0;
}
```

3302 表达式求值
```c++
#include <iostream>
#include <stack>
#include <unordered_map>
#include <cstring>
#include <algorithm>

using namespace std;

stack<int> num;
stack<char> op;

void eval()
{
    int b = num.top(); num.pop();
    int a = num.top(); num.pop();
    char c = op.top(); op.pop();
    
    int x;
    if (c == '+') x = a + b;
    else if (c == '-') x = a - b;
    else if (c == '*') x = a * b;
    else x = a / b;
    num.push(x);
}

int main()
{
    string s;
    cin >> s;
    
    unordered_map<char, int> pr{{'+', 1}, {'-', 1}, {'*', 2}, {'/', 2}};
    s
    for(int i = 0; i < s.size(); i ++)
    {
        if (isdigit(s[i]))
        {
            int j = i, x = 0;
            while(j < s.size() && isdigit(s[j])) x = x * 10 + s[j ++] - '0';
            num.push(x);
            i = j - 1;
        }
        else if (s[i] == '(') op.push(s[i]);
        else if (s[i] == ')')
        {
            while(op.top() != '(') eval();
            op.pop();
        }
        else 
        {
            while (op.size() && pr[op.top()] >= pr[s[i]]) eval();
            op.push(s[i]);
        }
    }
    while(op.size()) eval();
    cout << num.top() << endl;
    
    return 0;
}
```

829 模拟队列
```c++
#include <iostream>

using namespace std;

const int N = 100010;
int q[N], hh, tt = -1;

int main()
{
    int m;
    cin >> m;
    
    while(m --)
    {
        strin op;
        int x;
        
        cin >> op;
        
        if (op == "push")
        {
            cin >> x;
            q[++tt] = x;
        }
        else if (op == "pop") hh ++;
        else if (op == "query") cout << q[hh] << endl;
        else
        {
            if (hh > tt) cout << "YES" << endl;
            else cout << "NO" << endl;
        }
    }
    return 0;
}

```

830 单调栈
```c++
#include <iostream>

using namespace std;

const int N = 1000010;

int stk[N], tt; 

int main()
{
    int n;
    cin >> n;
    while(n --)
    {
        int x;
        scanf("%d", &x);
        while(tt && stk[tt] >= x) tt--;
        if (tt) printf("%d ", stk[tt]);
        else cout << -1 << ' ';
        stk[++ tt] = x;
    }
    return 0;
}

```

154 滑动窗口
```c++
#include <iostream>

using namespace std;

const int N = 1000010;

int q[N], a[N];

int n, k;

int main()
{
    scanf("%d%d", &n, &k);
    for(int i = 0; i < n; i++) scanf("%d", &a[i]);
    
    int hh = 0, tt = -1;
    for(int i = 0; i < n; i ++)
    {
        if (hh <= tt && q[hh] < i - k + 1) hh ++;
        while (hh <= tt && a[q[tt]] >= a[i]) tt --;
        q[++ tt] = i;
        if (i >= k - 1) printf("%d ", a[q[hh]]);
    }
    
    printf("\n");
    hh = 0,tt = -1;
    for (int i = 0; i < n; i ++)
    {
        if (hh <= tt && q[hh] < i - k + 1) hh ++;
        while (hh <= tt && a[q[tt]] <= a[i]) tt --;
        q[++ tt] = i;
        if (i >= k - 1) printf("%d ", a[q[hh]]);
    }
    
    return 0;
}
```

831 KMP字符串匹配
```c++
#include <iostream>

using namespace std;

const int N = 100010;
const int M = 1000010;

char p[N], s[M];
int ne[N];
int n, m;

int main()
{
    cin >> n >> p + 1 >> m >> s + 1;
    
    for(int i = 2, j = 0; i <= n; i ++)
    {
        while(j && p[i] != p[j + 1]) j = ne[j];
        if (p[i] == p[j + 1]) j ++;
        ne[i] = j;
    }
    
    for(int i = 1, j = 0; i <= m; i ++)
    {
        while(j && s[i] != p[j + 1]) j = ne[j];
        if (s[i] == p[j + 1]) j ++;
        if (j == n)
        {
            printf("%d ", i - n);
            j = ne[j];
        }
    }
    return 0;
}
```
835 字符串统计(trie树)
```c++
#include <iostream>

using namespace std;

const int N = 100010;
int son[N][26], cnt[N], idx;
char str[N];

void insert(char *str)
{
    int p = 0;
    for (int i = 0; str[i]; i ++)
    {
        int u = str[i] - 'a';
        if (!son[p][u]) son[p][u] = ++ idx;
        p = son[p][u];
    }
    cnt[p] ++;
}

int query(char *str)
{
    int p = 0;
    for (int i = 0; str[i]; i ++)
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
    cin >> n;
    
    while (n --)
    {
        char op[2];
        scanf("%s%s", op, str);
        
        if (op[0] == 'I') insert(str);
        else printf("%d\n", query(str));
    }
    
    return 0;
}
```

836 合并集合(并查集)
```c++
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
    cin >> n >> m;
    
    for (int i = 1; i <= n; i ++) p[i] = i;
    
    while (m --)
    {
        char op[2];
        int a, b;
        scanf("%s%d%d", op, &a, &b);
        
        if (op[0] == 'M') p[find(a)] = find(b);
        else
        {
            if (find(a) == find(b)) cout << "Yes" << endl;
            else cout << "No" << endl;
        }
    }
    return 0;
}
```

838 堆排序
```c++
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 100010;
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
    int n, m;
    cin >> n >> m;
    
    for (int i = 1; i <= n ; i ++) scanf("%d", &h[i]);
    cnt = n;
    
    for (int i = n / 2; i; i --) down(i);
    
    while (m --)
    {
        printf("%d ", h[1]);
        h[1] = h[cnt --];
        down(1);
    }
    
    return 0;
}

```

143 最大异或数(trie树)
```c++
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 100010;
const int M = 31 * N;
int son[M][2], idx;
int a[N];

void insert(int x)
{
    int p = 0;
    for (int i = 30; i >= 0; i --)
    {
        int u = x >> i & 1;
        if (!son[p][u]) son[p][u] = ++ idx;
        p = son[p][u];
    }
}

int query(int x)
{
    int p = 0, res = 0;
    for (int i = 30; i >= 0; i --)
    {
        int u = x >> i & 1;
        if (son[p][!u])
        {
            p = son[p][!u];
            res = res * 2 + !u;
        }
        else 
        {
            p = son[p][u];
            res = res * 2 + u;
        }
    }
    return res;
}

int main()
{
    int n;
    cin >> n;
    
    for (int i = 0; i < n; i ++) scanf("%d", &a[i]);
    
    int res = 0;
    for (int i = 0; i < n; i ++)
    {
        insert(a[i]);
        int t = query(a[i]);
        res = max (res, t ^ a[i]);
    }
    
    printf("%d\n", res);
    return 0;
}

```

837 连通块中的数量(并查集)
```c++
#include <iostream>

using namespace std;

const int N = 100010;
int p[N], cnt[N];
int n, m;

int find (int x)
{
    if (p[x] != x) p[x] = find(p[x]);
    return p[x];
}

int main()
{
    cin >> n >> m;
    for (int i = 1; i <= n; i ++)
    {
        p[i] = i;
        cnt[i] = 1;
    }
    
    while (m --)
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
                cnt[a] += cnt[b];
            }
        }
        else if (op == "Q1")
        {
            cin >> a >> b;
            if (find(a) == find(b)) cout << "Yes" << endl;
            else cout << "No" << endl;
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

240 食物链(利用并查集维护信息)
```c++
#include <iostream>

using namespace std;

const int N = 100010;
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
    int n, m;
    cin >> n >> m;
    
    for (int i = 1; i <= n; i ++) p[i] = i;
    
    int res = 0;
    while (m --)
    {
        int t, x, y;
        cin >> t >> x >> y;
        
        if (x > n || y > n) res ++;
        else 
        {
            int px = find(x), py = find(y);
            if (t == 1)
            {
                if (px == py && (d[x] - d[y]) % 3) res ++;
                else if (px != py)
                {
                    p[px] = py;
                    d[px] = d[y] - d[x];
                }
            }
            else 
            {
                if (px == py && (d[x] - d[y] - 1) % 3) res ++;
                else if (px != py)
                {
                    p[px] = py;
                    d[px] = d[y] - d[x] + 1;
                }
            }
        }
    }
    cout << res << endl;
    return 0;
}
```

839 模拟堆
```c++
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 100010;
int h[N], hp[N], ph[N], cnt;

void heap_swap(int a, int b)
{
    swap(ph[hp[a]], ph[hp[b]]);
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
    while (u / 2 && h[u / 2] > h[u])
    {
        heap_swap(u / 2, u);
        u /= 2;
    }
}

int main()
{
    int n, m = 0;
    cin >> n;
    while (n --)
    {
        string op;
        int k, x;
        
        cin >> op;
        if (op == "I")
        {
            cin >> x;
            cnt ++, m ++;
            ph[m] = cnt, hp[cnt] = m;
            h[cnt] = x;
            up(cnt);
        }
        else if (op == "PM") cout << h[1] << endl;
        else if (op == "DM")
        {
            heap_swap(1, cnt);
            cnt --;
            down(1);
        }
        else if (op == "D")
        {
            cin >> k;
            k = ph[k];
            heap_swap(k, cnt);
            cnt --;
            down(k);
            up(k);
        }
        else
        {
            cin >> k >> x;
            k = ph[k];
            h[k] = x;
            down(k);
            up(k);
        }
    }
    return 0;
}

```

840 模拟散列表(开放寻址法)
```c++
#include <iostream>
#include <cstring>

using namespace std;

const int N = 200003, null = 0x3f3f3f3f;
int h[N];

int find(int x)
{
    int t = (x % N + N) % N;
    while (h[t] != null && h[t] != x)
    {
        t ++;
        if (t == N) t = 0;
    }
    return t;
}

int main()
{
    memset(h, 0x3f, sizeof h);
    int n;
    cin >> n;
    
    while (n --)
    {
        string op;
        int x;
        cin >> op >> x;
        if (op == "I") h[find(x)] = x;
        else
        {
            if (h[find(x)] == null) cout << "No" << endl;
            else cout << "Yes" << endl;
        }
    }
    return 0;
}

```

840 模拟散列表(拉链法)
```c++
#include <iostream>
#include <cstring>

using namespace std;

const int N = 100003;
int h[N], e[N], ne[N], idx;

void insert(int x)
{
    int k = (x % N + N) % N;
    e[idx] = x;
    ne[idx] = h[k];
    h[k] = idx ++;
}

bool find(int x)
{
    int k = (x % N + N) % N;
    for (int i = h[k]; i != -1; i = ne[i])
        if (e[i] == x) return true;
    return false;
}

int main()
{
    memset(h, -1, sizeof h);
    int n;
    cin >> n;
    while (n --)
    {
        string op;
        int x;
        cin >> op >> x;
        if (op == "I") insert(x);
        else
        {
            if (find(x)) cout << "Yes" << endl;
            else cout << "No" << endl;
        }
    }
}
```
