# Las Vegas Algorithm

Like Monte Carlo, Las Vegas is another probabilistic algorithm, we iterate smartly, but also randomly to solve the question. Below is the N queen question.

blog : [http://blog.sina.com.cn/s/blog\_6982136301010q01.html](http://blog.sina.com.cn/s/blog\_6982136301010q01.html)

submit link : [https://binarysearch.com/problems/N-Queens-Puzzle](https://binarysearch.com/problems/N-Queens-Puzzle)

python code example&#x20;

```cpp
class Solution:
    from random import randint
    def __init__(self):
        self.m = {}
    def check(self,row,col):
        for r in self.m:
            if r == row or self.m[r] == col or abs(row-r) == abs(col-self.m[r]):
                return False
        return True
    def queen(self,n):
        for row in range(n):
            if row in self.m:
                continue
            ok = []
            for col in range(n):
                if self.check(row,col) == True:
                    ok.append(col)
            if len(ok) == 0:
                return False
            col = ok[randint(0,len(ok)-1)]
            self.m[row] = col
        return True
    def solve(self, matrix):
        n = len(matrix)
        for row in range(n):
            for col in range(n):
                if matrix[row][col] == 1:
                    if self.check(row,col) == False:
                        return False
                    self.m[row] = col
        init = self.m.copy()
        for _ in range(100):
            self.m = init.copy()
            if self.queen(n):
                return True
        return False
```
