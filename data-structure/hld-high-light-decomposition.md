# HLD / High Light Decomposition

```cpp
#include <bits/stdc++.h>
using namespace std;
const int MAX_N = 1e5+1;
int nbNoeuds,nbRequetes;
vector<vector<int>> adj(MAX_N);
int next_heavy[MAX_N];
int peres[17][MAX_N], depth[MAX_N]; // lca
int in[MAX_N], out[MAX_N];
int subtree[MAX_N];
int segt[4*MAX_N];
int timer = 0;
void pre_calc(){ // pour le lca
   for (int i=1; i<17; ++i){
      for (int j=0; j<nbNoeuds; ++j){
         peres[i][j] = peres[i-1][peres[i-1][j]];
      }
   }
}
int lca(int a, int b){
   if (depth[a] > depth[b]) swap(a,b);
   int diff = depth[b]-depth[a];
   for (int i=16; i>=0; --i){
      if (diff&(1<<i)){
         b = peres[i][b];
      }
   }
   if (a == b) return a;
   for (int i=16; i>=0; --i){
      if (peres[i][a] != peres[i][b]){
         a = peres[i][a];
         b = peres[i][b];
      }
   }
   return peres[0][a];
}
void lca_dfs(int node, int prev, int d){
   depth[node] = d;
   for (int i:adj[node]){
      if (i != prev){
         peres[0][i] = node;
         lca_dfs(i,node,d+1);
      }
   }
}
void modify(int l, int r){
   l += timer;
   r += timer;
   while (l <= r){
      if (l&1){
         segt[l]++;
         l++;
      }
      if (r%2 == 0){
         segt[r]++;
         r--;
      }
      l >>= 1;
      r >>= 1;
   }
}
int query(int a){
   a += timer;
   int sum = 0;
   while (a){
      sum += segt[a];
      a >>= 1;
   }
   return sum;
}
void dfs_size(int node, int prev){
   subtree[node] = 1;
   for (int& i:adj[node]){
      if (i != prev){
         dfs_size(i,node);
         subtree[node] += subtree[i];
         if (subtree[i] > subtree[adj[node][0]]){
            swap(adj[node][0],i);
         }
      }
   }
}
void dfs_hld(int node, int prev){
   in[node] = timer;
   timer++;
   for (int i:adj[node]){
      if (i != prev){
         next_heavy[i] = i;
         if (i == adj[node][0]) next_heavy[i] = next_heavy[node];
         dfs_hld(i,node);
      }
   }
   out[node] = timer;
}
int main(){
   ios::sync_with_stdio(false);
   cin.tie(nullptr);
   cout.tie(nullptr);
   cin >> nbNoeuds >> nbRequetes;
   for (int i=1; i<nbNoeuds; ++i){
      int a,b;
      cin >> a >> b;
      a--; b--;
      adj[a].emplace_back(b);
      adj[b].emplace_back(a);
   }
   lca_dfs(0,-1,0);
   pre_calc();
   dfs_size(0,-1);
   dfs_hld(0,-1);
   for (int i=0; i<nbRequetes; ++i){
      char type;
      cin >> type;
      if (type == 'Q'){
         int a,b;
         cin >> a >> b;
         a--; b--;
         if (depth[a] < depth[b]) swap(a,b);
         cout << query(in[a]) << '\n';
      }
      else{
         int a,b;
         cin >> a >> b;
         a--; b--;
         int ancetre = lca(a,b);
         while (in[ancetre] <= in[a] and out[ancetre] >= out[a]){
            if (ancetre == a) break;
            modify(max(in[ancetre]+1,in[next_heavy[a]]),in[a]);
            a = peres[0][next_heavy[a]];
         }
         while (in[ancetre] <= in[b] and out[ancetre] >= out[b]){
            if (ancetre == b) break;
            modify(max(in[ancetre]+1,in[next_heavy[b]]),in[b]);
            b = peres[0][next_heavy[b]];
         }
      }
   }
   return 0;
}
```

{% embed url="https://www.spoj.com/problems/QTREE/" %}

