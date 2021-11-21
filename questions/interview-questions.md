# interview questions

#### C++ derived class

```cpp
#include <bits/stdc++.h>
using namespace std;
class A {
public:
        virtual void print(){
                cout << 1 << endl;
        }
};
class B : public A {
public:
        virtual void print(){
                cout << 2 << endl;
        }
};
int main(){
        A* obj = new B();
        obj->print(); // output : 2
        return 0;
}
```

#### difference between C++ metaprogramming and Golang metaprogramming

\=> there is no metaprogramming in Golang !

#### Qualcomm coding question

The objective is to write a program that finds the shortest path inside a maze. The maze is stored in the attached text file: maze.txt. Your program should print the path from S to E as a sequence of coordinates: (0,1),(1,1),(1,2),(1,3)…

Specification of maze.txt:

* The maze size is 8x8 cells (8 rows and 8 columns)
* Start is represented by letter S
* End is represented by letter E
* Open cells are represented by space characters
* Obstacles are represented by x characters
* Cells are connected horizontally and vertically. Diagonal moves are forbidden.

```
xxxxxxxx
S      x
x x xx x
x xxxx E
x  xx  X
xx xx xx
xx    xx
xxxxxxxx
```

```cpp
#include <bits/stdc++.h>
using namespace std;
vector<pair<int,int>> dirs = {{1,0},{-1,0},{0,1},{0,-1}};
int main(){
  int n = 8;
  vector<string> maze(n);
  pair<int,int> S,E;
  for(int i = 0; i < n; ++i){
    for (int j=0; j<n; ++j){
      char c=getchar();
      maze[i].push_back(c);
      if(c == 'S') S={i,j};
      if(c == 'E') E={i,j};
    }
    getchar();
  }
  vector<vector<int>> step(n,vector<int>(n,INT_MAX));
  deque<pair<int,int>> q;
  q.emplace_back(S);
  step[S.first][S.second] = 0;
  while(!q.empty()){
    int x,y;
    tie(x,y) = q.front();
    q.pop_front();
    int nbstep = step[x][y];
    for(auto& dir : dirs){
      int nx = x + dir.first, ny = y + dir.second;
      if(0 <= nx and nx < n and 0 <= ny and ny < n and (maze[nx][ny] != ' ' or maze[nx][ny] == 'E') and step[nx][ny] == INT_MAX){
        step[nx][ny] = nbstep+1;
        q.emplace_back(nx,ny);
      }
    }
  }
  vector<pair<int,int>> path;
  path.emplace_back(E);
  while(path.back() != S){
    int x,y;
    tie(x,y) = path.back();
    int nbstep = step[x][y];
    for(auto& dir : dirs){
      int nx = x + dir.first, ny = y + dir.second;
      if(0 <= nx and nx < n and 0 <= ny and ny < n and maze[nx][ny] != 'X' and step[nx][ny] == nbstep-1){
        path.emplace_back(nx,ny);
      }
    }
  }
  reverse(begin(path),end(path));
  for(auto& node : path){
    cout << "(" << node.first << "," << node.second << "),";
  }
  cout << endl;
  return 0;
}
```

#### Qualcomm design question

For this exercise, the goal is to design a software to manage a car rental agency.

You only need to provide class interfaces: class name, method and attribute names, possibly method arguments.

Do NOT code any method body. The goal is to evaluate your class architecture.

You can send header files, or if you prefer a photo of an hand drawn UML diagram.

This is an open ended exercise, so try to finish the coding exercise, before diving too deep into this exercise.

Requirements:

* The agency rents cars, but also small trucks and a couple of motorbikes.
* The agency must store rental orders. The state of the vehicles is stored at check in and updated at when the customer returns the vehicle.
* Clients renting several times a year are eligible for a discount. Clients must have their driving license for at least two year, and be older than 25 years old.
* The agency owner needs to handle a stock of spare wheel for the vehicles. For legal reasons, he also needs to track maintenance dates of the vehicles.
* There several employees, with different working hours. We want to track which employee handles a given rental order.
