## Best Time to Buy and Sell Stock I

### 题意

Say you have an array for which the ith element is the price of a given stock on day i.

If you were only permitted to complete at most one transaction (ie, buy one and sell one share of the stock), design an algorithm to find the maximum profit.

### 思路

动态规划的题目，进行一次买卖交易，使自己获得的利益最大化，此题目的所使用的方法可以一眼看穿。最优子结构 local[i] 表示在当天卖出股票所获得的最大的收益，整体的最优解可以通过遍历local来得到，并且最优子结构可以通过其自身得到（无需区分全局最优和局部最优），可得最优子结构的状态转移方程为：

**local[i] = max{local[i - 1] + prices[i] - prices[i - 1], 0};**

### 代码

由于最优解存在于local中，为其最大值。而且对于local中的每个值只在其后一个值的计算中用到，因此我们设置一个变量记录其最大值，然后只用一个整形变量来记录local中当前值，可以将空间由O(n)降为O(1)。

```
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        if (prices.size() == 0)
            return 0;
        int i, result, OPT;
        OPT = 0;
        result = OPT;
        for (i = 1; i < prices.size(); i++){
            OPT = max(OPT + prices[i] - prices[i - 1], 0);
            if (OPT > result)
                result = OPT;
        }
        return result;
    }
};
```

## Best Time to Buy and Sell Stock II

### 题意

Say you have an array for which the i<sup>th</sup> element is the price of a given stock on day i.

Design an algorithm to find the maximum profit. You may complete as many transactions as you like (ie, buy one and sell one share of the stock multiple times). However, you may not engage in multiple transactions at the same time (ie, you must sell the stock before you buy again).

### 思路

动态规划：
global[i]表示前i天卖出股票所获得的最大利润。则:
global[i] = global[i - 1] + max(0, prices[i] - prices[i - 1]);
公式的意思是：当第i天的股价比第i - 1天高时，若第i - 1天将股票卖出了，则当前最优值为将前一天出售的股票延迟到今天出售，从而获得更大的收益；若第i - 1天没有将股票出售，则让第i - 1买入股票，今天卖出，然后将获得的收益加到当前最大收益上；当第i天的股票比第i - 1低的时候，则不发生任何交易。

### 代码

```
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        if (prices.size() == 0)
            return 0;
        vector<int> global(prices.size());
        int i;
        global[0] = 0;
        for (i = 1; i < prices.size(); i++){
            global[i] = global[i - 1] + max(prices[i] - prices[i - 1], 0);
        }
        return global[prices.size() - 1];
    }
};
```
更简单的代码：
```
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int result = 0, i;
        for (i = 1; i < prices.size(); i++)
            if (prices[i] > prices[i - 1])
                result += (prices[i] - prices[i - 1]);
        return result;
    }
};
```

## Best Time to Buy and Sell Stock III

### 题意：

Say you have an array for which the i<sup>th</sup> element is the price of a given stock on day i.

Design an algorithm to find the maximum profit. You may complete at most two transactions.

Note:
You may not engage in multiple transactions at the same time (ie, you must sell the stock before you buy again).

### 思路

取一个中间点，将整个数组分为两个部分，分别在两个部分求一次交易的最大值，然后将两个部分的收益相加，即为最大收益。需要注意的是，后半部分的一次交易的遍历需要从后往前，才能实现O(n)的复杂度。
maxProfitbefore[i]代表前i次交易所获得的最大收益，minPrice表示当前位置（包含）之前股票的最低价,则：
maxProfitbefore[i] = max(maxProfitbefor[i - 1], prices[i] - minPrice)
maxProfitafter[i]代表从i开始（包含）到数列结尾进行一次交易所获得的最大收益。maxPrice表示从但前位置（包含）到数组结尾的股票的最高价， 则：
maxProfitafter[i] = max{maxProfitafter[i + 1], maxPrice - prices[i]}

### 代码

