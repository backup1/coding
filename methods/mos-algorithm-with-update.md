# Mo's algorithm with update

We need to work with bloc size of $$n^{\frac{2}{3}}$$, so that all queries will be split into $$n^{\frac{2}{3}}$$groups. The complexity is $$O(n^{\frac{5}{3}})$$

The comparison function used to sort all queries is the following

```cpp
inline bool mo_cmp(const query& q1,const query& q2){
  int b1 = q1.left/BLOCK, b2 = q2.left/BLOCK;
  if(b1 != b2) return b1 < b2;
  int c1 = q1.right/BLOCK, c2 = q2.right/BLOCK;
  if(c1 != c2) return (b1&1) ? (c1 < c2) : (c2 < c1);
  return q1.time < q2.time;
}
```

All updates are stored elsewhere using timestamp.

