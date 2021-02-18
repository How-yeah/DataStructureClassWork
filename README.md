# 求失效函数

## 要求

模式匹配 KMP 算法中，失效函数 next 的算法思想和实现

15 min

## `kmp` 代码实现

```c++
#include <bits/stdc++.h>
using namespace std;

void GetNext(const string &pat, int *next)
//p[k]表示前缀，p[j]表示后缀
{
    int j = 0, k = -1;
    next[0] = -1; //设next[0]的初始值为-1
    while (pat[j] != '\0')
    {
        if (k == -1 || pat[j] == pat[k])
        {
            j++;
            k++;         //j,k向后走
            next[j] = k; //记录到此索引前字符串真子串的长度
        }
        else
            k = next[k]; //寻求新的匹配字符
    }
}

int KMP(const string &ob, const string &pat, const int start = 0)
{
    int n = pat.length(), m = ob.length();
    int *next = new int[pat.length()];
    GetNext(pat, next);
    int i = start, j = 0;
    while (i < m && j < n && (m - i) >= (n - j))
    {
        if (j == -1 || ob[i] == pat[j])
        {
            i++; //继续对下一个字符比较
            j++; //模式串向右滑动
        }
        else
            j = next[j]; //寻找新的匹配字符位置，模式串尽可能向右滑动
    }
    //delete[] next;
    if (j >= n)
        return (i - j); //匹配成功返回下标
    else
        return -1; //匹配失败返回-1
}

int main()
{
    string a = "aakdgukfoiehoiughbsoichiuwbcledfhuiodsbrffhsdhahjazdfbsetrjhavoiwsbhvcobsaiuadcbiduasfvwsfvsncusbcoiusadgvoisgvbdjbghspjfosbbc";
    string b = "spjf";
    cout << KMP(a, b) << endl;
    system("pause");
}
```

## 思路

最长公共前后缀

`next[j] = k` 的含义：代表  j 位前的子串的最大相同前后缀的长度，也就是j指针的下一步的移动位置

k 指向前缀末尾位置；还是 j 位前的子串的最大相同前后缀的长度

j 指向后缀末尾位置 

甲的前缀=乙的后缀