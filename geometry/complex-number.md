# Complex number

```cpp
#include <bits/stdc++.h>
using namespace std;
using pt = complex<double>;
const double PI = acos(-1);
// dot product / produit scalaire
double dot(pt a,pt b){
  return real(conj(a)*b);
}
// cross product / produit vectoriel
double cross(pt a,pt b){
  return imag(conj(a)*b);
}
double distance(pt a,pt b){
  return abs(a-b);
}
// la pente d'une droite (AB)
double slope(pt a,pt b){
  return tan(arg(b-a));
}
// rotation de radian degres autour d'un point p
pt rotate(pt a,pt p,double radian){
  return (a-p)*polar(1.0,radian)+p;
}
// angle ABC
double angle(pt a,pt b,pt c){
  return abs(remainder(arg(a-b)-arg(c-b),2.0*PI));
}
// aire ABC
double area(pt a,pt b,pt c){
  return 0.5*abs(cross(b-a,c-a));
}
// le projete (orthogonal) de p sur le vector v
pt projete(pt p,pt v){
  return v*dot(p,v)/norm(v);
}
// le projete (orthogonal) de p sur (AB)
pt projete(pt p,pt a,pt b){
  return a+(b-a)*dot(p-a,b-a)/norm(b-a);
}
// symetrie / reflection de p par rapport a (AB)
pt sym(pt p,pt a,pt b){
  return a+(b-a)*conj((p-a)/(b-a));
}
// intersection des deux droites (AB) et (PQ) non-paralleles
pt intersect(pt a,pt b,pt p,pt q){
  double c1 = cross(p-a,b-a), c2 = cross(q-a,b-a);
  assert(abs(c1-c2)>1e-6);
  return (c1*q-c2*p)/(c1-c2);
}
// check if (AB) // (PQ)
bool parallel(pt a,pt b,pt p,pt q){
  double c = cross(q-p,b-a);
  return (abs(c) < 1e-6);
}
// check if (AB) orthogonal to (PQ)
bool perp(pt a,pt b,pt p,pt q){
  double c = dot(q-p,b-a);
  return (abs(c) < 1e-6);
}

/* z = pt(x,y) // x + iy
argument  : arg(z)
magnitude : abs(z) [ norm(z) = x^2 + y^2 = abs(z)^2 ]
r * e^ia  : polar(r,a) = r * exp(pt(0,a)
x - iy    : conj(z)
for complex<ll>, avoir norm(z) and use dot(z,z) instead for better precision */
int main(){
  ios::sync_with_stdio(0);
  cin.tie(0);

  return 0;
}
```

