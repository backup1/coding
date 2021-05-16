# Edmond Karp



```cpp
// 101 = source;
// 102 = sink;
#include <bits/stdc++.h>
#define int long long
using namespace std;

vector<map<int,int>> adj(20005);
int n,m;

int EdmondKarp(){
   int max_flow = 0;
   vector<int> backtrack(20005,-1);
   bitset<20005> vu;
   deque<pair<int,int>> q;
   bool foundAugmentingPath = true;
   while (foundAugmentingPath){
      q.clear();
      vu.reset();
      foundAugmentingPath = false;
      q.emplace_back(1,INT_MAX);
      vu[1] = true;
      while (!q.empty()){
         auto node = q.front();
         q.pop_front();
         if (node.first == n){
            // we got to the sink => found augmenting path
            // so we are gonna do backtracking to update edges
            int crtNode = n, mini = node.second;
            while (backtrack[crtNode] != -1){
               int pastNode = backtrack[crtNode];
               adj[crtNode][pastNode] += mini;
               adj[pastNode][crtNode] -= mini;
               crtNode = pastNode;
            }
            foundAugmentingPath = true;
            max_flow += mini;
//            break;
         }
         for (auto i:adj[node.first]){
            if (!vu[i.first] and i.second > 0){
               vu[i.first] = true;
               backtrack[i.first] = node.first;
               q.emplace_back(i.first,min(node.second,i.second));
            }
         }
      }
   }
   return max_flow;
}
int32_t main(){
   ios::sync_with_stdio(false);
   cin.tie(nullptr);
   cout.tie(nullptr);
   cin >> n >> m;
   for (int i=0; i<m; ++i){
      int a,b,c;
      cin >> a >> b >> c;
      adj[a][b] = c;
   }
   int max_flow = EdmondKarp();
   cout << max_flow << '\n';
   return 0;
}

```

