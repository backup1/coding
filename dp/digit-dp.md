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

[https://atcoder.jp/contests/abc154/tasks/abc154\_e](https://atcoder.jp/contests/abc154/tasks/abc154_e)

```cpp
#include <bits/stdc++.h>
using namespace std;
using ll = long long;
ll dp[105][5];
int a[105];
string s;
ll dfs(int pos,int d,bool lim){
  if(pos == -1) return (d==0) ? 1 : 0;
  if(d < 0) return 0;
  if(!lim and dp[pos][d] != -1) return dp[pos][d];
  int x = (lim ? a[pos] : 9);
  ll ans = 0;
  for(int i = 0; i <= x; ++i){
    ans += dfs( pos-1,
                d - (i==0 ? 0 : 1),
                (i==x) ? lim : false );
  }
  if(!lim) dp[pos][d] = ans;
  return ans;
}
void solve(){
  int k,l = s.size();
  cin >> k;
  for(int i = 0; i < l; ++i) a[l-1-i] = s[i]-'0';
  cout << dfs(l-1,k,true);
}
int main(){
  ios::sync_with_stdio(0);
  cin.tie(0);
  memset(dp,-1,sizeof(dp));
  cin >> s;
  solve();
  return 0;
}
```

[https://codeforces.com/contest/401/problem/D](https://codeforces.com/contest/401/problem/D)

```cpp
#include <bits/stdc++.h>
using namespace std;
using ll = int64_t;
ll dp[1<<18][100];
int a[18];
int m,l;
ll dfs(int pos,int mask,int rem){
  if(pos < 0) return (rem ? 0 : 1);
  if(dp[mask][rem] != -1) return dp[mask][rem];
  vector<bool> flag(10);
  ll ans = 0;
  for(int i = 0; i < l; ++i){
    if((mask&(1<<i)) == 0 and !flag[a[i]]){
      flag[a[i]] = true;
      ans += dfs( pos-1,
                  mask+(1<<i),
                  (10*rem+a[i])%m );
    }
  }
  dp[mask][rem] = ans;
  return ans;
}
void solve(ll n){
  l = 0;
  while(n){
    a[l++] = n%10;
    n /= 10;
  }
  vector<bool> flag(10);
  ll ans = 0;
  for(int i = 0; i < l; ++i){
    if(a[i] and !flag[a[i]]){
      flag[a[i]] = true;
      ans += dfs(l-2,(1<<i),(a[i]%m));
    }
  }
  cout << ans;
}
int main(){
  ios::sync_with_stdio(0);
  cin.tie(0);
  memset(dp,-1,sizeof(dp));
  ll n;
  cin >> n >> m;
  solve(n);
  return 0;
}
```

[https://codeforces.com/contest/507/problem/D](https://codeforces.com/contest/507/problem/D)

```cpp
#include <bits/stdc++.h>
using namespace std;
using ll = long long;
int n,k,mod;
ll p10[1005];
ll dp[1005][105][2];
ll dfs(int pos,int rem,ll ok){
  if(pos >= n) return ok;
  if(dp[pos][rem][ok] != -1) return dp[pos][rem][ok];
  ll ans = 0;
  int x = (pos==n-1) ? 1 : 0;
  for(int i = x; i <= 9; ++i){
    int rem2 = (rem+p10[pos]*i)%k;
    int ok2 = ok;
    if(rem2 == 0 and i != 0) ok2 = 1;
    ans += dfs(pos+1,rem2,ok2);
    ans %= mod;
  }
  dp[pos][rem][ok] = ans;
  return ans;
}
int main(){
  ios::sync_with_stdio(0);
  cin.tie(0);
  memset(dp,-1,sizeof(dp));
  p10[0] = 1;
  cin >> n >> k >> mod;
  for(int i = 1; i <= 1000; ++i) p10[i] = (10*p10[i-1])%k;
  cout << dfs(0,0,0);
  return 0;
}
```

[https://cses.fi/problemset/task/2220/](https://cses.fi/problemset/task/2220/)

```cpp
#include <bits/stdc++.h>
using namespace std;
using ll = long long;
int v[20];
ll dp[20][11];
ll dfs(int pos,int prev,bool lead,bool lim){
  if(pos < 0) return 1;
  if(!lim and !lead and dp[pos][prev] != -1) return dp[pos][prev];
  int x = (lim ? v[pos] : 9);
  ll ans = 0;
  for(int i = 0; i <= x; ++i){
    if(lead and i==0){
      ans += dfs(pos-1,i,true,false);
    }
    else{
      if(i == prev) continue;
      ans += dfs(pos-1,i,false,lim and (i==x));
    }
  }
  if(!lim and !lead) dp[pos][prev] = ans;
  return ans;
}
ll solve(ll a){
  int d = 0;
  while(a){
    v[d++] = a%10;
    a /= 10;
  }
  return dfs(d-1,10,true,true);
}
int main(){
  ios::sync_with_stdio(0);
  cin.tie(0);
  memset(dp,-1,sizeof(dp));
  ll a,b;
  cin >> a >> b;
  cout << solve(b)-solve(a-1);
  return 0;
}
```

[https://codeforces.com/problemset/problem/1036/C](https://codeforces.com/problemset/problem/1036/C)

```cpp
#include <bits/stdc++.h>
using namespace std;
using ll = int64_t;
ll dp[20][5];
int a[20];
ll dfs(int pos,int nb,bool lim){
  if(nb < 0) return 0;
  if(pos < 0) return (nb >= 0) ? 1 : 0;
  if(!lim and dp[pos][nb] != -1) return dp[pos][nb];
  ll ans = 0;
  int x = (lim ? a[pos] : 9);
  for(int i = 0; i <= x; ++i){
    ans += dfs( pos-1,
                (i == 0) ? nb : nb-1,
                (i == x) ? lim : false );
  }
  if(!lim) dp[pos][nb] = ans;
  return ans;
}
ll solve(ll n){
  int d = 0;
  while(n){
    a[d++] = n%10;
    n /= 10;
  }
  return dfs(d-1,3,true);
}
int main(){
  ios::sync_with_stdio(0);
  cin.tie(0);
  memset(dp,-1,sizeof(dp));
  int t;
  cin >> t;
  while(t--){
    ll l,r;
    cin >> l >> r;
    cout << solve(r)-solve(l-1) << '\n';
  }
  return 0;
}
```

