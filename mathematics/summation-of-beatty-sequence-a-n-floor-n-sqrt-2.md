# Summation of Beatty sequence: a\(n\) = floor\(n\*sqrt\(2\)\)

Solution link: [https://math.stackexchange.com/questions/2052179/how-to-find-sum-i-1n-left-lfloor-i-sqrt2-right-rfloor-a001951-a-beatty-s](https://math.stackexchange.com/questions/2052179/how-to-find-sum-i-1n-left-lfloor-i-sqrt2-right-rfloor-a001951-a-beatty-s)

```python
def getSqrt(n):
    left = 1
    right = n
    while left+1 < right:
        mid = (left+right)//2
        if mid*mid <= n:
            left = mid
        else:
            right = mid-1
    if left == right:
        return left
    if right*right > n:
        return left
    return right

def getAns(n):
    if n <= 1:
        return n
    n2 = getSqrt(2*n*n)-n
    return n*n2+n*(n+1)//2-n2*(n2+1)//2-getAns(n2)

print(getAns(10**100))
```



