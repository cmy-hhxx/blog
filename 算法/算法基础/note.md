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
4. Trie 树  
    高效的存储和查找字符串集合的数据结构

    ![trie_tree](img/trie_tree.png) 


5. 并查集  
    - 将两个集合合并
    - 询问两个元素是否在一个集合当中

    基本原理：
    每个集合用一棵树表示，树根的编号就是整个集合的编号。每个节点存储它的父节点，p[x]表示x的父节点
    
    问题1：如何判断树根： if (p[x] == x)  
    问题2：如何求x集合的编号: while (p[x] != x) x = p[x];  
    问题3：如何合并两个集合： px 是x集合的编号， py是y集合的编号， p[x] = y
    
    优化问题2： 路径压缩
    tips: 用字符串读字母
6. 堆
    - 插入一个树: heap[ ++ size] = x;up(size);
    - 求集合当中最小值: heap[1]
    - 删除最小值:  heap[1] = heap[size]; size--;down(1);
    - 删除任意一个元素: heap[k] = heap[size]; size--; down(k); up(k);
    - 修改任意一个元素: heap[k] = x; down(k); up(k);

    ![heap_structure](img/heap_structure.png)

7. 哈希表
    - 存储结构
    - 字符串哈希方式
8. STL使用
    - vector 变长数组 倍增的思想
        - size() 返回元素个数
        - empty() 返回是否为空
        - clear() 清空数组
        - front()/back()
        - push_back()/pop_back()
        - begin()/end()
        - []
        - 支持比较运算
    - pair<int, int> 二元组
        - p.first/p.second
        - 支持比较运算 字典序
    - string 字符串
        - size()/length() 返回字符串长度
        - empty() 返回是否为空
        - clear() 清空
        - substr(开始的位置，长度) 子串
        - c_str() 转换为字符串
    - queue 队列
        - size() 返回元素个数
        - empty() 返回是否为空
        - push() 向队尾插入一个元素
        - pop() 弹出队头
        - front() 返回队头
        - back() 返回队尾
    - priority_queue 优先队列 默认大根堆
        - push() 插入一个元素
        - top() 返回堆顶元素
        - pop() 弹出堆顶元素
        - 定义成小根堆：priority_queue<int, vector<int> greater<int>> q;
    - stack 栈
        - push() 向栈顶压入一个元素
        - pop() 弹出栈顶
        - top() 返回栈顶
    - deque 双端队列
        - size()
        - empty()
        - clear()
        - front()/back()
        - push_back()/pop_back()
        - push_front()/pop_front()
        - begin()/end()
        - []
    - set map multiset multimap 基于平衡二叉树(红黑树) 动态维护有序序列
        - size()
        - empty()
        - clear()
        - begin()/end() ++ --
        - set multiset 
            - insert()
            - find()
            - count()
            - erase()
                - 如果输入的是一个数x，删除所有x
                - 如果输入的是一个迭代器， 删除迭代器
            - lower_bound()/upper_bound()
                - lower_bound() : 返回大于等于x的最小的数的迭代器
                - upper_bound() : 返回大于x的最小的数的迭代器
        - map multimap
            - insert() 插入的数是一个pair
            - erase() 输入的参数是pair或者迭代器
            - find() 
            - [] 时间复杂度O(logn)
            - lower_bound()/upper_bound()
    - unordered_set unordered_map unordered_mutiset unordered_mutimap
        - 与上面类似，增删改查时间复杂度O(1)
        - 不支持lower_bound()/upper_bound(), 迭代器的 ++ --
    - bitset 压位
        - bitset<10000> s;
        - count() 返回有多少个1
        - any() 判断是否至少有一个1
        - none() 判断是否全为0
        - set() 把所有位置1 set(k, v) 把第k位变成v
        - reset() 把所有位置0
        - flip() 等价与~ flip(k) 把第k为取反

## 搜索和图论

1. DFS
2. BFS
3. 图论
