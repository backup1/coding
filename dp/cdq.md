# CDQ

[https://codeforces.com/problemset/problem/669/E](https://codeforces.com/problemset/problem/669/E)

```cpp
#include <bits/stdc++.h>
using namespace std;
struct node {
  int type,time,x,qidx;
};
vector<node> v,tmp;
map<int,int> cnt;
int ans[100100],nb;
void cdq(int left,int right){
  if(left == right) return;
  int mid = (left+right)/2;
  cdq(left,mid);
  cdq(mid+1,right);
  int left1 = left, left2 = mid+1, pos = left;
  cnt.clear();
  while(left1 <= mid and left2 <= right){
    if(v[left1].time < v[left2].time){
      if(v[left1].type == 1) ++cnt[v[left1].x];
      else if(v[left1].type == 2) --cnt[v[left1].x];
      tmp[pos++] = v[left1++];
    }
    else{
      if(v[left2].type == 3) ans[v[left2].qidx] += cnt[v[left2].x];
      tmp[pos++] = v[left2++];
    }
  }
  while(left1 <= mid){
    if(v[left1].type == 1) ++cnt[v[left1].x];
    else if(v[left1].type == 2) --cnt[v[left1].x];
    tmp[pos++] = v[left1++];
  }
  while(left2 <= right){
    if(v[left2].type == 3) ans[v[left2].qidx] += cnt[v[left2].x];
    tmp[pos++] = v[left2++];
  }
  for(int i = left; i <= right; ++i) v[i] = tmp[i];
}
int main(){
  ios::sync_with_stdio(0);
  cin.tie(0);
  int n,a,t,x;
  cin >> n;
  v.resize(n);
  tmp.resize(n);
  memset(ans,0,sizeof(ans));
  nb = 0;
  for(int i = 0; i < n; ++i){
    cin >> a >> t >> x;
    v[i] = node{a,t,x,nb};
    if(a == 3) ++nb;
  }
  cdq(0,n-1);
  for(int i = 0; i < nb; ++i) cout << ans[i] << '\n';
  return 0;
}
```

