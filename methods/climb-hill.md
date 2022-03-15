# Climb Hill

\+Question 1/ **POJ 2069 Super Star** : http://poj.org/problem?id=2069

```cpp
#include <cstdio>
#include <cmath>
using namespace std;
struct pt{
  double x,y,z;
  pt(){}
  pt(const double &_x, const double &_y, const double &_z) : x(_x), y(_y), z(_z) {}
} pts[35];
double dist(pt& a,pt& b){
  double dx = a.x-b.x, dy = a.y-b.y, dz = a.z-b.z;
  return sqrt(dx*dx+dy*dy+dz*dz);
}
void play(int n){
  double x = 0, y = 0, z = 0;
  for(int i = 0; i < n; ++i){
    scanf("%lf%lf%lf", &pts[i].x, &pts[i].y, &pts[i].z);
    x += pts[i].x;
    y += pts[i].y;
    z += pts[i].z;
  }
  pt cpt(x/n,y/n,z/n);
  double best = 1e15;
  for(double t = 100; t > 1e-8; t *= 0.98){
    int idx = 0;
    double R = dist(pts[0],cpt);
    for(int i = 1; i < n; ++i){
      double Ri = dist(pts[i],cpt);
      if(Ri > R){
        idx = i;
        R = Ri;
      }
    }
    if(best > R) best = R;
    cpt.x += (pts[idx].x-cpt.x)/R*t;
    cpt.y += (pts[idx].y-cpt.y)/R*t;
    cpt.z += (pts[idx].z-cpt.z)/R*t;
  }
  printf("%.5f\n", best);
}
int main(){
  int n;
  while(scanf("%d",&n), n) play(n);
  return 0;
}

```
