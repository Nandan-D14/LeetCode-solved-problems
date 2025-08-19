
# LeetCode Problem: Median of Two Sorted Arrays

## 📌 Problem Statement
You are given two sorted arrays `nums1` and `nums2` of size `m` and `n` respectively.  
Return **the median** of the two sorted arrays.  
The overall run time complexity should be `O(log (m+n))` (for the optimal solution).  

---

## 🔎 Example

### Example 1:
```

Input: nums1 = \[1,3], nums2 = \[2]
Output: 2.0
Explanation: merged array = \[1,2,3], median = 2

```

### Example 2:
```

Input: nums1 = \[1,2], nums2 = \[3,4]
Output: 2.5
Explanation: merged array = \[1,2,3,4], median = (2+3)/2 = 2.5

````

---

## 💡 Hints & Approach
1. Merge both arrays into a single array.
2. Sort the merged array.
3. If the merged length is odd → pick the middle element.  
   If even → average of the two middle elements.
4. **Note:** This approach is simple `O((m+n) log(m+n))`, not the optimal logarithmic solution,  
   but works for understanding and passing test cases.

---

## 🧑‍💻 Java Solution

```java
// Java
import java.util.Arrays;

class Solution {
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
        int[] grandArray = new int[nums1.length + nums2.length];

        System.arraycopy(nums1, 0, grandArray, 0, nums1.length);
        System.arraycopy(nums2, 0, grandArray, nums1.length, nums2.length);

        Arrays.sort(grandArray);

        int len = grandArray.length;

        if (len % 2 != 0) {
            // Odd length → return middle element
            return grandArray[len / 2];
        } else {
            // Even length → average of two middle elements
            return (grandArray[len / 2 - 1] + grandArray[len / 2]) / 2.0;
        }
    }
}
````

---

## ✅ Complexity Analysis

* **Time Complexity:** `O((m+n) log(m+n))` (due to sorting)
* **Space Complexity:** `O(m+n)` (for merged array)

---
