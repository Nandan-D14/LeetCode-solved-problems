
# Maximum 69 Number (LeetCode 1323)

## 📘 Problem Statement
You are given a positive integer `num` consisting only of digits **6** and **9**.

Return the **maximum number** you can get by changing **at most one digit**:
- You can change a `6` into a `9`, or
- You can change a `9` into a `6`.

### Example 1
**Input:**  
```

num = 9669

```
**Output:**  
```

9969

```
**Explanation:**  
- Change the first digit → `6669`  
- Change the second digit → `9969` ✅  
- Change the third digit → `9699`  
- Change the fourth digit → `9666`  
The maximum number is `9969`.

---

### Example 2
**Input:**  
```

num = 9996

```
**Output:**  
```

9999

```
**Explanation:**  
Changing the last digit `6` to `9` gives the maximum number.

---

### Example 3
**Input:**  
```

num = 9999

```
**Output:**  
```

9999

````
**Explanation:**  
No change is better.

---

## ✅ Constraints
- `1 <= num <= 10^4`  
- `num` consists only of digits `6` and `9`.  

---

## 💡 Approach
We want to maximize the number with **at most one change**.
- The **leftmost digit** contributes the most to the number's value.  
- So, change the **first `6`** (from left to right) into a `9`.  
- If no `6` exists, return the number as-is.  

This greedy strategy ensures maximum result.

---

## 🚀 Java Solution

```java
// java
class Solution {
    public int maximum69Number (int num) {
        char[] digits = String.valueOf(num).toCharArray();
        
        for (int i = 0; i < digits.length; i++) {
            if (digits[i] == '6') {
                digits[i] = '9';
                break; 
            }
        }
        
        return Integer.parseInt(new String(digits));
    }
}
````

---

## ⏱️ Complexity Analysis

* **Time Complexity:** `O(N)` (where `N` is the number of digits, at most 5 since `num <= 10^4`).
* **Space Complexity:** `O(N)` for the char array.

---

## 🔥 Alternate Math-only Approach (No Strings)

```java
// java
class Solution {
    public int maximum69Number (int num) {
        int temp = num;
        int index = -1, pos = 0;

        while (temp > 0) {
            if (temp % 10 == 6) {
                index = pos; 
            }
            temp /= 10;
            pos++;
        }

        if (index == -1) return num;

        return num + 3 * (int)Math.pow(10, index);
    }
}
```
