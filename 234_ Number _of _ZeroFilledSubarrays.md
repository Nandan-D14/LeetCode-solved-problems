
# 2348. Number of Zero-Filled Subarrays

## 📌 Problem Statement
You are given an integer array `nums`.  
A **subarray** is a contiguous non-empty sequence of elements within an array.  

Your task is to return the **number of subarrays filled with only `0`**.

### Example 1:
**Input:**  
`nums = [1,3,0,0,2,0,0,4]`  

**Output:**  
`6`  

**Explanation:**  
- `[0]` occurs **4** times  
- `[0,0]` occurs **2** times  
- No subarray larger than 2 exists with only 0s.  
So, total = 6  

---

### Example 2:
**Input:**  
`nums = [0,0,0,2,0,0]`  

**Output:**  
`9`  

**Explanation:**  
- `[0]` occurs **5** times  
- `[0,0]` occurs **3** times  
- `[0,0,0]` occurs **1** time  
So, total = 9  

---

### Example 3:
**Input:**  
`nums = [2,10,2019]`  

**Output:**  
`0`  

---

## 🎯 Constraints
- `1 <= nums.length <= 10^5`
- `-10^9 <= nums[i] <= 10^9`

---

## 💡 Hints
1. For each zero, calculate the number of **zero-filled subarrays ending at that index**.
2. Maintain a counter for **consecutive zeros**.

---

## 🔑 Approach
- Traverse the array while keeping track of consecutive zeros.  
- If `nums[i] == 0`, increment the zero streak counter.  
- Each time you find a zero, the number of new subarrays ending at that zero is equal to the current streak length.  
- Add this to the result.  
- If `nums[i] != 0`, reset the streak counter.  

This avoids extra loops and list storage — achieving **O(n) time** and **O(1) space**.

---

## ✅ Optimized Solution (Java)
```java
// Java
class Solution {
    public long zeroFilledSubarray(int[] nums) {
        long result = 0;
        long count = 0;

        for (int num : nums) {
            if (num == 0) {
                count++;           // extend streak of zeros
                result += count;   // add subarrays ending here
            } else {
                count = 0;         // reset streak
            }
        }

        return result;
    }
}
````

---

## ⚡ Complexity

* **Time Complexity:** `O(n)` – single pass through the array.
* **Space Complexity:** `O(1)` – only counters used.

---

## 🔍 Your Previous Approach vs Optimized

* **Your Approach:**

  * You tried to find blocks of zeros and count subarrays separately.
  * Involved extra inner loops and lists (`continuosCountOfZeroList`) → unnecessary overhead.
  * Time complexity: close to `O(n^2)` in worst case.

* **Optimized Approach:**

  * Directly counts subarrays as we traverse, without storing intermediate lists.
  * Always **O(n)**.
  * Cleaner and shorter code.

---

## 🏆 Key Takeaway

👉 Instead of post-processing segments of zeros, track **running streaks** and count subarrays incrementally. This is the most efficient way for this problem.

