# Segment Tree

## Min Segment Tree

```cpp
int getMin(vector<int>& segt,int idx,int left,int right,int L,int R){
  if(L >= R) return inf;
  if(L == left and R == right) return segt[idx];
  int mid = (left+right)/2;
  return min(getMin(segt,2*idx+1,left,mid,L,min(R,mid)),
             getMin(segt,2*idx+2,mid,right,max(L,mid),R));
}
void update(vector<int>& segt,int idx,int left,int right,int pos,int val){
  if(right-left <= 1){
    segt[idx] = val;
    return;
  }
  int mid = (left+right)/2;
  if(pos < mid) update(segt,2*idx+1,left,mid,pos,val);
  else update(segt,2*idx+2,mid,right,pos,val);
  segt[idx] = min(segt[2*idx+1],segt[2*idx+2]);
}
```

## Problems

{% embed url="https://codeforces.com/contest/1187/problem/D" %}

```cpp
#include <bits/stdc++.h>
using namespace std;
const int inf = INT_MAX/2;
int getMin(vector<int>& segt,int idx,int left,int right,int L,int R){
  if(L >= R) return inf;
  if(L == left and R == right) return segt[idx];
  int mid = (left+right)/2;
  return min(getMin(segt,2*idx+1,left,mid,L,min(R,mid)),
             getMin(segt,2*idx+2,mid,right,max(L,mid),R));
}
void update(vector<int>& segt,int idx,int left,int right,int pos,int val){
  if(right-left <= 1){
    segt[idx] = val;
    return;
  }
  int mid = (left+right)/2;
  if(pos < mid) update(segt,2*idx+1,left,mid,pos,val);
  else update(segt,2*idx+2,mid,right,pos,val);
  segt[idx] = min(segt[2*idx+1],segt[2*idx+2]);
}
int main(){
  ios::sync_with_stdio(false);
  cin.tie(nullptr);
  int t;
  cin >> t;
  while(t--){
    int n,a,b;
    cin >> n;
    vector<deque<int>> aidxs(n+1);
    vector<int> segt(4*n+100,inf);
    for(int i = 0; i < n; ++i){
      cin >> a;
      --a;
      aidxs[a].push_back(i);
    }
    for(int i = 0; i < n; ++i){
      if(!aidxs[i].empty())
        update(segt,0,0,n,i,aidxs[i][0]);
    }
    bool done = false;
    for(int i = 0; i < n; ++i){
      cin >> b;
      --b;
      if(done) continue;
      if(aidxs[b].empty() or
         aidxs[b][0] != getMin(segt,0,0,n,0,b+1)){
        cout << "NO\n";
        done = true;
        continue;
      }
      aidxs[b].pop_front();
      int val = inf;
      if(!aidxs[b].empty()) val = aidxs[b][0];
      update(segt,0,0,n,b,val);
    }
    if(!done) cout << "YES\n";
  }
  return 0;
}
```

{% embed url="https://codeforces.com/problemset/problem/292/E" %}

```cpp
#include <bits/stdc++.h>
using namespace std;
vector<vector<int>> v;
int get(vector<pair<int,int>>& segt,int idx,int left,int right,int pos){
  if(segt[idx].first){
    int array,i;
    tie(array,i) = segt[idx];
    return v[array-1][i+pos-left];
  }
  int mid = (left+right)/2;
  if(pos < mid) return get(segt,2*idx+1,left,mid,pos);
  return get(segt,2*idx+2,mid,right,pos);
}
void update(vector<pair<int,int>>& segt,int idx,int left,int right,int L,int R,int array,int beg){
  if(L >= R) return;
  if(L == left and R == right){
    segt[idx] = make_pair(array,beg);
    return;
  }
  int mid = (left+right)/2;
  if(segt[idx].first){
    segt[2*idx+1] = segt[idx];
    segt[2*idx+2] = segt[idx];
    segt[2*idx+2].second += mid-left;
    segt[idx] = make_pair(0,0);
  }
  update(segt,2*idx+1,left,mid,L,min(R,mid),array,beg);
  update(segt,2*idx+2,mid,right,max(L,mid),R,array,beg+max(mid-L,0));
}
int main(){
  ios::sync_with_stdio(false);
  cin.tie(nullptr);
  int n,m,t,x,y,k;
  cin >> n >> m;
  v = vector<vector<int>>(2,vector<int>(n+1));
  for(int i = 1; i <= n; ++i) cin >> v[0][i];
  for(int i = 1; i <= n; ++i) cin >> v[1][i];
  vector<pair<int,int>> segt(4*n+5);
  update(segt,0,1,n+1,1,n+1,2,1);
  while(m--){
    cin >> t;
    if(t == 1){
      cin >> x >> y >> k;
      update(segt,0,1,n+1,y,y+k,1,x);
    }
    else{
      cin >> x;
      cout << get(segt,0,1,n+1,x) << '\n';
    }
  }
  return 0;
}
```

{% embed url="https://codeforces.com/problemset/problem/641/E" %}

```cpp
#include <bits/stdc++.h>
using namespace std;
int getSum(vector<int>& segt,int idx,int left,int right,int L,int R){
  if(L >= R) return 0;
  if(L == left and R == right) return segt[idx];
  int mid = (left+right)/2;
  return getSum(segt,2*idx+1,left,mid,L,min(R,mid))+
         getSum(segt,2*idx+2,mid,right,max(L,mid),R);
}
void update(vector<int>& segt,int idx,int left,int right,int pos,int delta){
  if(right-left <= 1){
    segt[idx] += delta;
    return;
  }
  int mid = (left+right)/2;
  if(pos < mid) update(segt,2*idx+1,left,mid,pos,delta);
  else update(segt,2*idx+2,mid,right,pos,delta);
  segt[idx] = segt[2*idx+1]+segt[2*idx+2];
}
int main(){
  ios::sync_with_stdio(false);
  cin.tie(nullptr);
  int n,a,t,x;
  cin >> n;
  vector<tuple<int,int,int>> cmds;
  map<int,set<int>> ts;
  while(n--){
    cin >> a >> t >> x;
    cmds.emplace_back(a,t,x);
    ts[x].emplace(t);
  }
  map<int,vector<int>> tv;
  for(auto& p : ts) tv[p.first].assign(begin(p.second),end(p.second));
  map<int,map<int,int>> mt;
  map<int,int> ls;
  for(auto& p : tv){
    ls[p.first] = p.second.size();
    for(int i = 0; i < p.second.size(); ++i) mt[p.first][p.second[i]] = i;
  }
  map<int,vector<int>> m;
  for(auto& p : cmds){
    tie(a,t,x) = p;
    if(!m.count(x)) m[x] = vector<int>(4*ls[x]+4);
    if(a == 1) update(m[x],0,0,ls[x],mt[x][t],1);
    else if(a == 2) update(m[x],0,0,ls[x],mt[x][t],-1);
    else cout << getSum(m[x],0,0,ls[x],0,mt[x][t]) << '\n';
  }
  return 0;
}
```

