# Simulated annealing

Chip design algorithm : https://www.geeksforgeeks.org/simulated-annealing/

### Questions 1. **\[LeetCode#1815]**

Questions 1. https://leetcode.com/problems/maximum-number-of-groups-getting-fresh-donuts/ https://leetcode-cn.com/problems/maximum-number-of-groups-getting-fresh-donuts/solution/

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

### Question 2. **\[UVA#10228]**

Question 2. **\[UVA#10228]** [https://onlinejudge.org/index.php?option=com\_onlinejudge\&Itemid=8\&page=show\_problem\&problem=1169](https://onlinejudge.org/index.php?option=com\_onlinejudge\&Itemid=8\&page=show\_problem\&problem=1169)

```cpp
#include <bits/stdc++.h>
using namespace std;
struct node{
  double x,y;
};
double calc(node& pt,vector<node>& v){
  double ret = 0;
  for(auto& item : v){
    double dx = item.x - pt.x, dy = item.y - pt.y;
    ret += sqrt(dx*dx+dy*dy);
  }
  return ret;
}
void simulate_annealing(int n,vector<node>& v){
  srand(time(NULL));
  node a;
  for(auto& item : v){
    a.x += item.x;
    a.y += item.y;
  }
  a.x /= n;
  a.y /= n;
  double best = calc(a,v);
  double ans = best, X = a.x, Y = a.y;
  double lr = 0.99;
  for(double t = 1e5; t > 1e-5; t *= lr){
    node b;
    b.x = a.x + t*(2*rand()-RAND_MAX)/RAND_MAX;
    b.y = a.y + t*(2*rand()-RAND_MAX)/RAND_MAX;
    double res = calc(b,v);
    if(best > res){
      best = res;
      X = b.x;
      Y = b.y;
    }
    if(ans > res or exp((ans-res)/t) > (double)rand()/RAND_MAX){
      ans = res;
      a = b;
    }
  }
  cout.precision(0);
  cout << fixed << best << '\n';
}
void play(){
  int n;
  cin >> n;
  vector<node> v(n);
  double ax = 0, ay = 0;
  for(int i = 0; i < n; ++i){
    cin >> v[i].x >> v[i].y;
  }
  simulate_annealing(n,v);
}
int main(){
  ios::sync_with_stdio(0);
  cin.tie(0);
  int T;
  cin >> T;
  while(T--){
    play();
    if(T) cout << '\n';
  }
  return 0;
}
```

### Question 3. **CEOI 2004 Two sawmills**

Question 1c/ **CEOI 2004 Two sawmills** [https://www.oi.edu.pl/old/ceoi2004/problems/two.pdf](https://www.oi.edu.pl/old/ceoi2004/problems/two.pdf)

submit : [https://www.luogu.com.cn/problem/P4360](https://www.luogu.com.cn/problem/P4360)

blog : [https://blog.csdn.net/qq\_35786326/article/details/109177830](https://blog.csdn.net/qq\_35786326/article/details/109177830)

```cpp
#include <bits/stdc++.h>
using namespace std;
using ll = long long;
vector<ll> w,d;
ll sumwd,n;
struct node{
  ll a,b;
  ll calc(){
  // \sum_{1..a} wi*(da-di) + \sum_{a+1..b} wi*(db-di) + \sum_{b+1..n} wi*(dn-di)
  // = da*\sum_{1..a} wi + db*\sum_{a+1..b} wi + dn*\sum_{b+1..n} wi - \sum_{1..n} wi*di
  // = da*psum_a + db*(psum_b-psum_a) + dn*(psum_n-psum_b) - \sum_{1..n} wi*di
    if(a > b) swap(a,b);
    return d[a]*w[a]+d[b]*(w[b]-w[a])+d[n+1]*(w[n]-w[b])-sumwd;
  }
};
int main(){
  ios::sync_with_stdio(0);
  cin.tie(0);
  srand(time(NULL));
  cin >> n;
  w.resize(n+5);
  d.resize(n+5);
  sumwd = 0;
  for(int i = 1; i <= n; ++i){
    cin >> w[i] >> d[i+1];
    sumwd += w[i]*d[i];
    d[i+1] += d[i];
    w[i] += w[i-1];
  }
  node best;
  best.a = 1;
  best.b = 1;
  double lr = 0.999;
  for(int i = 0; i < 100; ++i){
    node ans;
    ans.a = rand()%n + 1;
    ans.b = rand()%n + 1;
    for(double t = n; t > 0.5; t *= lr){
      node res;
      res.a = (ans.a+ll(round(t*(2.0*rand()/RAND_MAX-1))))%n;
      res.b = (ans.b+ll(round(t*(2.0*rand()/RAND_MAX-1))))%n;
      res.a = (res.a+n)%n + 1;
      res.b = (res.b+n)%n + 1;
      if(ans.calc() > res.calc() or exp((ans.calc()-res.calc())/t)*RAND_MAX > rand()){
        ans = res;
        if(best.calc() > ans.calc()) best = ans;
      }
    }
  }
  cout << best.calc();
  return 0;
}
```

### Question 4. **USACO 2017 January Platinum Subsequence Reversal**

Question 4. **USACO 2017 January Platinum Subsequence Reversal** : [http://www.usaco.org/index.php?page=viewproblem2\&cpid=698](http://www.usaco.org/index.php?page=viewproblem2\&cpid=698)

```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
const int N = 51;
int good[N], backup[N], a[N];
int main(){
    freopen("subrev.in", "r", stdin);
    freopen("subrev.out", "w", stdout);
    srand(time(NULL));
    ios::sync_with_stdio(0);
    cin.tie(0);
    int n;
    cin >> n;
    for(int i = 0; i < n; i++){
        good[i] = rand() % 2;
        cin >> a[i];
    }
    double T = 1e9, t = T;
    int best = 0;
    int ans = 0;
    while(clock() / (double) CLOCKS_PER_SEC <= 1.95){
        vector<int> p;
        for(int i = 0; i < n; ++i) backup[i] = good[i];
        for(int i = 0; i < n*t/T; ++i) good[rand()%n] ^= 1;
        for(int i = 0; i < n; ++i){
            if(good[i]) p.push_back(i);
        }
        int sz = p.size();
        for(int i = 0; (i<<1) < sz; ++i) swap(a[p[i]], a[p[sz-1-i]]);
        vector<int> v;
        for(int i = 0; i < n; ++i){
            if(v.empty() or a[i] >= v.back()) v.push_back(a[i]);
            else{
                auto it = upper_bound(begin(v),end(v),a[i]);
                *it = a[i];
            }
        }
        int res = v.size();
        best = max(best, res);
        for(int i = 0; (i<<1) < sz; ++i) swap(a[p[i]], a[p[sz-1-i]]);
        if(res > ans or exp((res-ans)/t)*RAND_MAX > rand()) ans = res;
        else{
            for(int i = 0; i < n; ++i) good[i] = backup[i];
        }
        t *= 0.9999;
    }
    cout << best << '\n';
}
```

### Question 5. P1337 \[JSOI2004]平衡点 / 吊打XXX

Question 5. P1337 \[JSOI2004]平衡点 / 吊打XXX [https://www.luogu.com.cn/problem/P1337](https://www.luogu.com.cn/problem/P1337)

```cpp
#include <bits/stdc++.h>
using namespace std;
using ld = long double;
int n;
struct pt{
  ld x,y,w;
} pts[1005];
ld calc(ld a,ld b){
  ld ret = 0;
  for(int i = 0; i < n; ++i){
    ld x = pts[i].x, y = pts[i].y, w = pts[i].w;
    ret += w * sqrt((x-a)*(x-a)+(y-b)*(y-b));
  }
  return ret;
}
int main(){
  ios::sync_with_stdio(0);
  cin.tie(0);
  srand(time(NULL));
  cin >> n;
  ld bx = 0, by = 0;
  for(int i = 0; i < n; ++i){
    cin >> pts[i].x >> pts[i].y >> pts[i].w;
    bx += pts[i].x;
    by += pts[i].y;
  }
  bx /= n;
  by /= n;
  ld best = calc(bx,by);
  cout.precision(3);
  while(clock() <= 0.95*CLOCKS_PER_SEC){
    ld ans = best, x = bx, y = by;
    for(ld t = 1e9; t > 1e-5; t*= 0.99){
      ld nx = x + t*(2*rand()-RAND_MAX)/RAND_MAX;
      ld ny = y + t*(2*rand()-RAND_MAX)/RAND_MAX;
      ld res = calc(nx,ny);
      if(best > res){
        best = res;
        bx = nx;
        by = ny;
      }
      if(ans > res or exp((ans-res)/t)*RAND_MAX > rand()){
        ans = res;
        x = nx;
        y = ny;
      }
    }
  }
  cout.precision(3);
  cout << fixed << bx << ' ' << by;
  return 0;
}
```

2/ https://www.luogu.com.cn/problem/P1337 https://www.cnblogs.com/flashhu/p/8900466.html https://www.cnblogs.com/peng-ym/p/9189390.html

3/ https://www.cnblogs.com/peng-ym/p/9192203.html

4/ blogs https://www.cnblogs.com/flashhu/p/8884132.html https://www.cnblogs.com/peng-ym/p/9158909.html https://m-sea.blog.luogu.org/qian-tan-SA https://zhuanlan.zhihu.com/p/23968011 https://cardiffle.github.io/2019/08/05/%E6%A8%A1%E6%8B%9F%E9%80%80%E7%81%ABSA/ [https://blog.csdn.net/crazyhacking/article/details/8715932](https://blog.csdn.net/crazyhacking/article/details/8715932?spm=1001.2101.3001.4242.1)

5/ English blogs https://codeforces.com/blog/entry/94437 http://www.usaco.org/index.php?page=viewproblem2\&cpid=698 https://codeforces.com/contest/1556/problem/H https://atcoder.jp/contests/intro-heuristics/tasks/intro\_heuristics\_a

Hill Climb : https://codeforces.com/contest/723/problem/E solution : https://codeforces.com/blog/entry/57437

除了模拟退火外，还有不少随机化算法。比如遗传算法、蚁群算法，这些算法被称为“元启发算法”，有兴趣的读者可以查阅相关资料。

blog : [https://blog.csdn.net/weixin\_42604241/article/details/97509168](https://blog.csdn.net/weixin\_42604241/article/details/97509168?spm=1001.2101.3001.6650.3\&depth\_1-utm\_source=distribute.pc\_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7ERate-3.pc\_relevant\_default)

1. 数值概率算法
2. 蒙特卡罗（Monte Carlo）算法
3. 拉斯维加斯（Las Vegas）算法
4. 舍伍德（Sherwood）算法
