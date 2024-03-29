# Bipartite / Max matching

### O\(V^2\) Matching : [https://cses.fi/problemset/task/1696](https://cses.fi/problemset/task/1696)

```cpp
#include <bits/stdc++.h>
using namespace std;

const int N = 501;

vector<vector<int>> adj(N);
int l[N],r[N];
bitset<N> vu;

int dfs(int left){
	if (vu[left]) return 0;
	vu[left] = true;
	for (int right:adj[left]){
		if (r[right] == -1 or dfs(r[right])){
			r[right] = left;
			l[left] = right;
			return 1;
		}
	}
	return 0;
}

int main(){
	ios::sync_with_stdio(0);
	cin.tie(0);
	int n,m,k;
	cin >> n >> m >> k;
	for (int i=0; i<k; ++i){
		int a,b;
		cin >> a >> b;
		adj[a].emplace_back(b);
	}
	memset(l,-1,sizeof(l));
	memset(r,-1,sizeof(r));
	int max_matching = 0;
	for (int i=1; i<=n; ++i){
		vu.reset();
		max_matching += dfs(i);
	}
	cout << max_matching << '\n';
	for (int i=1; i<=m; ++i){
		if (r[i] != -1){
			cout << r[i] << ' ' << i << '\n';
		}
	}
	return 0;
}
```

### Hopcroft-Krap

[https://codeforces.com/problemset/problem/120/H](https://codeforces.com/problemset/problem/120/H)

```cpp
#include <bits/stdc++.h>
using namespace std;
const int inf = INT_MAX/2;
int n,m;
vector<int> matchu,matchv,dist;
vector<vector<int>> adj;
bool bfs(){
  deque<int> q;
  for(int u = 1; u <= n; ++u){
    if(matchu[u] == 0){
      dist[u] = 0;
      q.emplace_back(u);
    }
    else dist[u] = inf;
  }
  dist[0] = inf;
  while(!q.empty()){
    int u = q.front();
    q.pop_front();
    if(dist[u] >= dist[0]) continue; // break;
    for(int v : adj[u]){
      if(dist[matchv[v]] == inf){
        dist[matchv[v]] = dist[u] + 1;
        q.emplace_back(matchv[v]);
      }
    }
  }
  return (dist[0] != inf);
}
bool dfs(int u){
  if(u == 0) return true;
  for(int v : adj[u]){
    if(dist[matchv[v]] == dist[u]+1){
      if(dfs(matchv[v])){
        matchv[v] = u;
        matchu[u] = v;
        return true;
      }
    }
  }
  dist[u] = inf;
  return false;
}
int hopcroftKarp(){
  int ret = 0;
  while(bfs()){
    for(int u = 1; u <= n; ++u){
      if(matchu[u] == 0 and dfs(u)) ++ret;
    }
  }
  return ret;
}
int main(){
  ios::sync_with_stdio(false);
  cin.tie(nullptr);
  freopen("output.txt","w",stdout);
  freopen("input.txt","r",stdin);
  m = (1<<20);
  cin >> n;
  string s;
  adj.resize(n+1);
  matchu.resize(n+1);
  matchv.resize(m+1);
  dist.resize(n+1);
  for(int i = 1; i <= n; ++i){
    cin >> s;
    set<int> d1,d2,d3,d4;
    for(char c : s){
      for(int dd : d3) d4.insert((dd<<5)+(c-'a')+1);
      for(int dd : d2) d3.insert((dd<<5)+(c-'a')+1);
      for(int dd : d1) d2.insert((dd<<5)+(c-'a')+1);
      d1.insert((c-'a')+1);
    }
    for(int dd : d1) adj[i].push_back(dd);
    for(int dd : d2) adj[i].push_back(dd);
    for(int dd : d3) adj[i].push_back(dd);
    for(int dd : d4) adj[i].push_back(dd);
  }
  int ans = hopcroftKarp();
  if(ans < n) cout << -1;
  else{
    for(int u = 1; u <= n; ++u){
      int tmp = matchu[u];
      s = "";
      while(tmp > 0){
        s.push_back(char('a'+(tmp&31)-1));
        tmp >>= 5;
      }
      reverse(begin(s),end(s));
      cout << s << '\n';
    }
  }
  return 0;
}
```

Fario 2017

[https://orac.amt.edu.au/cgi-bin/train/problem.pl?set=fario17&problemid=924](https://orac.amt.edu.au/cgi-bin/train/problem.pl?set=fario17&problemid=924)

