# Max Flow

API : [https://github.com/atcoder/ac-library/blob/master/document\_en/maxflow.md](https://github.com/atcoder/ac-library/blob/master/document_en/maxflow.md)

```cpp
// constructor (0 <= n <= 1e8) O(n)
// Cap = int or ll
mf_graph<Cap> graph(int n)
// add edge from -> to with cap >= 0 and flow = 0
// O(1)
// return the id number (in order of adding) of the edge
int graph.add_edge(int from,int tp, Cap cap);
// flow :return amount of augmented flow from s to t (s != t)
// O(min(n^0.666 m, m^1.5) if all capacities are 1
// O(n^2 m) in general
Cap graph.flow(int s, int t);
Cap graph.flow(int s, int t, Cap flow_limit);
// min cut : s-t min cut - call it after flow(s,t) without flow_limit
// v[i] = true iff existing a directed path s -> i in the residual network
// O(m+n)
vector<bool> graph.min_cut(int s)
struct mf_graph<Cap>::edge {
  int from, to;
  Cap cap, flow;
};
// get_edge O(1)
mf_graph<Cap>::edge graph.get_edge(int i);
// edges O(m)
// same order as added by add_edge
vector<mf_graph<Cap>::edge> graph.edges();
// change edge O(1)
void graph.change_edge(int i, Cap new_cap, Cap new_flow);
```

Example question : [https://atcoder.jp/contests/practice2/tasks/practice2\_d](https://atcoder.jp/contests/practice2/tasks/practice2_d)

```cpp
#include <bits/stdc++.h>
#include <atcoder/maxflow>
using namespace std;
using namespace atcoder;
vector<pair<int,int>> dirs = { {0,1},{1,0},{0,-1},{-1,0} };
int main(){
  ios::sync_with_stdio(0);
  cin.tie(0);
  int n,m;
  cin >> n >> m;
  vector<string> vs(n);
  for(int i = 0; i < n; ++i) cin >> vs[i];
  mf_graph<int> graph(n*m+2);
  int src = n*m, sink = n*m+1;
  for(int i = 0; i < n; ++i){
    for(int j = 0; j < m; ++j){
      if(vs[i][j] == '.'){
        int from = i*m+j;
        if((i+j)&1){
          graph.add_edge(src,from,1);
          for(auto dir : dirs){
            int x = i + dir.first, y = j + dir.second;
            if(0 <= x and x < n and 0 <= y and y < m and vs[x][y] == '.'){
              int to = x*m+y;
              graph.add_edge(from,to,1);
            }
          }
        }
        else graph.add_edge(from,sink,1);
      }
    }
  }
  int ans = graph.flow(src,sink);
  cout << ans << '\n';
  for(auto& edge : graph.edges()){
    if(edge.from != src and edge.to != sink and edge.flow == 1){
      int from = edge.from, to = edge.to;
      int x1 = from/m, y1 = from%m, x2 = to/m, y2 = to%m;
      if(x1 == x2){
        if(y1 < y2){
          vs[x1][y1] = '>';
          vs[x2][y2] = '<';
        }
        else{
          vs[x2][y2] = '>';
          vs[x1][y1] = '<';
        }
      }
      else{
        if(x1 < x2){
          vs[x1][y1] = 'v';
          vs[x2][y2] = '^';
        }
        else{
          vs[x2][y2] = 'v';
          vs[x1][y1] = '^';
        }
      }
    }
  }
  for(auto& s : vs) cout << s << '\n';
  return 0;
}
```

