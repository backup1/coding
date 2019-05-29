---
description: introduced in C++11
---

# constexpr

```cpp
constexpr int F(int n){
  if(n <= 1) return 1;
  return F(n-1) + F(n-2);
}
int main(){
  cout << F(20); // calculated by the compiler
  return 0;
}
```