```
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        if (prices.size() == 0)
            return 0;
        vector<int> maxProfitbefore(prices.size());
        vector<int> maxProfitafter(maxProfitbefore);
        int i, result = 0, minPrice;
        
        maxProfitbefore[0] = 0;
        minPrice = prices[0];
        for (i = 1; i < prices.size(); i++){
            minPrice = min(minPrice, prices[i]);
            maxProfitbefore[i] = max(maxProfitbefore[i - 1], prices[i] - minPrice);
        }
        
        maxProfitafter[prices.size() - 1] = 0;
        minPrice = prices[prices.size() - 1];
        for( i = prices.size() - 2; i >= 0; i--){
            minPrice = max(minPrice, prices[i]);
            maxProfitafter[i] = max(maxProfitafter[i + 1], minPrice - prices[i]);
        }
        
        for (i = 0; i < prices.size() - 1; i++)
            result = max(result, maxProfitbefore[i] + maxProfitafter[i + 1]);
        result = max(result, maxProfitbefore[prices.size() - 1]);
        return result;
    }
};
```

## Best Time to Buy and Sell Stock IV

### 题意：

Say you have an array for which the i<sup>th</sup> element is the price of a given stock on day i.

Design an algorithm to find the maximum profit. You may complete at most k transactions.

Note:
You may not engage in multiple transactions at the same time (ie, you must sell the stock before you buy again).

### 思路

进行k次交易，获取交易的最大利润。由于引入了新的变量，会考虑最优子结构中包含两个维度。建立两个数组global和local,global[i][j]表示前i天进行j次交易所得到的最大收益，local[i][j]表示在第i天卖出后并且进行了j次交易后所得到的最大收益。则:
global[i][j] = max{global[i - 1][j], local[i][j]}
local[i][j] = max{local[i - 1][j] + prices[i] - prices[i - 1], global[i - 1][j - 1] + max{0, prices[i] - prices[i - 1]}}

第一个式子的意义是，第i天进行了交易的最优值和没有进行交易的最优值进行比较，取较大值作为global[i][j]的值。

第二个式子的意义：观察第i-1天是否进行交易，若进行交易，则第一天的最优值为local[i - 1][j] + prices[i] - prices[i - 1]，若没有进行交易则最优值为global[i - 1][j - 1] + max{0, prices[i] - prices[i - 1]}。这地方不太好理解，直观来看，式子的意思是第i天卖出且进行了j次交易的最大收益为前i - 1天且进行了j - 1次交易的最优值加上max{0, prices[i] - prices[i - 1]}，那有没有这种情况，第j次交易的买入时间不是第i - 1天，而是之前的时间，这种情况的得到的值并不是最优值。其实这种情况会发生，但是这种情况被local[i - 1][j] + prices[i] - prices[i  - 1]包括了。例如若存在这种情况的时候，在i - 1天买入卖出，从local[i - 2][j - 1]得到local[i - 1][j]，然后加上差值。若其值太小，意思就是不用进行交易，则发生迭代。（差不多是这个意思··············，可以跟明哥商量一下再整理一下思路···）

### 代码

```
class Solution {
public:
    int maxProfit(int k, vector<int>& prices) {
        int i, j, result = 0;
        if (prices.size() == 0)
            return 0;
        if (k > prices.size()){
            for (i = 1; i < prices.size(); i++)
                if (prices[i] > prices[i - 1])
                    result += (prices[i] - prices[ i - 1]);
            return result;
        }
        vector<vector<int>> global(prices.size(), vector<int>(k + 1));
        for (i = 0; i < prices.size(); i++)
            global[i][0] = 0;
        for (i = 0; i < k + 1; i++)
            global[0][i] = 0;
        for (j = 1; j < k + 1; j++)
            for (i = 1; i < prices.size(); i++){
                global[i][j] = max(global[i - 1][j], global[i - 1][j  - 1] + max(prices[i] - prices[i - 1], 0));
            }
        return global[prices.size() - 1][k];
    }
};
```