# 算法基础
## 基础算法
1. 排序
2. 二分
3. 高精度
4. 前缀和与差分
5. 双指针
6. 位运算
7. 离散化
8. 区间合并

## 数据结构
1. 链表与邻接表（数组模拟）
    - 单链表：邻接表，存储图和数；

        ![single_list](img/single_list.png) 
        注意下标从0开始还是从1开始
        ```c++
        #include <iostream>
        
        using namespace std;
        
        const int N = 100010;
        
        // head 表示头结点的下标
        // e[i] 表示结点i的值
        // ne[i] 表示结点i的next指针是多少
        // idx 表示当前已经用到了哪个点
        int head, e[N], ne[N], idx;
        
        // 初始化
        void init()
        {
            head = -1;
            idx = 0;
        }
        
        // 将x插入到头结点
        void add_to_head(int x)
        {
            e[idx] = x;
            ne[idx] = head;
            head = idx;
            idx ++；
        }
        
        // 将x插入到k后
        void add(int k, int x)
        {
            e[idx] = x;
            ne[idx] = ne[k];
            ne[k] = idx;
            idx ++;
        }
        
        // 将k后面一个节点删除
        void remove(int k)
        {
            ne[k] = ne[ne[k]];
        }
        
        ```
    - 双链表：优化问题

        ![double_list](img/double_list.png) 

        ```c++
        #include <iostream>
        
        using namespace std; 
        
        const int N = 100010;
        
        int e[N], l[N], r[N], idx;
        
        void init()
        {
            // 0代表第一个节点，1代表第二个节点
            r[0] = 1;
            l[1] = 0;
            idx = 2;
        }
        
        // 在下标为k的右边插入x
        void add(int k, int x)
        {
            e[idx] = x;
            r[idx] = r[k];
            l[idx] = k;
            l[r[k]] = idx;
            r[k] = idx;
        }
        
        // 删除第k个点
        void remove(int k)
        {
            r[l[k]] = r[k];
            l[r[k]] = l[k];
        }
        
        ```    
2. 栈与队列（数组模拟）
    - 栈 先进后出
        ```c++
        include <iostream>
        
        using namespace std;
        
        const int N = 10010;
        
        int stk[N], tt;
        
        // 插入
        stk[ ++tt] = x;
        
        // 删除
        tt --;
        
        // 判断非空
        if (tt > 0) not empty;
        else empty;
        
        ```
    - 单调栈

        ![monotonic_stack](img/monotonic_stack.png) 

    - 队列 先进先出
        ```c++
        include <iostream>
        
        using namespace std;
        
        const int N = 100010;
        
        // 在队尾插入元素，在队头弹出元素
        int q[N], hh, tt = -1;
        
        // 插入
        q[ ++ tt] = x;
        
        // 弹出队头
        hh ++;
        
        // 判断非空
        if(hh < tt) not empty;
        else empty;
        
        // 取队头
        q[hh];
        
        // 取队尾
        q[tt];
        
        ```
    - 单调队列

        ![monotonic_queue](img/monotonic_queue.png) 

3. KMP

    ![kmp](img/kmp.png) 
    
    ```c++
    #include <iostream>

    using namespace std;

    const int N = 100010;
    const int M = 1000010;

    int n, m;
    char p[N], s[M];
    int ne[N];

    int main()
    {
        cin >> n >> p + 1 >> m >> s + 1;
        
        // 求next 预处理过程
        for(int i = 2, j = 0; i <= n; i ++)
        {
            while (j && p[i] != p[j + 1]) j = ne[j];
            if (p[i] == p[j + 1]) j ++;
            ne[i] = j;
        }
        
        // KMP匹配过程
        for(int i = 1, j = 0; i <= m; i++)
        {
            while(j && s[i] != p[j + 1]) j = ne[j];
            if (s[i] == p[j + 1]) j ++;
            if(j == n)
            {
                // 匹配成功
                printf("%d ", i - n);
                j = ne[j];
            }
        }
        
        return 0;
    }
    ```
