# Problem & Contest

## leetcode

**37.解数独**

**用 b& (−b) 得到b二进制表示中最低位的1**
**我们可以用 b 和最低位的 1 进行按位异或运算b=(b^(b&(-b))，就可以将其从 b 中去除，这样就可以枚举下一个 1。同样地，我们也可以用b&(b-1)进行按位与运算达到相同的效果.**
**如果一个空白格只有唯一的数可以填入，也就是其对应的 b 值和b−1 进行按位与运算后得到 0**（即 b 中只有一个二进制位为 1）。此时，我们就可以确定这个空白格填入的数，而不用等到递归时再去处理它。

**172.阶乘后的零**

![image-20220828100807053](Problem & Contest.assets/image-20220828100807053.png)

时间复杂度O(n)

**528.按权重随机选择**

~~~C++
class Solution {
private:
    mt19937 gen;
    uniform_int_distribution<int> dis;
    vector<int> pre;

public:
    Solution(vector<int>& w): gen(random_device{}()), dis(1, accumulate(w.begin(), w.end(), 0)) {
        partial_sum(w.begin(), w.end(), back_inserter(pre));
    }
    
    int pickIndex() {
        int x = dis(gen);
        return lower_bound(pre.begin(), pre.end(), x) - pre.begin();
    }
};
~~~

1. mt19937头文件是\<random> 是伪随机数产生器，用于产生高性能的随机数
2. uniform_int_distribution 头文件在\<random>中，是一个随机数分布类，参数为生成随机数的类型，构造函数接受两个值表示区间段
3. partial_sum函数的头文件在\<numeric>，对(first, last)内的元素逐个求累计和，放在result容器内
4. back_inserter函数头文件\<iterator>，用于在末尾插入元素。

**652.寻找重复的子树**

~~~C++
class Solution {
public:
    vector<TreeNode*> findDuplicateSubtrees(TreeNode* root) {
        dfs(root);
        return {repeat.begin(), repeat.end()};
    }

    int dfs(TreeNode* node) {
        if (!node) {
            return 0;
        }
        auto tri = tuple{node->val, dfs(node->left), dfs(node->right)};
        if (auto it = seen.find(tri); it != seen.end()) {
            repeat.insert(it->second.first);
            return it->second.second;
        }
        else {
            seen[tri] = {node, ++idx};
            return idx;
        }
    }

private:
    static constexpr auto tri_hash = [fn = hash<int>()](const tuple<int, int, int>& o) -> size_t {
        auto&& [x, y, z] = o;
        return (fn(x) << 24) ^ (fn(y) << 8) ^ fn(z);
    };

    unordered_map<tuple<int, int, int>, pair<TreeNode*, int>, decltype(tri_hash)> seen{0, tri_hash};
    unordered_set<TreeNode*> repeat;
    int idx = 0;
};
~~~

**761.特殊的二进制序列**

由于题目定义特殊的二进制序列为，0和1数量相等且每一个前缀码中 1 的数量要大于等于 0 的数量，因此可以将1映射为(，0映射为)

**782.变为棋盘**

对于这类问题应该先考虑什么样的棋盘可以变为题目要求的棋盘

## Codeforces

1. **1060C** 矩阵C\[i][j]=a\[i]*b\[j]，C中子矩阵的元素和等于a和b的子数组的元素和的积。

2. **819 Div.2**

   ![image-20220826200939944](Problem & Contest.assets/image-20220826200939944.png)

   ~~~C++
   int ans = 0;
   for (int i = 1; i <= n; ++i)
   {
       ans += (a[i] != a[i + 1]) * (n - (i + 1) + 1) * i;
   }
   while (m--)
   {
       int i, x;
       cin >> i >> x;
       ans -= (a[i] != a[i - 1]) * (n - i + 1) * (i - 1);
       ans -= (a[i + 1] != a[i]) * (n - (i + 1) + 1) * i;
       a[i] = x;
       ans += (a[i] != a[i - 1]) * (n - i + 1) * (i - 1);
       ans += (a[i + 1] != a[i]) * (n - (i + 1) + 1) * i;
       cout << ans + n * (n + 1) / 2 << '\n';
   }
   ~~~

3. **808 Div.2**

   **D. Difference Array**

   ![image-20220823224000611](Problem & Contest.assets/image-20220823224000611.png)

4. **807 Div.2**

   ![image-20220824094644769](Problem & Contest.assets/image-20220824094644769.png)

5. **805 Div.3**

   ![image-20220826155542022](Problem & Contest.assets/image-20220826155542022.png)

   ~~~C++
   //hai'k'y
   vector<int> deg(n);
   dsu d(2 * n);
   for (int i = 0; i < n; i++) {
   	int x, y; cin >> x >> y; x--; y--;
   	deg[x]++; deg[y]++;
   	d.unite(x, y + n);
   	d.unite(x + n, y);
   }
   bool flag = true;
   for (int i = 0; i < n; i++) {
   	if (deg[i] > 2) {
   		flag = false;
   	}
   	if (d.get(i) == d.get(i + n)) {
   		flag = false;
   	}
   }
   cout << (flag ? "YES" : "NO") << endl;
   ~~~

6. 933 A

   ![image-20220831221632217](Problem & Contest.assets/image-20220831221632217.png)

   ![image-20220831221649734](Problem & Contest.assets/image-20220831221649734.png)

## Google competition

1. 对于一组点，最左边为l，最右边为r，我们选取(l+r)>>1作为中心点，这样所有的点到中心点总cost最小，结果为|v0-x| + |v1-x| + ... + |vk-x|
2. 但如果我们想让这组点连续并使总cost最小，假设连续的第一个位置为x，那么结果为：|v0-x| + |v1-(x+1)| + ... + |vk-(x+k-1)|，因此我们只需要让vi-i即可转化为问题1

### kick start 2021

**Round D**

对于区间计数问题可以使用差分。

### kick start 2019

**Round A**

将一个矩阵A中某些点及离该点曼哈顿距离为lim的所有点统一映射到另一个矩阵B中（其中矩阵A大小为n*n，则映射后的矩阵B大小为4n\*4n）
点(x,y)及离该点曼哈顿距离为lim的所有点映射到矩阵B的区域为左上角点**(lnx,lny)-**>右下角点**(rnx,rny)**。

~~~C++
int lnx = max(x - lim + y, 0);
int lny = max(x - lim - y + m, 0);
int rnx = max(x + lim + y, 0);
int rny = max(x + lim - y + m, 0);
~~~

矩阵B中某个点(x,y)，还原为A中(a,b)

~~~C++
int a = (x + y - m) / 2, b = x - a;
assert((x & 1) == ((y + m) & 1))//需要满足的条件
~~~

这是通过更换坐标系，将矩阵A中的(x,y)映射为矩阵B中的(x+y,x-y+m)

**Round D**

1. 两个数字异或以后的数位奇偶性满足只有在奇加偶的情况下才等于奇。
2. 就是多个数字**&  |  ^**，计算的先后顺序不影响最后结果

**Round F**

用一些杂乱无章的点中的部分点构成一个多边形来困住某个点，则一定可以用三角形和四边形来困住。通过判断一个点是否在多边形内可以用叉乘。

**Round H**

![image-20220818224836143](Problem & Contest.assets/image-20220818224836143.png)

### kick start 2018

**Round B**

能被9整除的数各个位上的数字加起来是9的倍数
每十个数中一定会有一个数被9整除并且还有一个数字含有9（除了边界情况）
考虑区间1-123，可以拆分为：1-99、100-119、120-123
1-99又可以拆分为：1-9（共8个）10-19、20-29…共有1×8×9个满足条件
100-119中有2×8个满足条件
120-123中有4个满足条件
因此总计100个no nine
考虑更一般的情况，区间1-num：
则有Σnum[i] * 8 * 9^(num.size()-2-i)，当i=num.size()-1时，需要逐个判断，复杂度为O(logn)

**Round C**

1. **problem B** 

   所有的三角形都是凸多边形。

2. **Problem C**

![image-20220806163944938](Problem & Contest.assets/image-20220806163944938.png)

**Round D**

![image-20220809193251988](Problem & Contest.assets/image-20220809193251988.png)

此题的核心在于坐标的变化

**Round H**

![image-20220820163908699](Problem & Contest.assets/image-20220820163908699.png)

### kick start 2017

**Round A**

一个\*可以匹配0-4个字符，可以将一个\*转化为4个\*，这样就可以正常转换了。

## Atcoder

**AtCoder Beginner Contest 265**

**E-Warp**

![image-20220823111750949](Problem & Contest.assets/image-20220823111750949.png)

**AtCoder Beginner Contest 264**

D-Left Right Operation

![image-20220824221905101](Problem & Contest.assets/image-20220824221905101.png)

这样每个元素都有三种状态，这样可以 通过dp来求得答案。

**E-Sugoroku 3**

![image-20220824222349086](Problem & Contest.assets/image-20220824222349086.png)

dp[i]＝（dp[i]+dp[i+1]+ dp[i+a[i]])/（a[i]+1）+1进行移项

