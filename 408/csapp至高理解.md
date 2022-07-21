# bigONE

## 动态链接

1. 什么是静态链接？

   在构建时将所有的全局变量、函数都链接进来---Link everything

   结果是产生一个大的可执行文件---one big happy executable *.out

2. 什么是**动态链接**？

   在构建时保持各种库在不同的文件---keep libraries in different files

   加载时链接全局变量---link global variables at load time

   函数声明时链接对应函数库---link library functions at invocation time

3. 为什么需要动态链接？

   1. 如果程序需要更新，可以只更新一部分（不能改变签名），开发速度提高
   2. 省内存，静态链接会duplicate库
   3. 程序速度变快，因为程序变小，更有可能进入cache中

4. 可执行程序加载进内存

   1. 操作系统拿到这块程序
   2. 操作系统分析program header table后，将section搬运至内存

5. 







