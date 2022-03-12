# Simulated annealing

Chip design algorithm

https://www.geeksforgeeks.org/simulated-annealing/

Questions 1/ https://leetcode.com/problems/maximum-number-of-groups-getting-fresh-donuts/ https://leetcode-cn.com/problems/maximum-number-of-groups-getting-fresh-donuts/solution/

```cpp
class Solution {
    int res;
public:
    int maxHappyGroups(int batchSize, vector<int>& groups) {
        // simulated annealing
        // 1/ get result for x, then make y by randomly swap 2 positions and get result for y
        // 2a/ if get better result, keep y
        // 2b/ or if exp(delta/t) > a random number between 0 and 1, then keep y
        int nbIteration = 36;
        res = 0; // global result, keep updating
        if(batchSize == 1) return groups.size(); // trivial case
        int already_ok = 0; // put multiple of batchSize at the beginning of the queue is always optimal
        vector<int> groups2;
        for(int g: groups){
            g %= batchSize;
            if(g == 0) ++already_ok;
            else groups2.push_back(g); // speedup by considering only the reminder
        }
        int n = groups2.size();
        if(n <= 1) return already_ok + n;
        for(int i = 0; i < nbIteration; ++i) simulate_anneal(batchSize,groups2);
        return res + already_ok;
    }
    void simulate_anneal(int batchSize, vector<int>& groups){
        int n = groups.size();
        double lr = 0.991; // lowering rate
        random_shuffle(begin(groups),end(groups));
        for(double t = 1e5; t > 1e-5; t *= lr){
            int a = rand()%n, b = rand()%n;
            int x = calc(batchSize,groups);
            swap(groups[a], groups[b]);
            int y = calc(batchSize,groups);
            int delta = y - x;
            if(y < x and exp(delta/t) <= (double)rand()/RAND_MAX) swap(groups[a], groups[b]);
        }
    }
    int calc(int batchSize, vector<int>& groups){
        int happy_group = 0;
        int cur_sum = 0;
        for(int i : groups){
            cur_sum = (cur_sum + i)%batchSize;
            if(cur_sum == 0) ++happy_group;
        }
        if(cur_sum > 0) ++happy_group;
        res = max(res, happy_group);
        return happy_group;
    }
};
```

2/ https://www.luogu.com.cn/problem/P1337 https://www.cnblogs.com/flashhu/p/8900466.html https://www.cnblogs.com/peng-ym/p/9189390.html

3/ https://www.cnblogs.com/peng-ym/p/9192203.html

4/ blogs https://www.cnblogs.com/flashhu/p/8884132.html https://www.cnblogs.com/peng-ym/p/9158909.html https://m-sea.blog.luogu.org/qian-tan-SA https://zhuanlan.zhihu.com/p/23968011 https://cardiffle.github.io/2019/08/05/%E6%A8%A1%E6%8B%9F%E9%80%80%E7%81%ABSA/

5/ English blogs https://codeforces.com/blog/entry/94437 http://www.usaco.org/index.php?page=viewproblem2\&cpid=698 https://codeforces.com/contest/1556/problem/H https://atcoder.jp/contests/intro-heuristics/tasks/intro\_heuristics\_a

Hill Climb : https://codeforces.com/contest/723/problem/E solution : https://codeforces.com/blog/entry/57437

除了模拟退火外，还有不少随机化算法。比如遗传算法、蚁群算法，这些算法被称为“元启发算法”，有兴趣的读者可以查阅相关资料。S
