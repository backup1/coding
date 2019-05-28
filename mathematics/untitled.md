# Power

## Modular Power

```cpp
const long long MOD = 1e9+7;
long long getPow(long long a,long long p){
  long long ret = 1,cp = a;
  while(p){
    if(p&1) ret = (ret*cp)%MOD;
    p >>= 1;
    cp = (cp*cp)%MOD;
  }
  return ret;
}
```

## Modular Inverse

```cpp
const long long MOD = 1e9+7; // prime number
long long getPow(long long a,long long p){
  long long ret = 1,cp = a;
  while(p){
    if(p&1) ret = (ret*cp)%MOD;
    p >>= 1;
    cp = (cp*cp)%MOD;
  }
  return ret;
}
long long getInverse(long long a){
  return getPow(a,MOD-2)%MOD;
}
```