**AtCoder Beginner Contest 262**

**D-I Hate Non-integer Number**

![image-20220825172544543](Problem & Contest.assets/image-20220825172544543.png)

N的范围为[1,100]

**E - Red and Blue Graph**

![image-20220825202212204](Problem & Contest.assets/image-20220825202212204.png)

### Atcoder经典dp题

1. **E - Knapsack 2**

2. **I - Coins**

3. **J - Sushi**
   ![image-20220817085846121](Problem & Contest.assets/image-20220817085846121.png)

4. **K-stones**

   设置dp[i]: 取i颗石子的胜负情况，dp[i] 为真则先手胜，否则后手胜。

   重点是找到破题点：剩余的石子中可以被一次刚好拿完的记下来，由此看一看剩 k个石子时能不能赢。

   ~~~C++
   fort(i, 1, k + 1) {
   	forn(j, n) {
   		if (i >= a[j] && g[i - a[j]] == 0) {
   			g[i] = 1; break;
   		}
   	}
   }
   cout << (g[k] ? "First" : "Second") << endl;
   ~~~

5. **L-Deque**

   ~~~C++
   forn(i, n) {
   	cin >> a[i];
   	dp[i][i] = a[i];
   }
   //dp[i][j]表示区间为(i,j)时，两个玩家玩得最优化时，X-Y的结果值
   fort(l, 1, n) {
   	for (int i = 0, j = l; j < n; i++, j++) {
   		dp[i][j] = max(a[i] - dp[i + 1][j], a[j] - dp[i][j - 1]);
   	}
   }
   cout << dp[0][n - 1] << endl;
   ~~~

