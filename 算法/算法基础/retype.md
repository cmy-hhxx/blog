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
