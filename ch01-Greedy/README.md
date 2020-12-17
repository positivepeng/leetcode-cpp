# 贪心算法系列

题目列表：
|ID|题目|备注(官方归类的topic)|链接|
|----|----|----|----|
|55|Jump Game|贪心、数组|[链接](https://leetcode.com/problems/jump-game/)|
|45|Jump Game II|贪心、数组|[链接](https://leetcode.com/problems/jump-game-ii/)|
|122|Best Time to Buy and Sell Stock II|贪心、数组|[链接](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/)|
|134|Gas Station|贪心|[链接](https://leetcode.com/problems/gas-station/)|

## 代码与题解
#### 55. Jump Game 
```cpp
class Solution {
public:
    bool canJump(vector<int>& nums) {
        /*
            思路：基于贪心的策略，每一步记录当前能够到达的最远的位置
            时间复杂度：O(n)
            空间复杂度：O(1)
        */
        int n = nums.size();
        int max_index = 0;
        for(int i = 0;i < n; i++){
            if(i <= max_index)
                max_index = max(max_index, i+nums[i]);
            else
                return false;
        }
        return max_index >= n-1;
    }
};
```

#### 45. Jump Game II 
```cpp
class Solution {
public:
    int jump(vector<int>& nums) {
        /*
            思路：本题第一感觉是要动态规划，使用dp[i]表示到达位置i需要的最少步数，这样时间复杂度为O(n^2),因为最坏情况下每一步都要遍历后面的所有位置的值(例如case：[5,5,5,5,5])，空间复杂度为O(n)，但是使用贪心算法则可优化。
            时间复杂度:O(n)
            空间复杂度:O(1)
        */
        int n = nums.size();
        int jump = 0;         // 跳的次数
        int max_index = 0;    // 当前能跳到的最远的距离
        int jump_max_index = 0;     // 下一跳能够到达的最远的地方
        
        for(int i = 0;i < n; i++){
            if(max_index >= n-1) // 注意边界条件，case: [0]
                return jump;
            if(i <= max_index)   // 还在可跳范围内，更新下一步能到达的最远位置
                jump_max_index = max(nums[i]+i, jump_max_index);
            if(i == max_index){  // 到达边界，跳！
                max_index = max(max_index, jump_max_index);
                jump += 1;
            }
        }
        return jump;
    }
};
```
#### 122. Best Time to Buy and Sell Stock II 
```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        /*
            思路：假设在第i个时间节点买入，在什么时候卖出收益是最大呢？肯定不是遇到比price[i]高的价格就卖，想要收益最大化就要继续往后看，假设后面价格是一直升高的（case：1 4 5 6 5 8），则在价格最高点（6）卖，需要证明的是：在1处买6处卖，再在5处买8处卖的收益是比1处买8处卖的收益更高。
            时间复杂度：O(n)
            空间复杂度：O(1)
        */
        // int n = prices.size();
        // int profit = 0,i = 0;
        // while(i < n){
        //     p0 = prices[i];
        //     while(i+1 < n && prices[i+1] >= prices[i])
        //         i += 1;
        //     if(prices[i] > p0)
        //         profit += prices[i] - p0;
        //     else
        //         i += 1;
        // }
        // return profit;
        
        /*最优：
            思路：将问题转换为不相交区间的最大和问题, 将a[j]-a[i] 转换为 a[j]-a[j-1]+a[j-1]-a[j-2]....a[i+1]-a[i]，则问题转换尽可能找长度为1的区间使得它们的差值和最大，遍历一次则保证没有相交
            时间复杂度：O(n)
            空间复杂度：O(1)
        */
        int n = prices.size();
        int ans = 0;
        for(int i = 1;i < n; i++)
            ans += max(0, prices[i]-prices[i-1]);
        return ans;
    }
};
```
#### 134. Gas Station
```cpp
class Solution {
public:
    int canCompleteCircuit(vector<int>& gas, vector<int>& cost) {
        /*
            思路：暴力求解，枚举所有可能的起点，然后模拟，优化思路：找起点，如果i是起点，则从i到n-1积累的油量肯定相比去其他点更多，所以有解则起点必定是i，如果到0（已经走了一圈）积累的油量为负值则无解。
            时间复杂度：O(n)
            空间复杂度：O(1)
        */
        int ans = 0, n = gas.size();
        int start_pos = 0, max_sum = 0, temp_sum = 0;
        for(int i = n-1; i >= 0; i--){
            temp_sum += gas[i]-cost[i];
            if(temp_sum > max_sum){
                max_sum = temp_sum;
                start_pos = i;
            }
        }
        if(temp_sum >= 0)
            return start_pos;
        return -1;
    }
};
```