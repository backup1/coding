# Link Cut Tree

LCT Tutorial : [https://oi-wiki.org/ds/lct/](https://oi-wiki.org/ds/lct/)

LCT code example to study : [https://judge.yosupo.jp/submission/13696](https://judge.yosupo.jp/submission/13696)

Splay Tree Tutorial : [https://oi-wiki.org/ds/splay/](https://oi-wiki.org/ds/splay/), [http://wcipeg.com/wiki/Size\_Balanced\_Tree](http://wcipeg.com/wiki/Size_Balanced_Tree), [https://codeforces.com/blog/entry/79524](https://codeforces.com/blog/entry/79524), tourist code example : [https://codeforces.com/contest/899/submission/44463457](https://codeforces.com/contest/899/submission/44463457)

**Splay tree** can be used to solve **Treap/Skip List/Cartesian Tree** questions \(both are BBSTs, i.e. Balanced Binary Search Tree\)

Treap problems \(try to solve them using splay\) : [https://codeforces.com/blog/entry/46479](https://codeforces.com/blog/entry/46479)

question 1 : [https://www.spoj.com/problems/ADALIST/](https://www.spoj.com/problems/ADALIST/)

question 2 \(easy\) : [https://www.spoj.com/problems/DYNACON1/](https://www.spoj.com/problems/DYNACON1/)

question 3 : [https://codeforces.com/problemset/problem/1344/E](https://codeforces.com/problemset/problem/1344/E)

question 4 \(medium\) : [http://www.spoj.com/problems/DYNALCA/](http://www.spoj.com/problems/DYNALCA/)

question 5 \(hard\) : [http://www.spoj.com/problems/QTREE6/](http://www.spoj.com/problems/QTREE6/)

Some not so trivial and practical:

* Timus [1553](http://acm.timus.ru/problem.aspx?space=1&num=1553). Caves and Tunnels
* SPOJ [4155](http://www.spoj.com/problems/OTOCI/). OTOCI
* liveArchive [5884](https://icpcarchive.ecs.baylor.edu/index.php?option=onlinejudge&page=show_problem&problem=3895). Strange Regulations
* Codeforces [117E](https://codeforces.com/problemset/problem/117/E). Tree or not Tree
* more : [https://www.codechef.com/tags/problems/link-cut-tree](https://www.codechef.com/tags/problems/link-cut-tree)

Example I : [https://www.luogu.com.cn/problem/P1501](https://www.luogu.com.cn/problem/P1501)

```cpp
#include <bits/stdc++.h>
using namespace std;
using ll = long long;
const ll maxn = 100010;
const ll mod = 51061;
struct Splay{
  ll ch[maxn][2], f[maxn], siz[maxn], val[maxn], sum[maxn], rev[maxn], add[maxn], mul[maxn];
  void clear(ll x){
    ch[x][0] = ch[x][1] = f[x] = siz[x] = val[x] = sum[x] = rev[x] = add[x] = 0;
    mul[x] = 1;
  }
  void pushup(ll x){
    clear(0);
    siz[x] = (siz[ch[x][0]]+1+siz[ch[x][1]]) % mod;
    sum[x] = (sum[ch[x][0]]+val[x]+sum[ch[x][1]]) % mod;
  }
  void pushdown(ll x){
    clear(0);
    if(mul[x] != 1){
      for(ll d : {0,1}){
        if(ch[x][d]){
          mul[ch[x][d]] = (mul[x]*mul[ch[x][d]]) % mod;
          val[ch[x][d]] = (mul[x]*val[ch[x][d]]) % mod;
          sum[ch[x][d]] = (mul[x]*sum[ch[x][d]]) % mod;
          add[ch[x][d]] = (mul[x]*add[ch[x][d]]) % mod;
        }
      }
      mul[x] = 1;
    }
    if(add[x]){
      for(ll d : {0,1}){
        if(ch[x][d]){
          add[ch[x][d]] = (add[x]+add[ch[x][d]]) % mod;
          val[ch[x][d]] = (add[x]+val[ch[x][d]]) % mod;
          sum[ch[x][d]] = (siz[ch[x][d]]*add[x]+sum[ch[x][d]]) % mod;
        }
      }
      add[x] = 0;
    }
    if(rev[x]){
      for(ll d : {0,1}){
        if(ch[x][d]){
          rev[ch[x][d]] ^= 1;
          swap(ch[ch[x][d]][0],ch[ch[x][d]][1]);
        }
      }
      rev[x] = 0;
    }
  }
  bool isroot(ll x){
    clear(0);
    return (ch[f[x]][0] != x) and (ch[f[x]][1] != x);
  }
  void update(ll x){
    if(!isroot(x)) update(f[x]);
    pushdown(x);
  }
  ll side(ll x){ return ch[f[x]][1] == x; }
  void rotate(ll x){
    ll y = f[x], z = f[y], chx = side(x), chy = side(y);
    if(!isroot(y)) ch[z][chy] = x;
    ch[y][chx] = ch[x][chx^1], f[ch[x][chx^1]] = y;
    ch[x][chx^1] = y, f[y] = x, f[x] = z;
    pushup(y), pushup(x), pushup(z);
  }
  void splay(ll x){
    update(x);
    for(; !isroot(x); rotate(x)){
      if(!isroot(f[x])) rotate(side(f[x]) == side(x) ? f[x] : x);
    }
  }
  void access(ll x){
    for(ll p = 0; x; p = x, x = f[x]) splay(x), ch[x][1] = p, pushup(x);
  }
  void makeroot(ll x){
    access(x);
    splay(x);
    swap(ch[x][0],ch[x][1]);
    rev[x] ^= 1;
  }
  ll find(ll x){
    access(x), splay(x);
    while(ch[x][0]) x = ch[x][0];
    splay(x);
    return x;
  }
} st;
int main(){
  ios::sync_with_stdio(0);
  cin.tie(0);
  ll n,q,u,v,c;
  char op;
  cin >> n >> q;
  for(ll i = 1; i <= n; ++i){
    st.val[i] = 1;
    st.pushup(i);
  }
  for(ll i = 1; i < n; ++i){
    cin >> u >> v;
    if(st.find(u) != st.find(v)) st.makeroot(u), st.f[u] = v;
  }
  while(q--){
    cin >> op >> u >> v;
    if(op == '+'){
      cin >> c;
      st.makeroot(u), st.access(v), st.splay(v);
      st.val[v] = (st.val[v]+c) % mod;
      st.sum[v] = (st.sum[v]+st.siz[v]*c) % mod;
      st.add[v] = (st.add[v]+c) % mod;
    } else if(op == '-'){
      st.makeroot(u), st.access(v), st.splay(v);
      if(st.ch[v][0] == u and !st.ch[u][1]) st.ch[v][0] = st.f[u] = 0;
      cin >> u >> v;
      if(st.find(u) != st.find(v)) st.makeroot(u), st.f[u] = v;
    } else if(op == '*'){
      cin >> c;
      st.makeroot(u), st.access(v), st.splay(v);
      st.val[v] = (st.val[v]*c) % mod;
      st.sum[v] = (st.sum[v]*c) % mod;
      st.mul[v] = (st.mul[v]*c) % mod;
    } else { // op == '/'
      st.makeroot(u), st.access(v), st.splay(v);
      cout << st.sum[v] << '\n';
    }
  }
  return 0;
}
```

Example II : [https://www.luogu.com.cn/problem/P3690](https://www.luogu.com.cn/problem/P3690)

```cpp
#include <bits/stdc++.h>
using namespace std;
const int maxn = 100010;
struct Splay{
  int ch[maxn][2], f[maxn], val[maxn], sum[maxn], rev[maxn];
  void clear(int x){
    ch[x][0] = ch[x][1] = f[x] = val[x] = sum[x] = rev[x] = 0;
  }
  void pushup(int x){
    clear(0);
    sum[x] = (sum[ch[x][0]]^val[x])^sum[ch[x][1]];
  }
  void pushdown(int x){
    clear(0);
    if(rev[x]){
      for(int d : {0,1}){
        if(ch[x][d]){
          rev[ch[x][d]] ^= 1;
          swap(ch[ch[x][d]][0],ch[ch[x][d]][1]);
        }
      }
      rev[x] = 0;
    }
  }
  bool isroot(int x){
    clear(0);
    return (ch[f[x]][0] != x) and (ch[f[x]][1] != x);
  }
  void update(int x){
    if(!isroot(x)) update(f[x]);
    pushdown(x);
  }
  int side(int x){ return ch[f[x]][1] == x; }
  void rotate(int x){
    int y = f[x], z = f[y], chx = side(x), chy = side(y);
    if(!isroot(y)) ch[z][chy] = x;
    ch[y][chx] = ch[x][chx^1], f[ch[x][chx^1]] = y;
    ch[x][chx^1] = y, f[y] = x, f[x] = z;
    pushup(y), pushup(x), pushup(z);
  }
  void splay(int x){
    update(x);
    for(; !isroot(x); rotate(x)){
      if(!isroot(f[x])) rotate(side(f[x]) == side(x) ? f[x] : x);
    }
  }
  void access(int x){
    for(int p = 0; x; p = x, x = f[x]) splay(x), ch[x][1] = p, pushup(x);
  }
  void makeroot(int x){
    access(x);
    splay(x);
    swap(ch[x][0],ch[x][1]);
    rev[x] ^= 1;
  }
  int find(int x){
    access(x), splay(x);
    while(ch[x][0]) x = ch[x][0];
    splay(x);
    return x;
  }
} st;
int main(){
  ios::sync_with_stdio(0);
  cin.tie(0);
  int n,m,a,op,x,y;
  cin >> n >> m;
  for(int i = 1; i <= n; ++i){
    cin >> a;
    st.sum[i] = st.val[i] = a;
  }
  while(m--){
    cin >> op >> x >> y;
    if(op == 0){
      st.makeroot(x), st.access(y), st.splay(y);
      cout << st.sum[y] << '\n';
    } else if(op == 1){
      if(st.find(x) != st.find(y)) st.makeroot(x), st.f[x] = y;
    } else if(op == 2){
      st.makeroot(x), st.access(y), st.splay(y);
      if(st.ch[y][0] == x and !st.ch[x][1]) st.ch[y][0] = st.f[x] = 0;
    } else{
      st.makeroot(x);
      st.sum[x] = st.sum[x] ^ st.val[x] ^ y;
      st.val[x] = y;
    }
  }
  return 0;
}
```

