
# 📘 1504. Count Submatrices With All Ones

## 🔹 Problem

You are given an `m x n` binary matrix `mat`.
Return the **number of submatrices that contain only `1`s**.

---

## 🔹 Example

### Example 1

```
Input:
mat = [[1,0,1],
       [1,1,0],
       [1,1,0]]

Output: 13
```

Explanation breakdown:

* 6 rectangles of size `1x1`
* 2 rectangles of size `1x2`
* 3 rectangles of size `2x1`
* 1 rectangle of size `2x2`
* 1 rectangle of size `3x1`

Total = 13 ✅

---

## 🔹 Intuition

* For each row, think of it as the **base** of rectangles.
* At each position `(i, j)` count how many submatrices end at `(i, j)`.

👉 Trick:
We keep track of **consecutive 1’s height** for each column (like a histogram).

Example for:

```
mat = [
 [1,0,1],
 [1,1,0],
 [1,1,0]
]
```

We build heights row by row:

```
Row 0: [1,0,1]
Row 1: [2,1,0]
Row 2: [3,2,0]
```

Now, for each row:

* Treat heights as "bars" in a histogram.
* At each column `j`, count rectangles ending at column `j` by extending left while keeping track of the **minimum height**.

---

## 🔹 Algorithm (O(m \* n²))

1. Build an array `height[j]` for each row:

   * If `mat[i][j] == 1` → `height[j] += 1`
   * Else → `height[j] = 0`
2. For each row, for each column `j`, expand leftwards:

   * Track the minimum height seen so far.
   * Add it to count (this gives rectangles ending at `(i, j)`).

---

## 🔹 Python Code

```python
# Python
from typing import List

class Solution:
    def numSubmat(self, mat: List[List[int]]) -> int:
        m, n = len(mat), len(mat[0])
        height = [0] * n
        total = 0
        
        for i in range(m):
            # update heights
            for j in range(n):
                if mat[i][j] == 1:
                    height[j] += 1
                else:
                    height[j] = 0
            
            # count submatrices ending at row i
            for j in range(n):
                if height[j] > 0:
                    min_h = height[j]
                    for k in range(j, -1, -1):   # expand left
                        if height[k] == 0:
                            break
                        min_h = min(min_h, height[k])
                        total += min_h
                        
        return total
```

---

## 🔹 Complexity

* Time: **O(m \* n²)** → acceptable since `n ≤ 150`
* Space: **O(n)**

---

✅ This method works because at each row we convert the problem into **counting rectangles in histograms**, which is a classic trick.

---
