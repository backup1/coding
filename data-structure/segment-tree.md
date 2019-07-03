# Segment Tree

## Min Segment Tree

```cpp
int getMin(vector<int>& segt,
           int idx,int left,int right,
           int L,int R){
  if(L >= R) return inf;
  if(L == left and R == right) return segt[idx];
  int mid = (left+right)/2;
  return min(getMin(segt,2*idx+1,left,mid,
                    L,min(R,mid)),
             getMin(segt,2*idx+2,mid,right,
                    max(L,mid),R));
}
void update(vector<int>& segt,
            int idx,int left,int right,
            int pos,int val){
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
int getMin(vector<int>& segt,
           int idx,int left,int right,
           int L,int R){
  if(L >= R) return inf;
  if(L == left and R == right) return segt[idx];
  int mid = (left+right)/2;
  return min(getMin(segt,2*idx+1,left,mid,
                    L,min(R,mid)),
             getMin(segt,2*idx+2,mid,right,
                    max(L,mid),R));
}
void update(vector<int>& segt,
            int idx,int left,int right,
            int pos,int val){
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

