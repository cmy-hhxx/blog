分享一个整数二分模板，经过真理检验过的，可以放心用。来自yxc算法基础课，[传送门](https://www.acwing.com/blog/content/277/)。

```c
bool check(int x)  // 检查x满足某种性质

// 以右端点的方式更新区间
int binary_search(int left, int right)
{
    while (left < right)
    {
        int mid = left + right >> 1;
        if (check(mid)) right = mid;
        else left = mid + 1;
    }
}

// 以左端点的方式更新区间
int binary_search(int left, int right)
{
    while (left < right)
    {
        int mid = left + right + 1 >> 1;
        if (check(mid)) left = mid;
        else right = mid - 1;
    }
}
```

我是这样记忆的，中点什么时候上取整，取决于区间更新方式，反正都有个加1，如果这个加1在底下以更新的方式出现了```left = mid + 1```，就不用上取整；如果底下反而减1了```right = mid - 1```，那我肯定要补回来的，因此要上取整。

因为二分的端点问题比较复杂，实际上，这份经得起考验的代码经过了好久才得以传承。人们对于二分这种算法比较熟悉，但真正严谨的二分算法代码是很久之后才出现的！因此，不必纠结为何这样，背过就行了。

最后需要注意的点是，这套代码退出循环的时候，**left 一定是等于 right** 的，如果不是这样，请您记得给我发一份代码，我再来修改。

通过一张图，再解释一下，比如现在的问题是在一个**升序**的顺序表中，返回元素x的起始位置和终止位置。

问题分解成:

1. 找到第一个大于等于x的数
2. 找到最后一个小于等于x的数

![image-20220625145752893](https://raw.githubusercontent.com/cmy-hhxx/cloudpic/main/img/image-20220625145752893.png)