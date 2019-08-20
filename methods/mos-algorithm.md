# Mo's algorithm

{% embed url="https://www.hackerearth.com/fr/practice/notes/mos-algorithm/" %}

The algorithm is applicable if all following conditions are met:

1. Arr is not changed by queries;
2. All queries are known beforehand \(techniques requiring this property are often called “offline algorithms”\);
3. If we know Func\(\[L, R\]\), then we can compute Func\(\[L + 1, R\]\), Func\(\[L - 1, R\]\), Func\(\[L, R + 1\]\) and Func\(\[L, R - 1\]\), each in **O\(F\)** time.

Mo’s algorithm provides a way to **answer all queries in O\(\(N + Q\) \* sqrt\(N\) \* F\) time** with at least O\(Q\) additional memory.

{% embed url="https://www.spoj.com/problems/DQUERY/" %}

```cpp
#include <bits/stdc++.h>
using namespace std;
const int A = 300005,N = 1000005,Q = 200005;
const int BLOCK = 555;
int a[A],m[N],cnt = 0;
tuple<int,int,int> queries[Q];
int ans[Q];
inline bool mo_cmp(const tuple<int,int,int>& x,const tuple<int,int,int>& y){
  int block_x = get<0>(x)/BLOCK;
  int block_y = get<0>(y)/BLOCK;
  if(block_x != block_y) return block_x < block_y;
  return get<1>(x) < get<1>(y);
}
inline void add(int val){
  ++m[val];
  if(m[val] == 1) ++cnt;
}
inline void remove(int val){
  --m[val];
  if(m[val] == 0) --cnt;
}
int main(){
  ios::sync_with_stdio(0);
  cin.tie(0);
  int n,q,l,r,idx;
  scanf("%d",&n);
  for(int i = 0; i < n; ++i) scanf("%d",&a[i]);
  scanf("%d",&q);
  for(int i = 0; i < q; ++i){
    scanf("%d%d",&l,&r);
    queries[i] = {l-1,r-1,i};
  }
  sort(queries,queries+q,mo_cmp);
  int mo_left = 0,mo_right = -1;
  for(int i = 0; i < q; ++i){
    tie(l,r,idx) = queries[i];
    while(mo_right < r) ++mo_right, add(a[mo_right]);
    while(mo_right > r) remove(a[mo_right]), mo_right--;
    while(mo_left < l) remove(a[mo_left]), mo_left++;
    while(mo_left > l) --mo_left, add(a[mo_left]);
    ans[idx] = cnt;
  }
  for(int i = 0; i < q; ++i) printf("%d\n",ans[i]);
  return 0;
}
```

