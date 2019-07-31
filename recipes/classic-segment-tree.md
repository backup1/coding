# Classic Segment Tree

## Modify one element + Query on interval

```cpp
const int N = 1e5;  // limit for array size
int n;  // array size
int t[2 * N];
// build the tree
void build(){
  for(int i = n-1; i > 0; --i) t[i] = t[i<<1] + t[i<<1|1];
}
// set value at position p
void modify(int p, int value){
  for(t[p += n] = value; p > 1; p >>= 1) t[p>>1] = t[p] + t[p^1];
}
// sum on interval [l, r)
int query(int l, int r){
  int res = 0;
  for(l += n,r += n; l < r; l >>= 1,r >>= 1){
    if(l&1) res += t[l++];
    if(r&1) res += t[--r];
  }
  return res;
}

int main(){
  scanf("%d", &n);
  for(int i = 0; i < n; ++i) scanf("%d", t + n + i);
  build();
  modify(0, 1);
  printf("%d\n", query(3, 11));
  return 0;
}
```

## Modify on interval + Query one element

```cpp
void modify(int l, int r, int value){
  for(l += n,r += n; l < r; l >>= 1,r >>= 1){
    if(l&1) t[l++] += value;
    if(r&1) t[--r] += value;
  }
}

int query(int p){
  int res = 0;
  for(p += n; p > 0; p >>= 1) res += t[p];
  return res;
}

// If at some point after modifications we need to
// inspect all the elements in the array, we can push
// all the modifications to the leaves using the
// following code. After that we can just traverse
// elements starting with index n. This way we reduce
// the complexity from O(nlog(n)) to O(n) similarly to
// using build instead of n modifications.
void push(){
  for(int i = 1; i < n; ++i){
    t[i<<1] += t[i];
    t[i<<1|1] += t[i];
    t[i] = 0;
  }
}
```

## Modify on interval + Query on interval \(Lazy propagation\)

```cpp
int h = sizeof(int)*8 - __builtin_clz(n);
int d[N];

void apply(int p, int value){
  t[p] += value;
  if(p < n) d[p] += value;
}

void build(int p){
  while(p > 1){
    p >>= 1;
    t[p] = max(t[p<<1],t[p<<1|1]) + d[p];
  }
}

void push(int p){
  for(int s = h; s > 0; --s){
    int i = p>>s;
    if(d[i] != 0){
      apply(i<<1, d[i]);
      apply(i<<1|1, d[i]);
      d[i] = 0;
    }
  }
}

void inc(int l, int r, int value){
  l += n;
  r += n;
  int l0 = l, r0 = r;
  for(; l < r; l >>= 1, r >>= 1){
    if(l&1) apply(l++, value);
    if(r&1) apply(--r, value);
  }
  build(l0);
  build(r0 - 1);
}

int query(int l, int r) {
  l += n;
  r += n;
  push(l);
  push(r - 1);
  int res = -2e9;
  for(; l < r; l >>= 1, r >>= 1){
    if(l&1) res = max(res,t[l++]);
    if(r&1) res = max(t[--r],res);
  }
  return res;
}
```

