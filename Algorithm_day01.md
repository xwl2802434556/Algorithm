### 剑指 Offer 14- I. 剪绳子 && 343. 整数拆分 ###
- 解法：动态规划
- 状态：dp[n]表示将长度为n的绳子拆分后得到的最大乘积
- 转移方程：dp[n] = max(dp[i] * dp[n - i])
- 代码：
	`if(n <= 3) return n - 1;
    int dp[n + 1];
    dp[0] = 0, dp[1] = 1, dp[2] = 2, dp[3] = 3;
    for(int i = 4; i <= n; i++){
        dp[i] = -1;
        for(int j = 1; j <= i / 2; j++){
            dp[i] = max(dp[i], dp[j] * dp[i - j]);
        }
    }
    return dp[n];`
### 剑指 Offer 14- II. 剪绳子 II ###
Solution:[https://leetcode-cn.com/problems/jian-sheng-zi-ii-lcof/solution/mian-shi-ti-14-ii-jian-sheng-zi-iitan-xin-er-fen-f/](https://leetcode-cn.com/problems/jian-sheng-zi-ii-lcof/solution/mian-shi-ti-14-ii-jian-sheng-zi-iitan-xin-er-fen-f/)
Code:
	`if(n <= 3) return n - 1;
    int p = 1e9 + 7;
    long long ans = 1;
    while(n > 4){
        ans = ans * 3 % p;
        n -= 3;
    }
    return ans * n % p;`
### 剑指 Offer 15. 二进制中1的个数 ###
解析：n & (n - 1)用于将n的二进制数最后一个1置为0，比如
n = 111000, n - 1 = 110111;
Code:
	`int ans = 0;
    while(n){
        ans++;
        n = n & (n - 1);
    }
    return ans;`