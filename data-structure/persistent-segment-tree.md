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

