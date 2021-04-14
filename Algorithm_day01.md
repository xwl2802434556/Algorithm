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
### 剑指 Offer 20. 表示数值的字符串 ###
Solution：[https://leetcode-cn.com/problems/biao-shi-shu-zhi-de-zi-fu-chuan-lcof/solution/que-ding-you-xian-zi-dong-ji-dfa-by-justyou/](https://leetcode-cn.com/problems/biao-shi-shu-zhi-de-zi-fu-chuan-lcof/solution/que-ding-you-xian-zi-dong-ji-dfa-by-justyou/)
有限自动机：模拟正确的结果，然后填表，最后根据没有通过的案例修改表项即可。
MainCode:
	```
	int transTable[10][6] = {{0,1,2,-1,8,-1},{-1,-1,2,-1,8,-1},{9,-1,2,5,3,-1},{9,-1,4,5,-1,-1},{9,-1,4,5,-1,-1},{-1,6,7,-1,-1,-1},{-1,-1,7,-1,-1,-1},{9,-1,7,-1,-1,-1},{-1,-1,4,-1,-1,-1},{9,-1,-1,-1,-1,-1}};
    int start = 0;
    int end[5] = {2,3,4,7,9};
	```
### 剑指 Offer 21. 调整数组顺序使奇数位于偶数前面 ###
- 双指针
### 面试题23：链表中环的入口节点 ###
1. 先确定是否有环(快慢指针，快的追上慢的就有环)
2. 计算环的长度
3. 用双指针，一个先移动环长步，然后两个一起移动，遇到的节点就是结果。
### 剑指 Offer 24. 反转链表 ###
- 头插法
### 剑指 Offer 25. 合并两个排序的链表 ###
- 归并，新建一个节点，然后往里面尾插，最后删掉该节点。
### 剑指 Offer 26. 树的子结构 ###
1. 将A中所有节点值等于B根节点的节点保存在数组中。
2. 对于数组中所有节点，与B比较是否相同即可。
### 剑指 Offer 27. 二叉树的镜像 ###
- 递归，先颠倒左子树的左右子树，再颠倒右子树的左右子树，最后颠倒根节点的左右子树。
### 剑指 Offer 28. 对称的二叉树 ###
1. 判断左子树的左节点 与 右子树的右节点
2. 判断左子树的右节点 与 右子树的左节点
### 剑指 Offer 29. 顺时针打印矩阵 ###
1. 设置四个变量up,down,left,right表示边界
2. 设置变量direct表示方向，右下左上对应0123
3. 当前访问位置(i,j), 结果下标k
4. 当向右访问时，当j<=right时一直循环将值放入结果；出循环时，将访问位置改变指向下一个位置(i++,j--)，同时将up边界改变为up++；同理向其他方向也是这样操作。
5. 模拟一下循环条件可以有两种：k < 矩阵元素数目 或者 up <= down && left <= right.
### 剑指 Offer 30. 包含min函数的栈 ###
- 双栈，每次push时，辅助栈都push最小值，这样辅助栈栈顶一直都对应着栈中的最小值。
### 剑指 Offer 31. 栈的压入、弹出序列 ###
1. 思考：当poped第一个元素为4，那么此时栈中必须是1234，不然第一个元素必不为4；
2. 算法：
	- 对于poped中每一个元素
		1. 如果栈顶元素与它不同，则一直将pushed元素入栈，直到pushed无元素或找到相同元素。
		2. 此时判断栈顶元素与它是否相同
			1. 不同则return false;
			2. 相同则弹出栈
3. Code：
	```
	bool validateStackSequences(vector<int>& pushed, vector<int>& popped) {
        int n = pushed.size();
        stack<int> st;
        int idx = 0;
        for(int p : popped){
            while((st.empty() || st.top() != p) && idx < n) st.push(pushed[idx++]);
            if(idx == n && st.top() != p) return false;
            st.pop();
        }
        return true;
    }
	```
### 剑指 Offer 32 - I II III. 从上到下打印二叉树 ###
- 层序遍历
### 剑指 Offer 33. 二叉搜索树的后序遍历序列 ###
- 递归的比较左子树和右子树，左子树都比根节点小，右子树都比根节点大
### 剑指 Offer 34. 二叉树中和为某一值的路径 ###
- 深度优先搜索
- Code：
```cpp
class Solution {
    vector<vector<int>> ans;
    vector<int> tmp;
    int target;
public:
    vector<vector<int>> pathSum(TreeNode* root, int target) {
        this->target = target;
        helper(root, 0);
        return ans;
    }
    void helper(TreeNode* &root, int sum){
        if(!root) return;
        int val = root->val;
        tmp.push_back(val);
        if(sum + val == target && !root->left && !root->right){
            ans.push_back(tmp);
        }
        helper(root->left, sum + val);
        helper(root->right, sum + val);
        tmp.pop_back();
    }
};
```
### 剑指 Offer 35. 复杂链表的复制 ###
- 深拷贝，用map保存<原始的，复制的>；函数返回节点的复制节点；
- 其他遍历也行
### 剑指 Offer 36. 二叉搜索树与双向链表 ###
- 中序遍历，将节点保存在数组中；然后遍历数组构造即可。
	```C++
	vector<Node*> arr;
	void inorder(Node* root){
	    if(!root) return;
	    inorder(root->left);
	    arr.push_back(root);
	    inorder(root->right);
	}
	Node* treeToDoublyList(Node* root) {
	    if(!root) return nullptr;
	    inorder(root);
	    int n = arr.size();
	    Node *head = arr[0], *rear = arr[n - 1];
	    Node *p, *q;
	    for(int i = 1; i < n; i++){
	        p = arr[i - 1], q = arr[i];
	        p->right = q;
	        q->left = p;
	    }
	    head->left = rear;
	    rear->right = head;
	    return head;
	}
	```
- 中序遍历，两个全局变量，用节点pre指向当前节点的前驱节点，head节点指向头节点，初始都为空；
	```
	Node *pre = nullptr, *head = nullptr;
    void inorder(Node *root){
        if(!root) return;
        inorder(root->left);
        if(pre) pre->right = root;
        else head = root;
        root->left = pre;
        pre = root;
        inorder(root->right);
    }
    Node* treeToDoublyList(Node* root) {
        if(!root) return nullptr;
        dfs(root);
        head->left = pre;
        pre->right = head;
        return head;
    }
	```
### 剑指 Offer 37. 序列化二叉树 ###
- 本题不能用先序加中序构造二叉树，因为可能有值相同的节点。
- 使用先序序列化二叉树，然后使用先序反序列化
- Code1:
	```C++
	// Encodes a tree to a single string.
    string serialize(TreeNode* root) {
        string s;
        ser(root, s);
        return s;
    }
    // Decodes your encoded data to tree.
    TreeNode* deserialize(string data) {
        vector<string> nums = split(data);
        TreeNode* r = nullptr;
        build(nums, r);
        return r;
    }
    //ser
    void ser(TreeNode* t, string &s){
        if(!t){
            s.append("X,");
            return;
        }
        s.append(to_string(t->val));
        s.push_back(',');
        ser(t->left, s);
        ser(t->right, s);
    }
    //build
    int idx = 0;
    void build(vector<string> &nums, TreeNode* &t){
        if(idx == nums.size()) return;
        if(nums[idx] == "X"){
            t = nullptr;
            idx++;
            return;
        }
        t = new TreeNode(stoi(nums[idx]));
        idx++;
        build(nums, t->left);
        build(nums, t->right);
    }
    //helper
    vector<string> split(string &s){
        int n = s.size();
        vector<string> ret;
        int i = 0, j = 0;
        while(i < n){
            j = i;
            while(i < n && s[i] != ',') i++;
            ret.push_back(s.substr(j, i - j));
            i++;
        }
        return ret;
    }
	```
- 层序遍历，反序列化时，将根节点入队列。
- Code2:
	```C++
	// Encodes a tree to a single string.
    string serialize(TreeNode* root) {
        if(!root) return ",";
        string ans;
        queue<TreeNode*> q;
        q.push(root);
        while(!q.empty()){
            TreeNode* x = q.front(); q.pop();
            if(!x){
                ans.append("X,");
                continue;
            }
            ans.append(to_string(x->val));
            ans.push_back(',');
            q.push(x->left);
            q.push(x->right);
        }
        return ans;
    }
    // Decodes your encoded data to tree.
    TreeNode* deserialize(string data) {
        if(data == ",") return nullptr;
        vector<string> nums = split(data);
        TreeNode* ans = new TreeNode(stoi(nums[0]));
        int i = 1, n = nums.size();
        queue<TreeNode*> q;
        q.push(ans);
        while(i < n){
            TreeNode* t = q.front(); q.pop();
            if(nums[i] == "X"){
                t->left = nullptr;
            }else{
                TreeNode* left = new TreeNode(stoi(nums[i]));
                t->left = left;
                q.push(left);
            }
            i++;
            if(nums[i] == "X"){
                t->right = nullptr;
            }else{
                TreeNode* right = new TreeNode(stoi(nums[i]));
                t->right = right;
                q.push(right);
            }
            i++;
        }
        return ans;
    }
    //helper
    vector<string> split(string &s){
        int n = s.size();
        vector<string> ret;
        int i = 0, j = 0;
        while(i < n){
            j = i;
            while(i < n && s[i] != ',') i++;
            ret.push_back(s.substr(j, i - j));
            i++;
        }
        return ret;
    }
	```
### 剑指 Offer 38. 字符串的排列 ###
- 全排列，用回溯，看看weiwei哥就够了
- [https://leetcode-cn.com/problems/permutations/solution/hui-su-suan-fa-python-dai-ma-java-dai-ma-by-liweiw/](https://leetcode-cn.com/problems/permutations/solution/hui-su-suan-fa-python-dai-ma-java-dai-ma-by-liweiw/)
### 剑指 Offer 39. 数组中出现次数超过一半的数字 ###
```
原理：一对不同的元素同时消去，那么最后剩下的相同元素就是结果
    cur表示可能的结果，cnt表示cur出现次数
    初始时cur=nums[0],cnt=1;
    从数组第二个元素开始遍历
        如果当前元素等于cur，那么cur的出现次数cnt++,继续下一个元素
        否则，当cur--
            如果cur等于0，那么说明当前元素和cur可能不是结果，将cur设置为下一个元素
            否则，继续下一个元素
```
###  ###
- 根据快速排序中的划分算法
- Code1:
	```cpp
    vector<int> getLeastNumbers(vector<int>& arr, int k) {
        int n = arr.size();
        find(arr, 0, n - 1, k);
        vector<int> ans;
        for(int i = 0; i < k; i++){
            ans.push_back(arr[i]);
        }
        return ans;
    }
    void find(vector<int>& arr, int low, int high, int k){
        if(low > high) return;
        int t = partion(arr, low, high);
        if(t - low + 1 == k) return;
        else if(t - low + 1 > k) find(arr, low, t - 1, k);
        else find(arr, t + 1, high, k - (t - low + 1));
    }
    int partion(vector<int>& arr, int low, int high){
        int x = arr[low];
        while(low < high){
            while(low < high && arr[high] >= x) high--;
            arr[low] = arr[high];
            while(low < high && arr[low] <= x) low++;
            arr[high] = arr[low];
        }
        arr[low] = x;
        return low;
    }
	```
- 根据堆
- Code2:
	```cpp
	vector<int> getLeastNumbers(vector<int>& arr, int k) {
        if(k == 0) return {};
        priority_queue<int> heap(arr.begin(), arr.begin() + k);
        int n = arr.size();
        for(int i = k; i < n; i++){
            if(heap.top() <= arr[i]) continue;
            heap.pop();
            heap.push(arr[i]);
        }
        vector<int> ans;
        while(!heap.empty()){
            ans.push_back(heap.top());
            heap.pop();
        }
        return ans;
    }
	```
- 两者比较
	||基于partion解法|基于堆解法|
	|----|----|-----|
	|时间复杂度|O(n)|O(nlogk)|
	|是否修改数组|是|否|
	|是否使用海量数据|否|是|
### 33. 搜索旋转排序数组 ###
- Code:
```cpp
int search(vector<int>& nums, int target) {
    int n = (int)nums.size();
    int l = 0, r = n - 1;
    while(l <= r){
        int mid = l + (r - l) / 2;
        if(nums[mid] == target) return mid;
        if(nums[l] <= nums[mid]){   //左边有序
            if(nums[l] <= target && target < nums[mid]){
                r = mid - 1;
            }else l = mid + 1;
        }else{                      //右边有序
            if(nums[mid] < target && target <= nums[r]) l = mid + 1;
            else r = mid - 1;
        }
    }
    return -1;
}
```
### 81. 搜索旋转排序数组 II ###
- Code:
```cpp
bool search(vector<int>& nums, int target) {
    int n = nums.size();
    int l = 0, r = n - 1;
    while(l <= r){
        int mid = l + ((r - l) >> 1);
        if(nums[mid] == target) return true;
        if(nums[mid] < nums[r]){
            if(nums[mid] < target && target <= nums[r]) l = mid + 1;
            else r = mid - 1;
        }else if(nums[mid] > nums[r]){
            if(nums[l] <= target && target < nums[mid]) r = mid - 1;
            else l = mid + 1;
        }else r--;
    }
    return false;
}
```
### 剑指 Offer 41. 数据流中的中位数 ###
- 用两个堆来保存数据，最大堆保存较小的一半数据，最小堆保存较大的一半数据。变量c1表示最大堆的元素个数，c2表示最小堆的元素个数。
- 如果当前加入元素小于最大堆堆顶元素，说明该元素属于较小的一半数据。那么将其加入最大堆，同时c1++，这时c1如果大于c2+1，那么需要调整，将最大堆堆顶元素弹给最小堆，同时修改c1,c2。
- 否则，同样....
- Code:
```cpp
class MedianFinder {
    priority_queue<int> maxh;
    priority_queue<int,vector<int>, greater<int>> minh;
    int c1 = 0, c2 = 0;
public:
    MedianFinder() {
    }
    void addNum(int num) {
        if(c1 == 0){
            maxh.push(num);
            c1++;
            return;
        }
        if(num < maxh.top()){
            maxh.push(num); c1++;
            if(c1 > c2 + 1){
                minh.push(maxh.top());
                maxh.pop();
                c1--; c2++;
            }
        }else{
            minh.push(num); c2++;
            if(c2 > c1 + 1){
                maxh.push(minh.top());
                minh.pop();
                c2--; c1++;
            }
        }
    }
    double findMedian() {
        double ans = 0;
        if(c1 == c2) ans = (double)(maxh.top() + minh.top()) / 2;
        else if(c1 < c2) ans = minh.top();
        else ans = maxh.top();
        return ans;
    }
};
```
### 剑指 Offer 42. 连续子数组的最大和 ###
- 动态规划
### 剑指 Offer 43. 1～n 整数中 1 出现的次数 ###
- 找规律，计算个、十、百、千..位上的1出现次数，相加
- 个位1：每十个数中有一个
- 十位1：每百位数中有十个
- 百位1：每千位数中有百个
- 举例：
	- 1234
	- 求个位1：
		- 1234 / 10 + (1234 % 10 > 0 ? 1 : 0)
	- 求十位1：
		- 1234 / 100 * 10 + (1234 % 100 - 99 >= 100 ?)
		- 这里括号里的值a根据c=1234%100而定(10-19)
			- 如果c<10，那么a=0;
			- 如果c>20，那么a=10;
			- 否则a=c-9;
		- 所以括号里为(1234%100<10?0:(1234%100>20?10:1234%100-9))
- Code:
```cpp
int countDigitOne(int n) {
        long ans = 0, bit = 1, sub = 0;
        while(bit <= n){
            long mod = bit * 10;
            ans = ans + n / mod * bit + (n % mod - sub <= 0 ? 0 : (n % mod - sub >= bit ? bit : n % mod - sub));
            bit = mod;
            sub = sub * 10 + 9;
        }
        return ans;
    }
```
### 剑指 Offer 44. 数字序列中某一位的数字 ###
- 找规律
- 一位数总数有10个
- 两位数总数有180个---2 * 10 * 9
- 三位数总数有2700个--3 * 100 * 9.....
- 算法：从一位数开始，计算一位数总数all，与n比较大小，大于则n-=all,再比较两位数..，直到找到一个n<all的，计bit为位数，则计算查找的是bit位数中的第几个即可。
- Code:
```cpp
int findNthDigit(int n) {
    if(n < 10) return n;
    int bit = 2, base = 10;
    long mul = 90;
    long all = bit * mul;
    n -= 10;
    while(n >= all){
        n -= all;
        bit++;
        mul *= 10;
        all = bit * mul;
        base *= 10;
    }
    int a = n / bit + base, b = n % bit;
    while(bit - b > 1){
        a /= 10;
        b++;
    }
    return a % 10;
}
```
### 剑指 Offer 45. 把数组排成最小的数 ###
- 数字拼接可能会产生大数，int无法表示，所以需要将数字转换为字符串。
- Code:
```cpp
static bool cmp(string &a, string &b){
    return a + b < b + a;
}
string minNumber(vector<int>& nums) {
    vector<string> arr;
    for(int num : nums) arr.push_back(to_string(num));
    sort(arr.begin(), arr.end(), cmp);
    string ans;
    for(string num : arr) ans.append(num);
    return ans;
}
```
### 剑指 Offer 46. 把数字翻译成字符串 ###
- 根据最后一位数，最后两位数，进行递归
- 如果从前往后，会有重复子问题，比如12258,会计算258两次。
- Code:
```cpp
int translateNum(int num) {
    if(num >= 0 && num <= 9) return 1;
    if(num >= 10 && num <= 25) return 2;
    return translateNum(num / 10) + translateNum(num / 100) * (num % 100 >= 10 && num % 100 <= 25);
}
```
### 剑指 Offer 47. 礼物的最大价值 ###
- 动态规划，从右下角向左上角遍历，dp[i][j] = max(right, down) + grid[i][j];
- Code:
```cpp
int maxValue(vector<vector<int>>& grid) {
    int m = grid.size(), n = grid[0].size();
    for(int i = m - 1; i >= 0; i--){
        for(int j = n - 1; j >= 0; j--){
            int down = i + 1 < m ? grid[i + 1][j] : 0;
            int right = j + 1 < n ? grid[i][j + 1] : 0;
            grid[i][j] = max(down, right) + grid[i][j];
        }
    }
    return grid[0][0];
}
```
### 剑指 Offer 48. 最长不含重复字符的子字符串 ###
- 双指针，哈希表
- 只有当字符出现过，并且比左指针大时，才修改左指针。
- Code：
```cpp
int lengthOfLongestSubstring(string s) {
    int n = s.size();
    unordered_map<char,int> hash;
    int ans = 0, l = 0, r = 1;
    for(int i = 0; i < n; i++){
        char c = s[i];
        if(hash.find(c) != hash.end() && hash[c] >= l){
            l = hash[c] + 1;
        }
        ans = max(ans, i - l + 1);
        hash[c] = i;
    }
    return ans;
}
```
### 剑指 Offer 49. 丑数 ###
- 丑数应该是另一个丑数的2,3,5倍，可以创建一个数组，保存排好序的丑数，每个丑数都由前面的丑数得到。
- 将每个丑数分别乘上2,3,5，将其与刚好大于已经计算出来的最大丑数比较，找到刚好大于最大丑数的最小丑数加入数组中。
- 可以想象存在一个丑数T2，排在它前面的丑数乘以2结果会小于已有的最大丑数，在它之后的丑数乘以2结果都会太大，只需记住T2位置即可，同样也存在T3,T5
- Code:
```cpp
int nthUglyNumber(int n) {
    vector<int> dp(n + 1);
    dp[1] = 1;
    int idx2 = 1, idx3 = 1, idx5 = 1;
    int i = 2;
    while(i <= n){
        int ugly2 = dp[idx2] * 2, ugly3 = dp[idx3] * 3, ugly5 = dp[idx5] * 5;
        int ugly = min(ugly2, min(ugly3, ugly5));
        if(ugly == ugly2) idx2++;
        if(ugly == ugly3) idx3++;
        if(ugly == ugly5) idx5++;
        dp[i++] = ugly;
    }
    return dp[n];
}
```
### 剑指 Offer 50. 第一个只出现一次的字符 ###
- 第一次遍历，统计每个字符出现的次数
- 第二次遍历到第一个只出现一次的字符时结束遍历，返回
- Code:
```cpp
char firstUniqChar(string s) {
    int n = s.size();
    vector<int> hash(256);
    for(char c : s) hash[c]++;
    char ans = ' ';
    for(char c : s){
        if(hash[c] == 1){
            ans = c;
            break;
        }
    }
    return ans;
}
```
### 剑指 Offer 51. 数组中的逆序对 ###
- 使用归并排序计算逆序对
- 两个已排序数组合并为一个排序数组时
- 左边数组的当前值如果大于右边数组当前值，那么说明左边数组中所有值均大于右边数组当前值，那么结果加上左边数组元素个数
- Code:
```cpp
class Solution {
    int ans = 0;
    vector<int> tmp;
public:
    int reversePairs(vector<int>& nums) {
        int n = nums.size();
        tmp.resize(n);
        mergeSort(nums, 0, n - 1);
        return ans;
    }
    void merge(vector<int>& nums, int left, int mid, int right){
        //vector<int> tmp(right - left + 1);
        int k = 0, i = left, j = mid + 1;
        while(i <= mid && j <= right){
            if(nums[i] <= nums[j]) tmp[k++] = nums[i++];
            else{
                ans += mid - i + 1;
                tmp[k++] = nums[j++];
            }
        }
        while(i <= mid) tmp[k++] = nums[i++];
        while(j <= right) tmp[k++] = nums[j++];
        for(int i = 0; i < k; i++) nums[i + left] = tmp[i];
    }
    void mergeSort(vector<int>& nums, int l, int r){
        if(l >= r) return;
        int mid = (l + r) >> 1;
        mergeSort(nums, l, mid);
        mergeSort(nums, mid + 1, r);
        merge(nums, l, mid, r);
    }
};
```
### 剑指 Offer 52. 两个链表的第一个公共节点 ###
- 解法一：先遍历一个链表，用哈希表保存每个节点；再遍历另一个链表，如果节点在哈希表中，直接返回，否则返回nullptr
- 解法二：计算两个链表长度，将较长的链表后移到短链表同样长度，然后两者同时后移，返回第一个相同的节点。