# 位运算实现N皇后

## [52. N 皇后 II](https://leetcode.cn/problems/n-queens-ii/)

**n 皇后问题** 研究的是如何将 `n` 个皇后放置在 `n × n` 的棋盘上，并且使皇后彼此之间不能相互攻击。

给你一个整数 `n` ，返回 **n 皇后问题** 不同的解决方案的数量。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2020/11/13/queens.jpg)

```
输入：n = 4
输出：2
解释：如上图所示，4 皇后问题存在两个不同的解法。
```

**示例 2：**

```
输入：n = 1
输出：1
```

 **提示：**

- `1 <= n <= 9`





```c++
#include<vector>
#include<string>
//N皇后的位运算实现
class Solution {
public:
   int totalNQueens(int n) {
    //如果棋盘小于1 返回
    if (n < 1) {
        return 0;
    }
    //问运算标志棋盘,全部放满旗的情况
    int limit = (1 << n) - 1;
     f2(limit, 0, 0, 0);
     return ans;
}

void f2(int limit, int col, int left, int right) {
    if (col == limit) {
        ans++;
        return ;
    }
    //不能放的
    int ban = col | left | right;
    //可以放的
    int candidate = limit & (~ban);
    int place = 0;
    while (candidate != 0) {
        place = candidate & (-candidate);
        candidate ^= place;
        f2(limit, col | place, (left | place) << 1, (right | place) >> 1);
    }
}
int ans =0;
};
```

