# Mo's algorithm

{% embed url="https://www.hackerearth.com/fr/practice/notes/mos-algorithm/" %}

The algorithm is applicable if all following conditions are met:

1. Arr is not changed by queries;
2. All queries are known beforehand \(techniques requiring this property are often called “offline algorithms”\);
3. If we know Func\(\[L, R\]\), then we can compute Func\(\[L + 1, R\]\), Func\(\[L - 1, R\]\), Func\(\[L, R + 1\]\) and Func\(\[L, R - 1\]\), each in **O\(F\)** time.

Mo’s algorithm provides a way to **answer all queries in O\(\(N + Q\) \* sqrt\(N\) \* F\) time** with at least O\(Q\) additional memory.



