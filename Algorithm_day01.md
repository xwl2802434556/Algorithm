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
### 剑指 Offer 16. 数值的整数次方 ###
- 要考虑exponent是否小于等于0；
- 还要能够处理错误，当base=0，exponent<0，用一个全局变量表示是否出错
- 考虑特殊边界INT_MIN无法在前面直接加-
- 还要考虑时间复杂度，同时用位运算更加快速
- 解法：判断特殊边界，同时将exponent变成无符号整数，这样对正数负数处理就统一了。
- Code:
	`bool g_InvalidInput = false;
    double myPow(double base, int exponent) {
        g_InvalidInput = false;
        if(base == 0 && exponent < 0){
            g_InvalidInput = true;
            return 0.0;
        }
        unsigned int absExponent = (unsigned int)exponent;
        if(exponent < 0 && exponent != INT_MIN) absExponent = (unsigned int)(-exponent);
        if(exponent == INT_MIN) absExponent = (unsigned int)(INT_MIN);
        double result = PowerWithUnsignedExponent(base, absExponent);
        if(exponent < 0) result = 1.0 / result;
        return result;
    }
    double PowerWithUnsignedExponent(double base, unsigned int absExponent){
        if(absExponent == 0) return 1;
        if(absExponent == 1) return base;
        double result = PowerWithUnsignedExponent(base, absExponent >> 1);
        result *= result;
        if((absExponent & 1) == 1) result *= base;
        return result;
    }`
### 剑指 Offer 17. 打印从1到最大的n位数 ###
- 没有指定n的大小，可能是大树问题，用字符串表示大树。
- 解法：设置一个字符串，长度为n+1，默认全为'0'，最后一位为0，每次模拟整数加法加1，并输出；结束条件：第一位从'9'变成'0';
- Code:
	```C++
	bool Increment(char *str, int n){
	    bool noOverFlow = true;	//判断是否达到最大值，没有返回true
	    char cnt = 1;   //进位，初始为0，但为了与后面一致，就设为1
	    for(int i = n - 1; i >= 0; i--){
			//判断每一位数，如果加上进位小于10，直接退出
	        char x = str[i] - '0' + cnt;
	        if(x < 10){
	            str[i] = x + '0';
	            break;
	        }
			//否则，将本位数字置0，进位置1.
	        if(i == 0) noOverFlow = false;
	        str[i] = '0'; cnt = 1;
	    }
	    return noOverFlow;
	}
	void Print(char *str, int n){
	    //将前面的0都跳过，然后再输出
	    int idx = 0;
	    while(idx < n && str[idx] == '0') idx++;
	    printf("%s\t", str + idx);
	}
	void PrintNumbers(int n){
	    if(n < 0) return;
	    char *str = new char[n + 1];
	    memset(str, '0', n);
	    str[n] = 0;
	    while(Increment(str, n)){
	        Print(str, n);
	    }
	    delete[] str;
	}```
### 剑指 Offer 18. 删除链表的节点 ###
- 删除一个节点，有两种方法；
- 一种找到前驱节点，时间复杂度为O(n)
- 另一种是直接将待删除结点的下一个节点p的值赋值给待删除节点，然后将p删除，时间复杂度为O(1)。
- 第二种方法有两个缺点：一：无法保证待删除节点是否在链表中；二：如果待删除结点是尾节点，仍然要找到其前驱节点。
- 特殊情况：链表只有一个节点，且是待删除结点，那么要将链表和待删除结点都置nullptr.
### 剑指 Offer 19. 正则表达式匹配 ###
- Solution:[https://leetcode-cn.com/problems/zheng-ze-biao-da-shi-pi-pei-lcof/solution/zhu-xing-xiang-xi-jiang-jie-you-qian-ru-shen-by-je/](https://leetcode-cn.com/problems/zheng-ze-biao-da-shi-pi-pei-lcof/solution/zhu-xing-xiang-xi-jiang-jie-you-qian-ru-shen-by-je/)
- 解法：
	- 判断最后一个字符是否相同
		- 相同则同时前移一位继续比较；
		- 不同的话
			- 如果模式串最后一位是'.'，那么两串都迁移一位
			- 如果模式串最后一位为'*'，那么比较模式串前一位与字符串最后一位，
				- 如果相同，那么字符串前移一位，模式串不变，继续比较
				- 不同，则字符串不变，模式串迁移两位。***（这里有点误解，如果模式串形如'.*'，那么就有两种情况，可以去掉模式串后两位，也可以去掉字符串最后一位）
			- 否则，返回false
- 边界的判断：
	- 模式串为空，则只用判断字符串即可。
	- 模式串不为空，那可能形如'a*b*'，可以匹配空串。
- Code：
	```C++
	bool isMatch(string s, string p) {
        this->s = s, this->p = p;
        int n = s.size(), m = p.size();
        return helper(n, m);
    }
    bool helper(int n, int m){
        if(m == 0) return n == 0;
        if(n == 0){
            if(m & 1) return false;
            while(m){
                if(p[m - 1] != '*') return false;
                m -= 2;
            }
            return true;
        }else{
            int lasts = n - 1, lastp = m - 1;
            if(s[lasts] == p[lastp] || p[lastp] == '.') return helper(n - 1, m - 1);
            else if(p[lastp] != '*') return false;
            else{
                //模式串不会是*开头，且不会是这种aa*
                if(p[lastp - 1] == s[lasts] || p[lastp - 1] == '.') return helper(n - 1, m) || helper(n, m - 2);
                else return helper(n, m - 2);
            } 
        }
    }
	```
- 这题跟编译原理有关，状态机明天看看会不会。