6. **M-Candies**

   ~~~C++
   //TLE做法
   f[0] = 1;//f[i]是i个蛋糕分配得方案数
   forn(i, n) {
       int a; cin >> a;
   	for (int j = m; j >= 0; j--)
   	{
   		fort (k, 1, a)
   		{
   			if (j >= k)f[j] += f[j - k];
   		}
   	}
   }
   cout << f[m] << endl;
   ~~~

   ~~~C++
   //优化做法 通过数组s来记录前缀和
   vll f(k + 1), sum(k + 1); f[0] = 1;
   forn(i, n) {
   	int a; cin >> a;
   	sum[0] = f[0];
   	fort(j, 1, k + 1) sum[j] = (sum[j - 1] + f[j]) % MOD;
   	forn(j, k + 1) {
   		f[j] = sum[j];
   		if (j > a) f[j] = (f[j] + MOD - sum[j - a - 1]) % MOD;
   	}
   }
   cout << f[k] << endl;
   ~~~

7. **O-matching(状压dp)**

   ~~~C++
   vl dp(1 << n);
   forn(i, n){
       forn(j, n) cin >> a[i][j];
   }
   dp[0] = 1;
   forn(i, n)
   {
       forn(j, 1 << n)//dp[j]为前i个男人匹配了j个女人的方案数
       {
           if (__builtin_popcount(j) != i)
               continue;
           forn(xx, n)
           {
               if (a[i][xx] && !((j >> xx) & 1))
                   dp[j + (1 << xx)] = (dp[j + (1 << xx)] + dp[j]) % mod;
           }
       }
   }
   cout << dp.back() << endl;
   ~~~

8. **Q-Flowers**

   建立数组dp，dp[i]为取第i朵花时的最大价值，在求要第i支花时的最大值时，要找到他前边比他矮的花的价值最大值，然后dp[i]=dp[j]+v[i]，在找dp[j]时，不能遍历。因为高度在1−n这个范围里，所以可以建立一个树状数组来存储前i个花的价值，这样就可以在lognlogn的时间内求出前i支花中高度在1−(w[i]−1)的价值的最大值。

   ~~~C++
   //用树状数组修改和维护区间最大值
   auto lowbit = [&](ll x) {
       return x & (-x);
   };
   auto update = [&](ll m, ll x) {
       while (m <= n) {
           f[m] = max(f[m], x);
           m += lowbit(m);
       }
   };
   auto query = [&](ll m) {
       ll res = 0;
       while (m > 0) {
           res = max(res, f[m]);
           m -= lowbit(m);
       }
       return res;
   };
   
   ll ans = 0;
   forn(i, n) {
       dp[i] = a[i] + query(h[i]);
       ans = max(ans, dp[i]);
       update(h[i], dp[i]);
   }
   ~~~

