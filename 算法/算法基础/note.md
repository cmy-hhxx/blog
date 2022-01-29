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
        
    - 双链表：优化问题
        ![double_list](img/double_list.png) 
2. 栈与队列（数组模拟）
    - 栈 先进后出
    - 单调栈
        ![monotonic_stack](img/monotonic_stack.png) 

    - 队列 先进先出
    - 单调队列
        ![monotonic_queue](img/monotonic_queue.png) 

3. KMP
    ![kmp](img/kmp.png) 
    
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
    深度优先搜索
2. BFS
    宽度优先搜索
3. 图论
    树与图的深度优先遍历
    树与图的广度优先遍历
    拓扑排序
4. 最短路
    - 单源最短路
        - 边权全为正数
            - 朴素Dijkstra算法
            - 堆优化Dijkstra算法
        - 存在负边权
            - Bellman-Ford
            - SPFA
    - 多元汇最短路
        - Floyd算法

5. 最小生成树
    - Prim算法
        - 朴素版(稠密图)
        - 堆优化版(稀疏图)
    - Kruskal算法(稀疏图)
6. 二分图
    - 染色法
    - 匈牙利算法
    
## 数学知识
1. 数论
    - 质数
        - 定义
        - 判定
        - 分解质因数
        - 朴素筛法 时间复杂度O(nlglgn)
        - 埃氏筛法 时间复杂度O(n)
        - 线性筛法
    - 约数
        - 求一个数所有的约数
        - 约数个数  推公式
        - 约数之和 秦九韶算法
        - 欧几里得算法
    - 欧拉函数
        - 公式
        - 容斥原理证明
        - 欧拉定理
        - 费马定理
    - 快速幂
    - 拓展欧几里得
    - 中国剩余定理
2. 高斯消元
3. 组合数
4. 容斥原理
5. 博弈论


## 动态规划
1. 背包问题
    - 01背包 每件物品最多使用一次
    - 完全背包 每件物品有无限个 注意递推，与01背包相似 
    - 多重背包 
    - 分组背包
    - 
2. 线性DP
    - 数字三角形
    - 最长上升子序列
    - 编辑距离
3. 区间DP
4. 计数类DP
5. 
    
