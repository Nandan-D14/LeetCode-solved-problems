
# 3195. Find the Minimum Area to Cover All Ones I

## 📌 Problem Statement
You are given a 2D binary array `grid`.  //
Find a rectangle with horizontal and vertical sides that covers **all the `1`s** in `grid` with the smallest area/.  9

Return the **minimum possible area** of the rectangle.

---

## 🔎 Example

### Example 1
```

Input: grid = \[\[0,1,0],
\[1,0,1]]

Output: 6

```
Explanation:  
The smallest rectangle covering all `1`s has width `3` and height `2`.  
So, area = `3 * 2 = 6`.

---

### Example 2
```

Input: grid = \[\[1,0],
\[0,0]]

Output: 1

```
Explanation:  
The smallest rectangle covers just the single `1`.  
So, area = `1 * 1 = 1`.

---

## 💡 Hints
1. Track the **minimum and maximum row indices** where a `1` occurs.
2. Track the **minimum and maximum column indices** where a `1` occurs.
3. The rectangle’s **height** = `maxRow - minRow + 1`.  
   The rectangle’s **width** = `maxCol - minCol + 1`.  
4. Area = `height * width`.

---

## 🧑‍💻 Approach
- Iterate over the entire grid.
- Update `minRow`, `maxRow`, `minCol`, and `maxCol` whenever a `1` is found.
- Finally compute:
```

height = maxRow - minRow + 1
width = maxCol - minCol + 1
area = height \* width

````

---

## ✅ Java Solution

```java
class Solution {
  public int minimumArea(int[][] grid) {
      int m = grid.length, n = grid[0].length;

      int minRow = m, maxRow = -1;
      int minCol = n, maxCol = -1;

      // Traverse grid to find bounds of 1's
      for (int i = 0; i < m; i++) {
          for (int j = 0; j < n; j++) {
              if (grid[i][j] == 1) {
                  minRow = Math.min(minRow, i);
                  maxRow = Math.max(maxRow, i);
                  minCol = Math.min(minCol, j);
                  maxCol = Math.max(maxCol, j);
              }
          }
      }

      int height = maxRow - minRow + 1;
      int width = maxCol - minCol + 1;

      return height * width;
  }
}
````

---

## 📊 Complexity Analysis

* **Time Complexity:** `O(m * n)` → need to scan all cells once.
* **Space Complexity:** `O(1)` → only a few variables used.

---

## 📂 Files in Repo

* [Solution.java](./Solution.java) → Java implementation
* [LeetCode source](https://leetcode.com/problems/find-the-minimum-area-to-cover-all-ones-i/description/?envType=daily-question&envId=2025-08-22) → Problem description & explanation

