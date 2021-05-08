# DP

[https://codeforces.com/contest/148/problem/E](https://codeforces.com/contest/148/problem/E)

```cpp
#include <bits/stdc++.h>
using namespace std;
int n,m;
vector<int> a[100];
int dp[100][101];
int main(){
  ios::sync_with_stdio(0);
  cin.tie(0);
  cin >> n >> m;
  for(int i = 0; i < n; ++i){
    int l,x;
    cin >> l;
    for(int j = 0; j < l; ++j){
      cin >> x;
      a[i].push_back(x);
    }
  }
  memset(dp,-1e9,sizeof(dp));
  for(int i = 0; i < n; ++i){
    int psum = 0;
    dp[i][0] = 0;
    int li = a[i].size();
    for(int j = 1; j <= li; ++j){
      psum += a[i][j-1];
      dp[i][j] = psum;
      int tmp = psum;
      for(int k = j-1; k >= 0; --k){
        tmp = tmp-a[i][k]+a[i][li-j+k];
        dp[i][j] = max(dp[i][j],tmp);
      }
    }
  }
  vector<int> vmax(m+1,-1e9);
  vmax[0] = 0;
  for(int i = 0; i < n; ++i){
    int li = a[i].size();
    vector<int> vmax2(m+1,1e-9);
    vmax2[0] = 0;
    for(int j = 0; j <= li; ++j){
      for(int k = m; k >= j; --k){
        vmax2[k] = max(vmax2[k],vmax[k-j]+dp[i][j]);
      }
    }
    vmax = vmax2;
  }
  cout << vmax[m];
  return 0;
}
```

