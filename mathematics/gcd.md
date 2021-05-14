---
description: 'GCD who treats edge cases for ex. a =0, b = 0'
---

# GCD

```cpp
int gcd(int a, int b){
  if(a > b) swap(a,b);
  if(a == b) return a;
  if(a == 0) return b;
  return gcd(a,b%a);
}
```

