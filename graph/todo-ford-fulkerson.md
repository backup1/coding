# Ford Fulkerson

[https://cses.fi/problemset/task/1694/](https://cses.fi/problemset/task/1694/)

```cpp
#include <bits/stdc++.h>
#define int long long
using namespace std;

const int N = 501, inf = 1e9;

int G[N][N], F[N][N]; // G initial Graph, F current flow
bitset<N> vu;
int n;

int dfs(int s, int t, int mini){
	vu[s] = true;
	if (s == t) return mini;
	for (int i=1; i<=n; ++i){
		int capacity = G[s][i]-F[s][i]; // residual graph
		if (capacity and !vu[i]){
			int flow = dfs(i,t,min(mini,capacity));
			if (flow){
				F[s][i] += flow;
				F[i][s] -= flow;
				return flow;
			}
		}
	}
	return 0;
} 

int32_t main(){
	ios::sync_with_stdio(0);
	cin.tie(0);
	int m; // source : 1, sink : n
	cin >> n >> m;
	for (int i=0; i<m; ++i){
		int a,b,c;
		cin >> a >> b >> c;
		G[a][b] += c;
	}
	int ans = 0;
	while (true){
		vu.reset();
		int flow = dfs(1,n,inf);
		if (flow == 0) break;
		ans += flow;
	}
	cout << ans;
	return 0;
}
```

