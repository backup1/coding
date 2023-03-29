# Binary Lifting

#### JOI 2013 Synchronisation

editorial : [https://bits-and-bytes.me/2020/01/05/JOI-2013-Synchronisation/](https://bits-and-bytes.me/2020/01/05/JOI-2013-Synchronisation/)

```cpp
#include <bits/stdc++.h>
#define FOR(i, x, y) for (int i = x; i < y; i++)
typedef long long ll;
using namespace std;

int n, m, q;
bool active[100001];
vector<int> graph[100001];
pair<int, int> edges[200001];

int info[100001], last_sync[100001];

// DFS order
int timer = 1, tin[100001], tout[100001];
// Binary lifting parents
int anc[100001][20];

void dfs(int node = 1, int parent = 0) {
    anc[node][0] = parent;
    for (int i = 1; i < 20 && anc[node][i - 1]; i++) {
        anc[node][i] = anc[anc[node][i - 1]][i - 1];
    }

    info[node] = 1;

    tin[node] = timer++;
    for (int i : graph[node]) if (i != parent) dfs(i, node);
    tout[node] = timer;
}

// Fenwick tree
int bit[100001];

void update(int pos, int val) { for (; pos <= n; pos += (pos & (-pos))) bit[pos] += val; }

int query(int pos) {
    int ans = 0;
    for (; pos; pos -= (pos & (-pos))) ans += bit[pos];
    return ans;
}

// Binary lifting
int find_ancestor(int node) {
    int lca = node;
    for (int i = 19; ~i; i--) {
        if (anc[lca][i] && query(tin[anc[lca][i]]) == query(tin[node])) lca = anc[lca][i];
    }
    return lca;
}

int main() {
    ios_base::sync_with_stdio(0);
    cin.tie(0);
    cin >> n >> m >> q;
    FOR(i, 1, n) {
        cin >> edges[i].first >> edges[i].second;
        graph[edges[i].first].push_back(edges[i].second);
        graph[edges[i].second].push_back(edges[i].first);
    }
    dfs();

    FOR(i, 1, n + 1) {
        update(tin[i], -1);
        update(tout[i], 1);
    }

    while (m--) {
        int x;
        cin >> x;
        int u = edges[x].first, v = edges[x].second;
        if (anc[u][0] == v) swap(u, v);

        if (active[x]) {
            info[v] = last_sync[v] = info[find_ancestor(u)];
            update(tin[v], -1);
            update(tout[v], 1);
        } else {
            info[find_ancestor(u)] += info[v] - last_sync[v];
            update(tin[v], 1);
            update(tout[v], -1);
        }
        active[x] = !active[x];
    }

    while (q--) {
        int x;
        cin >> x;
        cout << info[find_ancestor(x)] << '\n';
    }
    return 0;
}
```

#### APIO 2009 Convention Center [https://www.acmicpc.net/problem/4012](https://www.acmicpc.net/problem/4012)

Luogu : [https://www.luogu.com.cn/problem/P3626](https://www.luogu.com.cn/problem/P3626)

```cpp
#include <bits/stdc++.h>
using namespace std;
const int inf = INT_MAX/2;
const int N = 2e5+5;
int n,top;

int pos[N],seg[N],nxtIdx[N][25],ans[N];

struct Seg {
  int l,r,id;
  bool operator< (const Seg& a) const {
    if(l != a.l) return l < a.l;
    return r > a.r;
  }
} v[N];

set<Seg> S;

int calc(int x,int y){
  if(x > y) return 0;
  // binary search for the first seg #l with left in [x,y]
  int l = 1, r = top;
  while(l < r){
    int mid = (l+r)/2;
    if(v[seg[mid]].l < x) l = mid+1;
    else r = mid;
  }
  if(v[seg[l]].l < x) return 0;
  // calculate the number of seg in [x,y]
  int tot = 0;
  for(int i = 20; i >= 0; --i){
    if(nxtIdx[l][i] > 0 and v[seg[nxtIdx[l][i]]].r <= y){
      tot += (1<<i);
      l = nxtIdx[nxtIdx[l][i]][1];
    }
  }
  return tot;
}

int main(){
  ios::sync_with_stdio(0);
  cin.tie(0);

  cin >> n;
  int a,b;
  for(int i = 1; i <= n; ++i){
    cin >> a >> b;
    v[i] = Seg{a,b,i};
  }
  // trier les segments du plus petit x au plus grand,
  // puis du plus grand y au plus petit
  sort(v+1,v+n+1);
  // enregistrer position de segment id
  for(int i = 1; i <= n; ++i) pos[v[i].id] = i;
  // enlever les segments contenant les segments plus petits
  top = 0;
  // stack
  for(int i = 1; i <= n; ++i){
    while(top > 0 and v[seg[top]].r > v[i].r) --top;
    seg[++top] = i;
  }
  // nxtIdx[i][j] : Chosen seg #i, which will be the 2^j-th seg ?
  // nxtIdx[i][j] = nxtIdx[nxtIdx[nxtIdx[i][j-1]][1]][j-1]
  // i (+2^j-1) = i (+2^{j-1}-1) (+1) (+2^{j-1}-1)
  int crt = 1;
  // two pointers
  for(int i = 1; i <= top; ++i){
    nxtIdx[i][0] = i;
    while(v[seg[crt]].r < v[seg[i]].l){
      nxtIdx[crt][1] = i;
      ++crt;
    }
  }
  for(int i = 2; i <= 20; ++i){
    for(int j = 1; j <= top; ++j){
      nxtIdx[j][i] = nxtIdx[nxtIdx[nxtIdx[j][i-1]][1]][i-1];
    }
  }
  // start to create the solution by index order
  int cnt = 0;
  for(int i = 0; i <= n; ++i){
    int l = v[pos[i]].l, r = v[pos[i]].r, tot = 1, L, R;
    auto it = S.lower_bound(v[pos[i]]);
    if(it == end(S)) L = inf;
    else L = (*it).l;
    if(L <= r) continue; // not a solution
    if(it == begin(S)) R = 0;
    else{
      --it;
      R = (*it).r;
    }
    if(R >= l) continue; // not a solution
    tot += calc(r+1,L-1);
    tot += calc(R+1,l-1);
    if(tot == calc(R+1,L-1)){
      ++cnt;
      ans[cnt] = i;
      S.emplace(v[pos[i]]);
    }
  }
  // afficher le resultat
  cout << cnt << '\n';
  for(int i = 1; i <= cnt; ++i) cout << ans[i] << ' ';
  return 0;
}
```

