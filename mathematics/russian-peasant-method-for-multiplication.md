# Russian peasant method for multiplication

{% embed url="https://acm.timus.ru/problem.aspx?space=1&num=1013" %}

```cpp
#include <bits/stdc++.h>
using namespace std;
using ll = int64_t;
// the Russian peasant method for multiplication
ll mult1(ll a,ll b,ll m){
  // (a*b)%m
  a %= m;
  b %= m;
  if(a < b) swap(a,b);
  ll ret = 0;
  while(b){
    if(b&1) ret = (ret+a)%m;
    a = (a+a)%m;
    b >>= 1;
  }
  return ret;
}
vector<ll> mult2(vector<ll>& A,vector<ll>& B,ll m){
  ll A00 = A[0],A01 = A[1],A10 = A[2],A11 = A[3];
  ll B00 = B[0],B01 = B[1],B10 = B[2],B11 = B[3];
  ll C00 = (mult1(A00,B00,m)+mult1(A01,B10,m))%m;
  ll C01 = (mult1(A00,B01,m)+mult1(A01,B11,m))%m;
  ll C10 = (mult1(A10,B00,m)+mult1(A11,B10,m))%m;
  ll C11 = (mult1(A10,B01,m)+mult1(A11,B11,m))%m;
  return {C00,C01,C10,C11};
}
/******************************
 * | k-1 k-1 |^n   |  F(n)  * |
 * |         |   = |          |
 * |  1   0  |     | F(n-1) * |
 ******************************/
void solve(ll n,ll k,ll m){
  vector<ll> matrix = {(k-1)%m,(k-1)%m,1,0};
  vector<ll> mat = {1,0,0,1};
  while(n){
    if(n&1) mat = mult2(mat,matrix,m);
    matrix = mult2(matrix,matrix,m);
    n >>= 1;
  }
  cout << mat[0];
}
int main(){
  ios::sync_with_stdio(0);
  cin.tie(0);
  ll n,k,m;
  cin >> n >> k >> m;
  solve(n,k,m);
  return 0;
}

```

