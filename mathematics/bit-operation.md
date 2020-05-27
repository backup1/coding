# Bit operation

Power of 2

```cpp
a = (1<<k); // a = 2 x 2 x ... x 2; (a = 2^k)
```

Division by a power of 2

```cpp
int a;
a >>= 1; // a /= 2;
a >>= 2; // a /= 4;
a >>= k; // a /= (1<<k);
```

i-th digit \(from the end, indexed from 0\) in the binary representation

```cpp
a&(1<<i);
if(a&1){ // if a is odd number
  ...
}
```

test if a number is a power of 2

```cpp
if((x&(x-1)) == 0){ // if x is a power of 2
  ...
}
```

get the last non-zero digit in a binary representation

```cpp
x^(~x+1); // last non-zero digit
```

remove last non-zero digit

```cpp
x &= x-1; // remove last non-zero digit in the binary representation
```

builtin methods

```cpp
int x = 10;
__builtin_clz(x); // nb of leading zeros (in binary representation)
__builtin_ctz(x); // nb of tailing zeros (in binary representation)
__builtin_popcount(x); // nb of ones (in binary representation)
__builtin_parity(x); // parity of the nb of ones (in binary representation)
```

