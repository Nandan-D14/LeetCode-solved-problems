
# 1493. Longest Subarray of 1's After Deleting One Element

## 📌 Problem Statement
You are given a **binary array** `nums`.  
You must delete **exactly one element** from it.  

Return the **size of the longest non-empty subarray containing only 1's** in the resulting array.  
Return `0` if there is no such subarray.

LeetCode Link: [Longest Subarray of 1's After Deleting One Element](https://leetcode.com/problems/longest-subarray-of-1s-after-deleting-one-element/)

---

## 🔎 Examples

### Example 1
```

Input: nums = \[1,1,0,1]
Output: 3
Explanation: Delete the element at index 2. Resulting array \[1,1,1], length = 3

```

### Example 2
```

Input: nums = \[0,1,1,1,0,1,1,0,1]
Output: 5
Explanation: Delete element at index 4. Resulting array \[1,1,1,1,1] length = 5

```

### Example 3
```

Input: nums = \[1,1,1]
Output: 2
Explanation: Must delete one element. Resulting array length = 2

````

---

## 💡 Hints & Approach

1. **Sliding Window Technique:**
   - Maintain a window containing **at most 1 zero**.
   - Count length of window minus one (simulate deleting one element).

2. **Steps:**
   - Initialize `left = 0`, `zero_count = 0`, `max_len = 0`.
   - Move `right` pointer across the array.
   - If `nums[right] == 0`, increment `zero_count`.
   - If `zero_count > 1`, move `left` until `zero_count <= 1`.
   - Update `max_len = max(max_len, right - left)`.

3. **Time Complexity:** `O(n)`  
4. **Space Complexity:** `O(1)`

---

## 🧑‍💻 Python Solution

```python
class Solution:
    def longestSubarray(self, nums: List[int]) -> int:
        left = 0
        max_len = 0
        zero_count = 0

        for right in range(len(nums)):
            if nums[right] == 0:
                zero_count += 1

            while zero_count > 1:
                if nums[left] == 0:
                    zero_count -= 1
                left += 1

            # length of window minus one (delete one element)
            max_len = max(max_len, right - left)

        return max_len
````

---

## 📂 Files in Repo

* [Solution.py](./Solution.py) → Python implementation
* [README.md](./README.md) → Problem description, explanation, and solution

---

## ✅ Notes

* This sliding window approach is **efficient** for large arrays up to length `10^5`.
* It handles **edge cases** like all ones or all zeros.

```