9. **R - Walk**

   DP+矩阵快速幂

   ![image-20220903220647461](Problem & Contest.assets/image-20220903220647461.png)

   我们设a\[i][j]为从i走到j，走了k次的方案数，上图为k=0初始时的情况，此时只把从自己走到自己设为1作为初始状态。**那么怎样求k=1时的状态矩阵呢？只需要乘一次邻接矩阵即可**

10. **T-Permutation**(插入dp)

   ![image-20220905230040499](Problem & Contest.assets/image-20220905230040499.png)

   ~~~C++
   vvl f(n + 1, vl(n + 1)), sum(n + 1, vl(n + 1));
   //f[i][j]表示前i个数字为1-i的全排列并且第i个数字是j且满足大小关系的方案数
   //sum[i][j]表示第i个位置放1-j的方案数之和，即dp数组的前缀和
   f[1][1] = 1;
   fort(i, 1, n + 1) sum[1][i] = 1;
   fort(i, 1, n) {
       fort(j, 1, i + 2) {
           if (s[i - 1] == '<')f[i + 1][j] = (f[i + 1][j] + sum[i][j - 1]) % mod;
           else f[i + 1][j] = (f[i + 1][j] + sum[i][i + 1] - sum[i][j - 1] + mod) % mod;
       }
       fort(j, 1, n + 1) sum[i + 1][j] = (sum[i + 1][j - 1] + f[i + 1][j]) % mod;
   }
   
   ll res = 0;
   fort(i, 1, n + 1) res = (res + f[n][i]) % mod;
   ~~~

11. **U - Grouping**(状压dp)

    ~~~c++
    fort(k, 1, 1 << n) {
        forn(i, n) {
            fort(j, i + 1, n) {
                if ((k >> i & 1) && (k >> j & 1)) {
                    dp[k] += a[i][j];
                }
            }
        }
        for (int j = k & (k - 1); j; j = (j - 1) & k) {
            dp[k] = max(dp[k], dp[k ^ j] + dp[j]);
        }
    }
    ~~~

12. **V - Subtree**

    ![image-20220906193909696](Problem & Contest.assets/image-20220906193909696.png)

    ~~~C++
    function<void(int, int)> dfs1 = [&](int u, int p) {
        f[u] = 1;
        for (int& v : G[u]) {
            if (v != p) {
                dfs1(v, u);
                f[u] = ((ll)f[u] * (f[v] + 1)) % m;
            }
        }
        pre[u] = vi(G[u].size(), 1);
        suf[u] = vi(G[u].size(), 1);
        fort(i, 1, G[u].size()) {
            pre[u][i] = pre[u][i - 1];
            if (G[u][i - 1] ^ p) {
                pre[u][i] = ((ll)pre[u][i] * (f[G[u][i - 1]] + 1)) % m;
            }
        }
        rfort(i, G[u].size() - 2, 0) {
            suf[u][i] = suf[u][i + 1];
            if (G[u][i + 1] ^ p) {
                suf[u][i] = ((ll)suf[u][i] * (f[G[u][i + 1]] + 1)) % m;
            }
        }
    };
    function<void(int, int)> dfs2 = [&](int u, int p) {
        forn(i, G[u].size()) {
            int v = G[u][i];
            if (v != p) {
                g[v] = ((((ll)pre[u][i] * suf[u][i]) % m) * g[u]) % m + 1;
                dfs2(v, u);
            }
        }
    };
    dfs1(0, 0);
    g[0] = 1;
    dfs2(0, 0);
    forn(i, n) cout << ((ll)g[i] * f[i]) % m << endl;
    ~~~

### 总结

1. 集合和二进制是一一对应的
1. 寻找最少的修改方案次数可以尝试bfs
1. 对于多个点同时为起点时，可以考虑建议一个超级源点指向这些点
1. 对于多次删边的问题如果比较难考虑的话可以逆向考虑

## 其他

设计哈希、LRU缓存、自动机。
排序方法包括：插入排序、冒泡排序、归并排序、快速排序、桶排序、基数排序。

