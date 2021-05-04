# Digit DP

[https://codeforces.com/problemset/problem/55/D](https://codeforces.com/problemset/problem/55/D)

```cpp
#include <bits/stdc++.h>
using namespace std;
using ll = int64_t;
map<int,int> mp;
ll dp[20][50][2520];
int a[20];
ll dfs(int pos,int Lcm,int rem,bool lim){
  if(pos == -1) return (rem%Lcm) ? 0 : 1;
  if(!lim and dp[pos][mp[Lcm]][rem] != -1) return dp[pos][mp[Lcm]][rem];
  int x = (lim ? a[pos] : 9);
  ll ans = 0;
  for(int i = 0; i <= x; ++i){
    ans += dfs( pos-1,
                (i==0) ? Lcm : lcm(i,Lcm),
                (10*rem+i)%2520,
                (i==x) ? lim : false );
  }
  if(!lim) dp[pos][mp[Lcm]][rem] = ans;
  return ans;
}
ll solve(ll n){
  int d = 0;
  while(n){
    a[d++] = n%10;
    n /= 10;
  }
  return dfs(d-1,1,0,true);
}
int main(){
  ios::sync_with_stdio(0);
  cin.tie(0);
  ll idx = 0;
  for(int i = 1; i <= 2520; ++i){
    if(2520%i == 0) mp[i] = idx++;
  }
  memset(dp,-1,sizeof(dp));
  int t;
  cin >> t;
  while(t--){
    ll l,r;
    cin >> l >> r;
    cout << solve(r) - solve(l-1) << '\n';
  }
  return 0;
}
```

[https://atcoder.jp/contests/dp/tasks/dp\_s](https://atcoder.jp/contests/dp/tasks/dp_s)

```cpp
#include <bits/stdc++.h>
using namespace std;
using ll = long long;
const ll mod = 1e9+7;
ll dp[10000][100];
int a[10000];
string s;
int d;
ll dfs(int pos,int rem,bool lim){
  if(pos == -1) return (rem%d) ? 0 : 1;
  if(!lim and dp[pos][rem] != -1) return dp[pos][rem];
  int x = (lim ? a[pos] : 9);
  ll ans = 0;
  for(int i = 0; i <= x; ++i){
    ans = ( dfs( pos-1,
                 (rem+i)%d,
                 (i==x) ? lim : false ) + ans ) %mod;
  }
  if(!lim) dp[pos][rem] = ans%mod;
  return ans;
}
void solve(){
  cin >> s >> d;
  int l = s.size();
  for(int i = 0; i < l; ++i) a[l-1-i] = s[i]-'0';
  cout << (dfs(l-1,0,true)+mod-1)%mod;
}
int main(){
  ios::sync_with_stdio(0);
  cin.tie(0);
  memset(dp,-1,sizeof(dp));
  solve();
  return 0;
}
```

