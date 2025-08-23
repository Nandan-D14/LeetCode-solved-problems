
# 📘 3197. Find the Minimum Area to Cover All Ones

## 🔹 Problem

We are given a binary grid (`m x n`, up to 30x30).
We must cover **all 1’s** in the grid using **3 non-overlapping rectangles** (touching is allowed).

👉 Goal: Minimize the **sum of their areas**.

---

## 🔹 Key Observations

1. With **1 rectangle** → trivial: bounding box of all 1’s.
2. With **2 rectangles** →

   * Since they cannot overlap, one must be either **entirely above/below** or **entirely left/right** of the other.
   * So we can check **all possible vertical or horizontal splits**.
3. With **3 rectangles** →

   * Think of it as: pick **one rectangle**, then partition the remaining 1’s into **two groups** (via vertical or horizontal split).
   * Try all possible partitions and minimize.

---

## 🔹 Preprocessing

We need a fast way to compute **bounding rectangles + area** for subsets of the grid.

👉 Trick:

* Use **prefix sums (2D cumulative sum)** to quickly check which cells contain 1’s inside a subrectangle.
* Then compute the bounding box of those 1’s → area = `(xmax - xmin + 1) * (ymax - ymin + 1)`.

---

## 🔹 Approach

1. Compute the **bounding box area of all 1’s** = baseline.
2. For 2 rectangles:

   * Try every **vertical split (column k)** → left part + right part.
   * Try every **horizontal split (row k)** → top part + bottom part.
   * Each side → bounding rectangle → area.
3. For 3 rectangles:

   * First rectangle = bounding box of all 1’s in one region.
   * Then split the **remaining ones** into two groups (same as step 2).
   * Try all possible ways.

Since `m,n ≤ 30`, brute-forcing all partitions (O(n³)) is feasible.

---

## 🔹 Python Code (Brute Force Enumeration)

```python
from functools import lru_cache
from typing import List

class Solution:
    def minimumSum(self, grid: List[List[int]]) -> int:
        m, n = len(grid), len(grid[0])

        @lru_cache(None)
        def area1(r1: int, r2: int, c1: int, c2: int) -> int:
            """Minimal area of a single rectangle covering all 1s in subgrid; 0 if none."""
            min_i, max_i, min_j, max_j = 10**9, -1, 10**9, -1
            for i in range(r1, r2 + 1):
                row = grid[i]
                for j in range(c1, c2 + 1):
                    if row[j] == 1:
                        if i < min_i: min_i = i
                        if i > max_i: max_i = i
                        if j < min_j: min_j = j
                        if j > max_j: max_j = j
            if max_i == -1:  # no ones
                return 0
            return (max_i - min_i + 1) * (max_j - min_j + 1)

        @lru_cache(None)
        def best2(r1: int, r2: int, c1: int, c2: int) -> int:
            """Minimal sum of areas using two non-overlapping rectangles to cover all 1s."""
            if area1(r1, r2, c1, c2) == 0:
                return 0  # nothing to cover
            res = float("inf")
            # vertical inner splits
            for s in range(c1, c2):
                res = min(res,
                          area1(r1, r2, c1, s) + area1(r1, r2, s + 1, c2))
            # horizontal inner splits
            for t in range(r1, r2):
                res = min(res,
                          area1(r1, t, c1, c2) + area1(t + 1, r2, c1, c2))
            return res

        ans = float("inf")

        # vertical outer splits: one side gets 1 rect, the other side gets 2 rects (both directions!)
        for c in range(n - 1):
            # left | right
            ans = min(ans,
                      area1(0, m - 1, 0, c) + best2(0, m - 1, c + 1, n - 1),
                      best2(0, m - 1, 0, c) + area1(0, m - 1, c + 1, n - 1))

        # horizontal outer splits: top | bottom
        for r in range(m - 1):
            ans = min(ans,
                      area1(0, r, 0, n - 1) + best2(r + 1, m - 1, 0, n - 1),
                      best2(0, r, 0, n - 1) + area1(r + 1, m - 1, 0, n - 1))

        # For safety (though constraints guarantee ≥3 ones)
        return 0 if ans == float("inf") else ans

```

---

## 🔹 Complexity

* Brute force partitions: O((m+n)³) in worst case.
* Grid ≤ 30 → this is feasible.

---

✅ This brute-force approach passes because constraints are small (`≤30x30`).
For larger grids, one would need smarter DP with precomputed bounding boxes.

---
