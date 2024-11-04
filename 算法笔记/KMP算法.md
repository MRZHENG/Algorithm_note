## KMP算法

**next[i] =  j 的含义：从字符串首 1 -  j  的部分，和 i - j+1 到 i 这两个字符串相等**

如：![next例子.PNG](https://cdn.acwing.com/media/article/image/2020/06/12/31041_6f934f82ac-next%E4%BE%8B%E5%AD%90.PNG)



#### 匹配与实现 代码

KMP 算法分为 两部 1. 预处理next数组 ，2. 匹配字符串

 s串 和 p串都是从1开始的。i 从1开始，j 从0开始，每次s[ i ] 和p[ j + 1 ]比较

![匹配.PNG](https://cdn.acwing.com/media/article/image/2020/06/12/31041_8e70c3eeac-%E5%8C%B9%E9%85%8D.PNG)

**当 S[a  -  b]  == p [1 - j] 时候， 看  j+1 和 i 位置的字符是否相等， 如果相等 j 和 i 同时向后移动一位，否则调用**

**next[j]  直接跳到从 后缀和前缀匹配的位置**



```c++
for(int i = 1 ,j = 0; i <= n;++i){
    while(j && s[i]!= p[j+1]) j = p[j];  //以下标j结尾，1-next【j】前缀与jjie
    //退出循环就两种可能，一是 j 为空 ，二是 匹配成功 ,
    if(s[i] ==  p[j+1]) j++;
    if(j == m)  //匹配成功
    {  
       //做你该做的事情 
       j = next[j];  //然后直接跳到下一个子串   
    }
}
```



#### 求next数组的思路和实现代码

 next数组的求法是通过模板串自己与自己进行匹配操作得出来的（代码和匹配操作几乎一样）。

![next数组.PNG](https://cdn.acwing.com/media/article/image/2020/06/12/31041_97225cdcac-next%E6%95%B0%E7%BB%84.PNG)

代码如下

```c++
for(int i = 2, j = 0; i <= m; i++)   //next[1] 默认为0
{
    while(j && p[i] != p[j+1]) j = next[j]; // j 从1 到 j 如果存在 那么就跳

    if(p[i] == p[j+1]) j++; //直到找到j+1 与当前匹配
    
    next[i] = j;

}
```

代码和匹配操作的代码几乎一样，关键在于每次移动 i 前，将 i 前面已经匹配的长度记录到next数组中。

### [831. KMP字符串 - AcWing题库](https://www.acwing.com/problem/content/833/)

给定一个字符串 S，以及一个模式串 P，所有字符串中只包含大小写英文字母以及阿拉伯数字。

模式串 P 在字符串 S 中多次作为子串出现。

求出模式串 P 在字符串 S 中所有出现的位置的起始下标。

#### 输入格式

第一行输入整数 N，表示字符串 P 的长度。

第二行输入字符串 P。

第三行输入整数 M，表示字符串 SS的长度。

第四行输入字符串 S。

#### 输出格式

共一行，输出所有出现位置的起始下标（下标从 0 开始计数），整数之间用空格隔开。

#### 数据范围

1≤N≤105≤N≤105
1≤M≤106≤M≤106

#### 输入样例：

```
3
aba
5
ababa
```

#### 输出样例：

```
0 2
```

```c++
#include<iostream>
using namespace std;

#define MAXN 1001000
char s[MAXN];
char p[MAXN];
int ne[MAXN];
int n,m;

//预处理next数组
void build(){
   for(int i = 2,j = 0 ;i<=n; ++i){
       while(j && p[j+1] != p[i]) j = ne[j];
       if(p[j+1] == p[i])
       j++;
       ne[i] = j;
   }
}

int main(){
    cin>>n;
    for(int i = 1;i<=n ; ++i) cin>>p[i];
    cin>>m;
    for(int i = 1;i<=m;++i) cin>>s[i];
    build();
    for(int i = 1,j = 0;i<=m ;++i){
        while(j && p[j+1] != s[i]) j = ne[j];
        if(p[j+1]==s[i]) j++;
        if(j==n){
            cout<< i-n <<" ";
            j = ne[j];
        }
    }
    return 0;
}
```







**如果是下标为 0  的版本**

```c++
next[0] = -1;
for (int i = 1, j = -1; i < len; i ++)
{
    while (j != -1 && s[i] != s[j + 1]) // 前后缀匹配不成功
    {
        // 反复令 j 回退到 -1，或是 s[i] == s[j + 1]
        j = next[j];
    }
    if (s[i] == s[j + 1]) // 匹配成功
    {
        j ++; // 最长相等前后缀变长
    }
    next[i] = j; // 令 next[i] = j
}   
```

```c++
// 省略求 next 数组的步骤
int j = -1; // 表示当前还没有任意一位被匹配
for (int i = 0; i < text_len; i ++)
{
    while (j != -1 && text[i] != pattern[j + 1])
    {
        j = next[j];
    }
    // text[i] 与 pattern[j + 1] 匹配成功，令 j + 1
    if (text[i] == pattern[j + 1])
    {
        j ++;
    }
    if (j == pattern_len - 1) // 是子串，按题目要求处理
}


```