1. 环形链表或者是环形数组都可以使用快慢指针，快慢指针要注意快指针的第一个next是否为空
4. 对于需要考虑左右的情况，可以用数组统计，.需要拼接（比如删除一个0让所有1连续）可以考虑前后缀
6. 复制图和链表需要哈希表
7. 链表中总是可以考虑插入新的节点来避免一些特殊情况。（比如一个 伪头节点dummy）
8. 有限的节点数量可以考虑状态压缩+dp
9. 如何只有小写或者大写字母可以考虑遍历
10. 问题是关于计算一些组合对象的数量。 对此，动态规划始终是答案。
11. **如果需要在一个数组中快速删除某些元素，链表是最好的解决方案。**
12. 除了括号外，如遇到相邻两个元素可以消除都可以用到栈



![image-20220713122529963](Problem & Contest.assets/image-20220713122529963.png)

![image-20220713122617693](Problem & Contest.assets/image-20220713122617693.png)



![image-20220713122708088](Problem & Contest.assets/image-20220713122708088.png)



![image-20220713122801360](Problem & Contest.assets/image-20220713122801360-16576865588091.png)

![image-20220824155547957](Problem & Contest.assets/image-20220824155547957.png)

   【使....最大值尽可能小】是二分搜索题目常见的问法



记 c(x) 为 x的二进制表示中的 1 的个数，则有如下等式：
c(x|y)+c(x\&y)=c(x)+c(y)
x|y+x&y=x+y



![image-20220811175634473](Problem & Contest.assets/image-20220811175634473.png)

## 待做题

1. round E 2020 Toys
2. round b 2019 Diverse Subarray

# 笔试题

![image-20220827222708594](Problem & Contest.assets/image-20220827222708594.png)

# 关于工作

## 关于简历

**根据岗位定制简历**，用数字描述，比如GPA，关键信息加粗，关键词开头，句子描述，简历字体字号，简历精简并吸引眼球，项目达成了什么效果

STAR模式（还可以用于叙述故事）

![图3](Problem & Contest.assets/format,png.png)

## 关于面试

1. **了解公司的企业文化，公司的历史，岗位要求硬技能、软技能，面试官的背景（领英），准备给面试官的提问。**包括但不限于工作强度，待遇，晋升机会，上升渠道，发展前景，培养模式，公司的氛围，公司会对我怎么培养，我能为公司做点什么，团队的发展，职业发展路线，面试官对岗位的预期。职业描述中已经有的信息就不要问了。
2. **中英文自我介绍**，在自我介绍中用\**工具**语言做了什么（根据岗位要求），准备一些专业基础知识，挑选几个适合岗位的案例比较重要
3. 面试时**问清题目**至关重要。许多面试官在面试的时候，会故意先抛出一个模糊的问题。实际上，他们希望面试者能够经过一些询问理解问题。在这个过程中，面试者能够展现出自己对**问题的分析能力**以及**沟通的能力**
4. 确认了题目之后，我认为合理的做法是先和面试官**确认函数签名**，也即输入是什么参数，输出是什么参数等等。
5. 和面试官**确认思路**
6. 确认边界处理
7. 使用可读性较高的变量名和函数名
8. 写代码过程中不断和面试官交流
9. 写完代码主动测试（如何白板上写代码可以用鼠标光标指示运行到了哪一步）
10. 主动给出算法的复杂度
11. 如果在某些基础问题上自己有一些实际经验，那么**可以结合自己的经验来回答**，这样会让面试官觉得这个候选人不仅基础扎实、经验丰富，而且学以致用、分析问题的能力也挺强的。

    这里举一个简单的例子，比如面试官问hash table处理冲突有哪些常用的方式，各有什么优缺点。那么可以回答常用的有线性探测和链地址两种。如果自己有相应的经验，那么就可以结合经验谈谈优缺点，例如线性探测在实际使用的时候常常需要空间开得比较大，hash table的装载因子需要维持一个一直比较小的状态（比如25%-50%这样），否则的话性能就会很差，因为查询和插入都会频繁地进行长距离的线性探测。而拉链法对空间的利用效率就会比较高。在提供足够的空间的时候，按经验线性探测会比拉链法快很多，比如之前做了个项目，在满足空间条件的时候线性探测会快7倍左右（这是在结合经验谈），原因是线性探测比链接法对cache更友好（这是基础知识）。



**英文介绍：**

