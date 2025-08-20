# 1277. Count Square Submatrices with All Ones

## 📌 Problem Restated

We are given a binary matrix (only `0`s and `1`s).
We need to count how many **square submatrices** (1x1, 2x2, 3x3, …) contain only `1`s.

---

## 🔹 Brute Force (O(n³))

* Check every possible square submatrix → for each size `k`, check if all entries are `1`.
* Too slow (`300 x 300` → O(27M) checks).

We need **DP** to optimize.

---

## 🔹 Key DP Idea

We define a DP table `dp[i][j]`:

👉 `dp[i][j] = size of the largest square submatrix with bottom-right corner at (i, j)`

That means:

* If `matrix[i][j] == 0` → `dp[i][j] = 0`
* If `matrix[i][j] == 1` →

```
dp[i][j] = 1 + min(dp[i-1][j], dp[i][j-1], dp[i-1][j-1])
```

Why?
Because:

* `dp[i-1][j]` = max square size ending **above**
* `dp[i][j-1]` = max square size ending **left**
* `dp[i-1][j-1]` = max square size ending **top-left**

So the current cell can extend the smallest of those three.

---

## 🔹 Example Walkthrough

Matrix:

```
[
  [0,1,1,1],
  [1,1,1,1],
  [0,1,1,1]
]
```

Step-by-step filling `dp`:

1. First row/column → copy from matrix (since only 1x1 possible).

```
dp = [
  [0,1,1,1],
  [1,?, ?, ?],
  [0,?, ?, ?]
]
```

2. For (1,1): `matrix[1][1]=1`, so:

```
dp[1][1] = 1 + min(dp[0][1], dp[1][0], dp[0][0])
          = 1 + min(1,1,0)
          = 1
```

3. For (1,2): `matrix[1][2]=1`

```
dp[1][2] = 1 + min(dp[0][2], dp[1][1], dp[0][1])
          = 1 + min(1,1,1)
          = 2
```

4. Similarly we fill all cells.

Final `dp`:

```
[
  [0,1,1,1],
  [1,1,2,2],
  [0,1,2,3]
]
```

Now sum all `dp[i][j]` = **15** ✅

---

## 🔹 Python Code

```python
# Python
from typing import List

class Solution:
    def countSquares(self, matrix: List[List[int]]) -> int:
        m, n = len(matrix), len(matrix[0])
        dp = [[0]*n for _ in range(m)]
        total = 0
        
        for i in range(m):
            for j in range(n):
                if matrix[i][j] == 1:
                    if i == 0 or j == 0:  # first row or column
                        dp[i][j] = 1
                    else:
                        dp[i][j] = 1 + min(dp[i-1][j], dp[i][j-1], dp[i-1][j-1])
                    
                    total += dp[i][j]   # add number of squares ending here
        
        return total
```

---

## 🔹 Complexity

* Time: **O(m \* n)** (each cell computed once)
* Space: **O(m \* n)** (can reduce to O(n) if optimized)

---

✅ That’s why DP works beautifully here:
Instead of checking each square explicitly, we just accumulate the **maximum square size** possible at each point.

Want can **optimize space from O(m·n) → O(n)** using just 1D DP