```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 10005;
const int root = 0;
int logN,chainNo,pos;
int depth[N],subsz[N],lowEnd[N];
int chainIdx[N],posInBase[N],base[N];
int segt[4*N+5],qt[4*N+5];
vector<vector<int>> parent;
vector<int> adj[N],cost[N],edgeIdx[N];
vector<int> chainHead;
inline void dfs(int node,int pere,int dep){
  parent[0][node] = pere;
  depth[node] = dep;
  subsz[node] = 1;
  for(int i = 0; i < adj[node].size(); ++i){
    if(adj[node][i] == pere) continue;
    lowEnd[edgeIdx[node][i]] = adj[node][i];
    dfs(adj[node][i],node,dep+1);
    subsz[node] += subsz[adj[node][i]];
  }
}
inline void HLD(int node,int icost,int pere){
  if(chainHead[chainNo] == -1) chainHead[chainNo] = node;
  chainIdx[node] = chainNo;
  posInBase[node] = pos;
  base[pos] = icost;
  ++pos;
  int schild = -1,newcost;
  for(int i = 0; i < adj[node].size(); ++i){
    if(adj[node][i] == pere) continue;
    if(schild == -1 or subsz[schild] < subsz[adj[node][i]]){
      schild = adj[node][i];
      newcost = cost[node][i];
    }
  }
  if(schild != -1) HLD(schild,newcost,node);
  for(int i = 0; i < adj[node].size(); ++i){
    if(adj[node][i] == pere) continue;
    if(adj[node][i] == schild) continue;
    ++chainNo;
    HLD(adj[node][i],cost[node][i],node);
  }
}
inline void makeTree(int node,int b,int e){
  if(b+1 == e) segt[node] = base[b];
  else{
    int mid = (b+e)/2;
    makeTree(2*node,b,mid);
    makeTree(2*node+1,mid,e);
    segt[node] = max(segt[2*node],segt[2*node+1]);
  }
}
inline int LCA(int u,int v){
  if(depth[u] < depth[v]) swap(u,v);
  int d = depth[u]-depth[v];
  for(int i = 0; i < logN; ++i){
    if(d&(1<<i)){
      u = parent[i][u];
      d ^= 1<<i;
      if(d == 0) break;
    }
  }
  if(u == v) return u;
  for(int i = logN-1; i >= 0; --i){
    if(parent[i][u] != parent[i][v]){
      u = parent[i][u];
      v = parent[i][v];
    }
  }
  return parent[0][u];
}
inline void queryTree(int node,int left,int right,int b,int e){
  if(e <= left or right <= b){
    qt[node] = -1;
    return;
  }
  if(b <= left and right <= e){
    qt[node] = segt[node];
    return;
  }
  int mid = (left+right)/2;
  queryTree(2*node,left,mid,b,e);
  queryTree(2*node+1,mid,right,b,e);
  qt[node] = max(qt[2*node],qt[2*node+1]);
}
inline int queryUp(int u,int v){ // v is ancestor of u
  if(u == v) return 0;
  int uchain,vchain = chainIdx[v],ans = -1;
  while(true){
    uchain = chainIdx[u];
    if(uchain == vchain){
      if(u != v){
        queryTree(1,0,pos,posInBase[v]+1,posInBase[u]+1);
        ans = max(ans,qt[1]);
      }
      break;
    }
    queryTree(1,0,pos,posInBase[chainHead[uchain]],posInBase[u]+1);
    ans = max(ans,qt[1]);
    u = parent[0][chainHead[uchain]];
  }
  return ans;
}
inline void query(int u,int v){
  int lca = LCA(u,v);
  cout << max(queryUp(u,lca),queryUp(v,lca)) << '\n';
}
inline void updateTree(int node,int left,int right,int x,int val){
  if(x < left or right <= x) return;
  if(left+1 == right and left == x){
    segt[node] = val;
    return;
  }
  int mid = (left+right)/2;
  updateTree(2*node,left,mid,x,val);
  updateTree(2*node+1,mid,right,x,val);
  segt[node] = max(segt[2*node],segt[2*node+1]);
}
inline void update(int edge,int val){
  int u = lowEnd[edge];
  updateTree(1,0,pos,posInBase[u],val);
}
int main(){
  ios::sync_with_stdio(0);
  cin.tie(0);
  int t;
  cin >> t;
  while(t--){
    int n,u,v,c;
    cin >> n;
    // init
    pos = 0;
    logN = int(log2(n))+1;
    chainNo = 0;
    parent = vector<vector<int>>(logN,vector<int>(n,-1));
    chainHead = vector<int>(n,-1);
    for(int i = 0; i < n; ++i){
      adj[i].clear();
      cost[i].clear();
      edgeIdx[i].clear();
    }
    // input
    for(int i = 0; i < n-1; ++i){
      cin >> u >> v >> c;
      --u;
      --v;
      adj[u].push_back(v);
      cost[u].push_back(c);
      edgeIdx[u].push_back(i);
      adj[v].push_back(u);
      cost[v].push_back(c);
      edgeIdx[v].push_back(i);
    }
    // set up subtree size, depth and pere
    dfs(root,-1,0);
    // DP to prepare LCA
    for(int i = 1; i < logN; ++i){
      for(int j = 0; j < n; ++j){
        int prev = parent[i-1][j];
        if(prev >= 0) parent[i][j] = parent[i-1][prev];
      }
    }
    // High Light Decomposition
    HLD(root,-1,-1);
    // build underlying segment tree
    makeTree(1,0,pos);
    string cmd;
    int a,b,i,t;
    while(true){
      cin >> cmd;
      if(cmd[0] == 'D') break;
      if(cmd[0] == 'Q'){
        cin >> a >> b;
        query(a-1,b-1);
      }
      else{
        cin >> i >> t;
        update(i-1,t);
      }
    }
  }
  return 0;
}
```

