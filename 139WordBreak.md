## 题目

Given a non-empty string s and a dictionary wordDict containing a list of non-empty words, determine if s can be segmented into a space-separated sequence of one or more dictionary words. You may assume the dictionary does not contain duplicate words.

For example, given
s = "leetcode",
dict = ["leet", "code"].

Return true because "leetcode" can be segmented as "leet code".

## 分析

给定一个字典和一个单词，看看单词是否能由字典中的字组合而成，其中使用次数不限。
动态规划：OPT[i][j]--表示从i到j这一段字母是否可以用字典中的字组合而成。则
OPT[i][j] = OPT[i][k] && OPT[k + 1][j] (i <= k < j);
注意边界。
## 代码

```
class Solution {
public:
    bool wordBreak(string s, vector<string>& wordDict) {
        int i, j, l;
        vector<vector<bool>> OPT(s.size(), vector<bool>(s.size()));
        for(i = 0; i < s.size(); i++)
            OPT[i][i] = inDict(s.substr(i, 1), wordDict);
        for(l = 2; l <= s.size(); l++){
            for (i = 0; i <= s.size() - l; i++){
                if (inDict(s.substr(i, l), wordDict)){
                    OPT[i][i + l - 1] = true;
                    continue;
                }
                OPT[i][i + l - 1] =false;
                for (j = 0; j < i + l - 1; j++)
                    if (OPT[i][j] && OPT[j + 1][i + l - 1]){
                        OPT[i][i + l - 1] = true;
                        break;
                    }
            }
        }
        return OPT[0][s.size() - 1];
    }
    
    bool inDict(string s, vector<string>& wordDict){
        vector<string>::iterator ret;
        ret = find(wordDict.begin(), wordDict.end(), s);
        if (ret == wordDict.end())
            return false;
        else
            return true;
    }
    
};
```