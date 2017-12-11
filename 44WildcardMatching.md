## 题意

Implement wildcard pattern matching with support for '?' and '\*'.

'?' Matches any single character.
'\*' Matches any sequence of characters (including the empty sequence).

The matching should cover the entire input string (not partial).

The function prototype should be:
bool isMatch(const char \*s, const char \*p)

Some examples:
isMatch("aa","a") → false
isMatch("aa","aa") → true
isMatch("aaa","aa") → false
isMatch("aa", "\*") → true
isMatch("aa", "a\*") → true
isMatch("ab", "?\*") → true
isMatch("aab", "c\*a\*b") → false

## 分析

外卡匹配，\*表示任何长度的任何字符（这样就简便了不少），？表示数量为1的任何字符。这道题使用递归的方式能够很容易的做出来，但是对于某些刁钻的输入样例，递归的方式容易超时。
在这里，我们采用动态规划的方式，这样可以减少很多不必要的重复计算。跟编辑距离那道题有点儿相似。
OPT[i][j]表示a[i]到b[j]能否匹配的上。可以分情况进行计算。
1. 当 a[i] == b[j] || b[j] = '?'时，OPT[i][j] = OPT[i - 1][j - 1];
2. 当 b[j] == '*'时，OPT[i][j] == OPT[i - 1][j] || OPT[i][j - 1];
步骤二非常巧妙，\*的存在非常巧妙，因为其可以匹配任何长度的任何字符，所以碰到\*的时候分成两类（尽量将情况分少一点），0 或 任意，0 的意思是忽略星号，任意的意思是表示匹配上了，并且星号继续存在，这就是2表达式。

同样的，添加边界，与编辑距离那题如出一辙。

## 代码

```
class Solution {
public:
    bool isMatch(string s, string p) {
        int i, j, slen = s.size(), plen = p.size();
        vector<vector<bool>> OPT(slen + 1, vector<bool>(plen + 1));
        OPT[0][0] = true;
        for (i = 1; i <= slen; i++)
            OPT[i][0] = false;
        for (i = 1; i <=plen; i++)
            OPT[0][i] = OPT[0][i - 1] && p[i - 1] == '*';
        
        for (i = 0; i < slen; i++)
            for (j = 0; j < plen; j++)
                OPT[i + 1][j + 1] = (OPT[i][j] && (p[j] == '?' || p[j] == s[i])) || ((OPT[i + 1][j] || OPT[i][j + 1]) && p[j] == '*');
        return OPT[slen][plen];
    }
};
```
