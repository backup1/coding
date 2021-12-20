# Leetcode (Hard)

980 Unique Paths III [https://leetcode.com/problems/unique-paths-iii/](https://leetcode.com/problems/unique-paths-iii/)

```cpp
vector<pair<int,int>> dirs = {{0,1},{0,-1},{1,0},{-1,0}};
int uniquePathsIII(vector<vector<int>>& grid) {
  int r = grid.size(), c = grid[0].size();
  int bx,by,ex,ey,tot = 0;
  vector<vector<bool>> vu(r,vector<bool>(c));
  for(int i = 0; i < r; ++i){
    for(int j = 0; j < c; ++j){
      if(grid[i][j] == 1){
        bx = i;
        by = j;
        vu[i][j] = true;
      }
      else if(grid[i][j] == 2){
        ex = i;
        ey = j;
        ++tot;
      }
      else if(grid[i][j] == 0) ++tot;
      else vu[i][j] = true;
    }
  }
  int nb = 0;
  check(vu,r,c,bx,by,ex,ey,tot,nb);
  return nb;
}
void check(vector<vector<bool>>& vu,int r,int c,int bx,int by,int ex,int ey,int tot,int& nb){
  if(bx == ex and by == ey){
    if(tot == 0) ++nb;
    return;
  }
  for(auto& dir : dirs){
    int nx = bx+dir.first,ny = by+dir.second;
    if(0 <= nx and nx < r and 0 <= ny and ny < c and vu[nx][ny] == false){
      vu[nx][ny] = true;
      check(vu,r,c,nx,ny,ex,ey,tot-1,nb);
      vu[nx][ny] = false;
    }
  }
}
```

1402 Reducing Dishes [https://leetcode.com/problems/reducing-dishes/](https://leetcode.com/problems/reducing-dishes/)

```cpp
int maxSatisfaction(vector<int>& satisfaction) {
  sort(begin(satisfaction),end(satisfaction),[](int& a,int& b){return a > b;});
  int sum = 0,ans = 0;
  for(int i : satisfaction){
    sum += i;
    if(sum <= 0) break;
    ans += sum;
  }
  return ans;
}
```