Hello, sir/madam. I’m so grateful to be here for this interview. My name is Chen Ziyang, a first-year graduate student in Huazhong University of science and technology now. My major is computer science. I have good learning ability, coding ability, and love to communicate with other people. I think when I face a new technology or a new programming language, I can master it very quickly. I confirm that I'm a good fit for the Microsoft Explore position. By the way, I often use Word and OneNote to take notes, and OneDrive can help me sync files, which are closely connected with my life. So Microsoft is always my dream company. I want to work with the best engineers in the world and develop products that will be used by the world. which is an exciting and amazing work for me. Maybe my spoken English isn't very good at present, but I believe that if I come here for an internship, I can greatly improve my speaking ability. Thank you for the opportunity.

 面试官您好，我叫陈子阳，是华中科技大学研一的学生，我的专业是计算机科学与技术。非常感谢贵公司能给我这次机会来参加面试。我一直也算一个微软的小粉丝，我经常用Word和Onenote来记笔记，而且OneDrive能很方便的帮我同步这些内容。我的工作和生活已经离不开微软的产品。我觉得我比较适合贵公司Explore这个岗位。我认为我的优点是有较强的学习能力，当我遇到一个新的技术或者是新的编程语言的时候，我能很快掌握它，我一直非常坚信这一点。同时我有较强的编程能力，而且也热爱与人沟通来解决问题。微软一直是我梦想中的公司，我很想在这里和世界上最好的工程师做同事，能够和大家一起创造出能让世界都能用的产品，并且我也一直在为此奋斗。谢谢。

 遇到最大的困难：沟通问题，团队协同工作的问题
比方说，把“对工作负责”这个优点包装成“对自己和他人要求过高”，这种的回答方式明着看是缺点，实则是在告诉对方，你是严于律己、对工作认真负责的人。
同样的，你可以说自己遇事会反复考虑后再去做，导致不能及时作出决定，看似缺点，实则是让对方知道，你是一个善于思考，做事谨慎的人，符合这个岗位的要求。

 

**Turning Circle**

在那个时候跳一跳比较火，我自己也在玩，那个时候我就想这能不能自己设计一款游戏。
主要使用的语言是JavaScript和TypeScript
说出自己游戏的玩法。
游戏的所需的四个场景
加载界面（**如何设计**，并开始播放音乐）
开始界面（点击按钮开始游戏）
游戏界面（两个小球可以在一个中心点固定半径的圆上相反方向旋转，我们通过点击屏幕和**长按屏幕**来实现旋转来躲避障碍物，每躲避一个记1分。利用线程池生成障碍物，为什么用线程池。如何生成障碍物，让障碍物“下落”给人小球在前进的感觉。玩家分越高障碍物生成得越快。）
结束界面（游戏的分数，最高得分（哈希表），所有玩家的最高得分的排行榜（小顶堆，大小为10），点击按钮再玩一次）

游戏的缺陷：
1.障碍物设计的不够巧妙，就是游戏玩法上有待改进。
2.游戏设计的不够精美

 

**面试问题分析：**
1.说话紧张
2.项目也应该很清晰的讲出，算法题讲述的时候条理不清晰，比如对于一道题一开始就跟面试官清晰的讲述出思路，代码也要写的有条理，最后应该主动分析下时空复杂度。
3.leader面回答的不够好，比如学过什么课程，怎么学习一门新技术什么的都回答的不够好（看官方文档，需要什么都可以去去官方文档里面搜）。

![6](Problem & Contest.assets/6.png)

写代码的时候可以加点轻量的注释，写完后可以主动要求测试

**面试技巧：**
1.学会展现自己的优点，也要说出自己的弱点（但也要说自己做了那些事来改善自己的弱点）
2.要表现自己的热情
3.专注于这个工作需要的能力
4.会说故事（STAR method：Situation Task Action Result）

## 面经

**freewheel**

![image-20220713150817464](Problem & Contest.assets/image-20220713150817464.png)

大部分看对计算机基础知识的理解能力 **计组、操作系统、计算机网络、数据库。**还有语言类的问题，比如C++的STL和Java容器JVM。

freewheel的英文仅限于普通的交流，面试官不说，就还是拿中文回答，问一些日常，比如**英文自我介绍，介绍一下最喜欢的课程，介绍家乡**

学姐被面的场景题时如何设计一个数据库同步器，语言速成。12月Java基础，1、2月陆续刷一些leetcode简单题，3、4、5月学一些JavaWeb Spring数据库，7月背八股（有点晚了，错过了提前批，提前批是七月份）。项目就是用SpringBoot自己做了个烂大街的秒杀项目

项目简单介绍之后是聊天，为什么投他们公司，手上有什么offer，职业规划等等