# Binary Lifting

APIO 2009 Convention Center [https://www.acmicpc.net/problem/4012](https://www.acmicpc.net/problem/4012)

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



