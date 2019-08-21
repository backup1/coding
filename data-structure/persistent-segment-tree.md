# Persistent Segment Tree

{% embed url="https://www.spoj.com/problems/MKTHNUM/" %}

```cpp
#include <bits/stdc++.h>
using namespace std;
struct node{
  int count;
  node *left, *right;
  node() : count(0),left(nullptr),right(nullptr) {}
  node(int c,node *pl,node *pr) : count(c),left(pl),right(pr) {}
  node* insert(int l,int r,int val){
    if(val < l or val >= r) return this;
    if(l+1 == r) return new node(1,nullptr,nullptr);
    int mid = (l+r)>>1;
    return new node(count+1,left->insert(l,mid,val),right->insert(mid,r,val));
  }
};
int queryIdx(node *pl,node *pr,int l,int r,int k){
  if(l+1 == r) return l;
  int mid = (l+r)>>1;
  int cnt = pl->left->count - pr->left->count;
  if(k <= cnt) return queryIdx(pl->left,pr->left,l,mid,k);
  return queryIdx(pl->right,pr->right,mid,r,k-cnt);
}
const int N = 100005;
vector<int> a(N),midx(N);
vector<node*> root(N);
int main(){
  ios::sync_with_stdio(0);
  cin.tie(0);
  int n,m,u,v,k,ans;
  cin >> n >> m;
  map<int,int> mp;
  for(int i = 0; i < n; ++i){
    cin >> a[i];
    mp[a[i]];
  }
  int nb = 0;
  for(auto& p : mp){
    mp[p.first] = nb;
    midx[nb] = p.first;
    ++nb;
  }
  node *pnull = new node();
  pnull->left = pnull;
  pnull->right = pnull;
  root[0] = pnull->insert(0,nb,mp[a[0]]);
  for(int i = 1; i < n; ++i){
    root[i] = root[i-1]->insert(0,nb,mp[a[i]]);
  }
  while(m--){
    cin >> u >> v >> k;
    --u;
    --v;
    if(u == 0) ans = queryIdx(root[v],pnull,0,nb,k);
    else ans = queryIdx(root[v],root[u-1],0,nb,k);
    cout << midx[ans] << '\n';
  }
  return 0;
}
```

{% embed url="https://www.spoj.com/problems/COT/" %}

```cpp
#include <bits/stdc++.h>
using namespace std;
struct node{
  int count;
  node *left, *right;
  node() : count(0),left(nullptr),right(nullptr) {}
  node(int c,node *pl,node *pr) : count(c),left(pl),right(pr) {}
  node* insert(int l,int r,int val){
    if(val < l or val >= r) return this;
    if(l+1 == r) return new node(1,nullptr,nullptr);
    int mid = (l+r)>>1;
    return new node(count+1,left->insert(l,mid,val),right->insert(mid,r,val));
  }
};
const int N = 100005;
vector<int> a(N),midx(N),depth(N),adj[N];
vector<vector<int>> parent;
map<int,int> mp;
vector<node*> root(N);
node *pnull = new node();
int nb,logN;
inline void dfs(int node,int pere,int dep){
  parent[0][node] = pere;
  depth[node] = dep;
  if(pere == -1) root[node] = pnull;
  else root[node] = root[pere]->insert(0,nb,mp[a[node]]);
  for(int i = 0; i < adj[node].size(); ++i){
    if(adj[node][i] == pere) continue;
    dfs(adj[node][i],node,dep+1);
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
inline query(node *u,node *v,node *lca,node *plca,int left,int right,int k){
  if(left+1 == right) return left;
  int cnt = u->left->count + v->left->count - lca->left->count - plca->left->count;
  int mid = (left+right)/2;
  if(cnt >= k) return query(u->left,v->left,lca->left,plca->left,left,mid,k);
  return query(u->right,v->right,lca->right,plca->right,mid,right,k-cnt);
}
int main(){
  ios::sync_with_stdio(0);
  cin.tie(0);
  pnull->left = pnull;
  pnull->right = pnull;
  int n,m,u,v,k;
  cin >> n >> m;
  logN = int(log2(n))+1;
  parent = vector<vector<int>>(logN,vector<int>(n,-1));
  for(int i = 0; i < n; ++i){
    cin >> a[i];
    mp[a[i]];
  }
  nb = 0;
  for(auto& p : mp){
    mp[p.first] = nb;
    midx[nb] = p.first;
    ++nb;
  }
  for(int i = 0; i < n-1; ++i){
    cin >> u >> v;
    --u;
    --v;
    adj[u].push_back(v);
    adj[v].push_back(u);
  }
  dfs(0,-1,0);
  // DP to prepare LCA
  for(int i = 1; i < logN; ++i){
    for(int j = 0; j < n; ++j){
      int prev = parent[i-1][j];
      if(prev >= 0) parent[i][j] = parent[i-1][prev];
    }
  }
  while(m--){
    cin >> u >> v >> k;
    --u;
    --v;
    int lca = LCA(u,v),ans;
    if(parent[0][lca] == -1){
      ans = query(root[u],root[v],root[lca],pnull,0,nb,k);
    }
    else {
      ans = query(root[u],root[v],root[lca],root[parent[0][lca]],0,nb,k);
    }
    cout << midx[ans] << '\n';
  }
  return 0;
}
```

