# 背包问题

## 01 背包

> 给你一个体积为m的背包，从n个物品中选择一些物品，使得背包中物品价值最大且物品总体积不超过背包容量
>
> 1. 写暴力dfs， 可惜只能过很少的数据，拿少得可怜的分数
>
> 2. 动态规划
>
>     1. 状态表示  ```f[i][j]```
>
>         1. 可以将题目的意思转化为哪个集合？   从前i个物品中选择一些物品，总体积不超过j的所有选择方案构成的集合
>         2. 集合的属性？保存的是物品价值总和的最大值
>         3. 答案？ ```f[n][m]```, 顺便想一下初始条件，```f[0][0]```的值， 这里是0， 如果是方案数的话，```f[0][0]```是1
>
>     2. 集合划分  观察```f[i][j]```由可以从哪些状态转移过来
>
>         对于第i个物品， 我有两种选择， 选或者不选
>
>         1. 不选第i个物品， 也就是说前i个物品中不选i，等价于从前i - 1个物品中选，体积不超过j----> ```f[i - 1][j]```
>         2. 选第i个物品， 也就是说第i个物品必选的所有集合中，```f[i][j]```应该如何表示呢 ？答：```f[i - 1][j - v[i]] + w[i]```
>
>         由于我是选最大值，所以```f[i][j] = max(f[i - 1][j], f[i - 1][j - v[i]] + w[i]) ```
>
> 一般来说，我们不用考虑代码的空间复杂度，因此二维数组用来理解这01背包是最好的
>
> 但是由于我们保存i的状态对于答案来说并没有什么特别大的意义，因此可以优化代码，压缩到一维
>
> 然而在压缩至一维的过程中， 枚举的顺序需要发生改变
>
> 具体来说，若用到上一层的状态时,从大到小枚举, 反之从小到大！

```c++
#include <iostream>

using namespace std;

const int N = 1010;
int f[N][N];
int v[N], w[N];
int n, m;

int main()
{
    cin >> n >> m;
    
    for (int i = 1; i <= n; i ++ ) scanf("%d%d", &v[i], &w[i]);
    
    for (int i = 1; i <= n; i ++ )
    {
        for (int j = 1; j <= m; j ++ )
        {
            f[i][j] = f[i - 1][j];
            if (j >= v[i]) f[i][j] = max(f[i][j], f[i - 1][j - v[i]] + w[i]);
        }
    }
    cout << f[n][m] << endl;
    
    return 0;
}
```

## [质数拆分](https://www.lanqiao.cn/problems/809/learning/)

```c++
#include <iostream>
#include <algorithm>
#include <cstring>
#include <cstdio>
#include <vector>
#include <unordered_map>
#include <climits>
#include <queue>

using namespace std;

typedef long long LL;

typedef pair<int, int> PII;

const int N = 5000;
LL f[N][N];
int primes[N], cnt = 1;
bool st[N];

void get_primes(int n)
{
    for (int i = 2; i <= n; i ++ )
    {
        if (!st[i]) primes[cnt ++] = i;
        for (int j = 1; primes[j] <= n / i; j ++)
        {
            st[primes[j] * i] = true;
            if (i % primes[j] == 0) break;
        }
    }
}

int main()
{
    int n;

    //cin >> n;
    get_primes(2019);


    f[0][0] = 1;

    for (int i = 1; i < cnt; i ++ )
    {
        for (int j = 0; j <= 2019;j ++ )
        {
            f[i][j] = f[i - 1][j];
            if (j >= primes[i]) f[i][j] += f[i - 1][j - primes[i]];
        }
    }

    cout << f[cnt - 1][2019] << endl;

    return 0;
}
```

