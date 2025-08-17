
# 🎲 LeetCode - New 21 Game (Problem 837)

## 📌 Problem Description
Alice plays a game, loosely based on the card game **21**.

- She starts with **0 points**.
- While her score is **less than `k`**, she keeps drawing numbers.
- Each draw adds an integer between **1** and **maxPts** (inclusive), chosen uniformly at random.
- Alice stops drawing when her score is **≥ k**.
- We need to return the **probability that Alice's score is ≤ n** at the end of the game.

---

### 🔹 Example 1
```text
Input: n = 10, k = 1, maxPts = 10
Output: 1.00000
Explanation: Alice gets a single card, then stops. Always between 1–10, so probability = 1.
````

### 🔹 Example 2

```text
Input: n = 6, k = 1, maxPts = 10
Output: 0.60000
Explanation: Alice gets a single card, then stops. Out of 10 possible outcomes, 6 are ≤ 6 → probability = 0.6.
```

### 🔹 Example 3

```text
Input: n = 21, k = 17, maxPts = 10
Output: 0.73278
```

---

## 🔑 Key Idea

We use **Dynamic Programming (DP)** with a **sliding window** optimization.

* Let `dp[i]` = probability of having exactly `i` points.
* Initially `dp[0] = 1`.
* For each score `i < k`, Alice can draw `1 … maxPts`.
  Thus:

  ```
  dp[i + j] += dp[i] / maxPts   (for 1 ≤ j ≤ maxPts)
  ```
* Alice stops drawing once `i ≥ k`.
* The final answer is the sum of `dp[i]` for `k ≤ i ≤ n`.

⚡ To optimize, we use a **running sum (sliding window)** instead of updating each `dp[i+j]` separately.

---

## 🧑‍💻 Java Solution

```java
// java
class Solution {
    public double new21Game(int n, int k, int maxPts) {
        if (k == 0 || n >= k + maxPts - 1) return 1.0;

        double[] dp = new double[n + 1];
        dp[0] = 1.0;

        double windowSum = 1.0;  // sliding window sum of probabilities
        double result = 0.0;

        for (int i = 1; i <= n; i++) {
            dp[i] = windowSum / maxPts;

            if (i < k) {
                windowSum += dp[i];  // add for next states
            } else {
                result += dp[i];     // final result contribution
            }

            if (i - maxPts >= 0) {
                windowSum -= dp[i - maxPts];  // remove old value
            }
        }

        return result;
    }
}
```

---

## 📝 Complexity Analysis

* **Time Complexity:** `O(n)`
* **Space Complexity:** `O(n)`

---

## 🔗 Resources

* [LeetCode Problem Link](https://leetcode.com/problems/new-21-game/description/?envType=daily-question&envId=2025-08-17)
