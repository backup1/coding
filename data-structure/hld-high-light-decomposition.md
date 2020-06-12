# HLD / High Light Decomposition

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