{% embed url="https://www.spoj.com/problems/QTREE3/" %}

The solution below get only 41.67 points over 100. Any idea ?

```cpp
#include <bits/stdc++.h>
using namespace std;
using ll = long long;
const ll N = 1000005;
ll chainNo;
vector<ll> parent;
ll key[N],subsz[N],chainSz[N],chainHead[N],chainIdx[N],posInChain[N];
vector<ll> adj[N];
set<ll> chainSet[N];
inline ll getKey(ll pos,ll node){
  return (pos<<20)+node;
}
inline pair<ll,ll> getVal(ll x){
  return {x>>20,x&((1<<20)-1)};
}
inline void dfs(ll node,ll pere){
  parent[node] = pere;
  subsz[node ] = 1;
  for(ll i : adj[node]){
    if(i == pere) continue;
    dfs(i,node);
    subsz[node] += subsz[i];
  }
}
inline void HLD(ll node,ll pere){
  if(chainHead[chainNo] == -1) chainHead[chainNo] = node;
  chainIdx[node] = chainNo;
  posInChain[node] = chainSz[chainNo];
  ++chainSz[chainNo];
  ll schild = -1;
  for(ll i : adj[node]){
    if(i == pere) continue;
    if(schild == -1 or subsz[schild] < subsz[i]){
      schild = i;
    }
  }
  if(schild != -1) HLD(schild,node);
  for(ll i : adj[node]){
    if(i == pere) continue;
    if(i == schild) continue;
    ++chainNo;
    HLD(i,node);
  }
}
int main(){
  ll n,q,u,v,cmd;
  scanf("%lld%lld",&n,&q);
  if(n == 0){
    while(q--){
      scanf("%lld%lld",&cmd,&u);
      cout << "-1\n";
    }
    return 0;
  }
  chainNo = 0;
  parent = vector<ll>(n,-1);
  for(ll i = 0; i < n-1; ++i){
    scanf("%lld%lld",&u,&v);
    --u;
    --v;
    adj[u].push_back(v);
    adj[v].push_back(u);
  }
  dfs(0,-1);
  HLD(0,-1);
  for(ll i = 0; i < n; ++i){
    key[i] = getKey(posInChain[i],i);
  }
  while(q--){
    scanf("%lld%lld",&cmd,&u);
    --u;
    if(cmd == 0){
      set<ll>& st = chainSet[chainIdx[u]];
      if(st.count(key[u])) st.erase(key[u]);
      else st.insert(key[u]);
    }
    else{
      bool ok = false;
      ll x,y,ans;
      while(true){
        ll uchain = chainIdx[u];
        if(!chainSet[uchain].empty()){
          tie(x,y) = getVal(*begin(chainSet[uchain]));
          if(x <= posInChain[u]){
            ans = y+1;
            ok = true;
          }
        }
        u = chainHead[uchain];
        if(u == 0) break;
        u = parent[u];
      }
      if(ok) printf("%lld\n",ans);
      else printf("-1\n");
    }
  }
  return 0;
}
```

A simple implementation : [https://codeforces.com/blog/entry/53170](https://codeforces.com/blog/entry/53170)

```cpp
void dfs_sz(int v = 0) {
    sz[v] = 1;
    for(auto &u: g[v]) {
        dfs_sz(u);
        sz[v] += sz[u];
        if(sz[u] > sz[g[v][0]]) {
            swap(u, g[v][0]);
        }
    }
}

void dfs_hld(int v = 0) {
    in[v] = t++;
    for(auto u: g[v]) {
        nxt[u] = (u == g[v][0] ? nxt[v] : u);
        dfs_hld(u);
    }
    out[v] = t;
}
```

Then you will have such array that subtree of v correspond to segment \[in\_v, out\_v\) and the path from v to the last vertex in ascending heavy path from v \(which is nxt\_v\) will be \[in\_{nxt\_v}, in\_v\] subsegment which gives you the opportunity to process queries on paths and subtrees simultaneously in the same segment tree.

