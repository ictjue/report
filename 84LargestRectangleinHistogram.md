## 题目

Given n non-negative integers representing the histogram's bar height where the width of each bar is 1, find the area of largest rectangle in the histogram.

## 分析

使用堆栈。思想主要是：若第i+1个柱子比第i个柱子高的话，则整个全局的最优值不可能是以第i个柱子结尾的，我们将其存起来，并不进行计算；若第i+1个柱子比第i柱子矮，则第i个柱子可能使最优值。在堆栈中，我们保证出栈的顺序为从大到小。 整个计算过程为：
1. 当栈为空的时候或者当前柱子比前一个柱子高的时候，当当前柱子的索引值入栈。
2. 当当前柱子比前一个柱子矮的时候，按照出栈顺序，先出栈，然后以出栈索引所对应的直方图的高度作为高度，栈顶(现在)到当前柱子（不包含）之间的宽度作为宽度，计算面积。直到栈顶元素对应的柱子的高度小于当前柱子的高度。（此处需要细细斟酌，因为出栈元素对应的柱子到当前柱子之间可以以出栈的柱子作为高度来进行计算，而且此时宽度不太好理解。选取的区间右边好理解，但是左边为什么选取出栈以后栈顶的元素所对应的柱子作为起始位置呢？因为在栈中的索引不是连续的，若以出栈的元素作为起始位置，则从栈顶元素到出栈元素之间的宽度被忽略了。此时你可能会想，万一中间被忽略的宽度中柱子比出栈元素所对应的柱子低怎么办？不可能！若其高度比当前出栈元素所对应的低，那么他肯定会在栈中。）当栈为空时，则说明刚刚弹出的是到目前为止全局最矮的柱子，计算是直接以从位置0到当前柱子之间的宽度作为宽度来进行计算。(我在说什么几把？)。
3.遍历得到最大值。

## 代码

```
class Solution {
public:
    int largestRectangleArea(vector<int>& heights) {
        int result = 0, p = 1, position, l;
        stack<int> s;
        heights.push_back(-1);
        l = heights.size();
        if (l == 1)
            return 0;
        s.push(0);
        while (p < l){
            if (s.empty() || heights[p] >= heights[s.top()]){
                s.push(p++);
                continue;
            }
            
            while (s.size() > 0 && heights[s.top()] > heights[p]){
                position = s.top();
                s.pop();
                result = max(result, s.size() == 0 ? p * heights[position] : (p - s.top() - 1) * heights[position]);
            }
            s.push(p++);
        }
        return result;
    }
};
```