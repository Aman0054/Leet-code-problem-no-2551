# Put Marbles in Bags

## Problem Statement
You have `k` bags and a 0-indexed integer array `weights`, where `weights[i]` represents the weight of the `i`th marble. The marbles must be divided into `k` bags according to these rules:

1. No bag can be empty.
2. If the `i`th marble and `j`th marble are in a bag, all marbles with indices between `i` and `j` must also be in that bag.
3. If a bag contains marbles from index `i` to `j` inclusively, its cost is calculated as `weights[i] + weights[j]`.
4. The score after distributing the marbles is the sum of the costs of all `k` bags.

The task is to return the **difference** between the maximum and minimum scores possible for any valid distribution.

### Example 1:
**Input:**  
```plaintext
weights = [1,3,5,1], k = 2
```
**Output:**  
```plaintext
4
```
**Explanation:**  
- Minimum score distribution: `[1], [3,5,1]` → Score = `(1+1) + (3+1) = 6`
- Maximum score distribution: `[1,3], [5,1]` → Score = `(1+3) + (5+1) = 10`
- Difference: `10 - 6 = 4`

### Example 2:
**Input:**  
```plaintext
weights = [1, 3], k = 2
```
**Output:**  
```plaintext
0
```
**Explanation:**  
The only valid way to split the marbles is `[1], [3]`. Both max and min scores are the same, so the difference is `0`.

---

## Solution Approach

### **Intuition**
- Since the marbles must be placed in **continuous** groups, the key to solving this problem lies in finding the best way to split the array.
- The cost of each bag depends on the **first and last elements** in the subarray, meaning that splits influence the final cost.
- Instead of focusing on actual partitions, we observe that **adjacent element sums** determine how we can split the array.

### **Steps to Solve**
1. Compute an array of `adjacentSums` where each element is `weights[i] + weights[i + 1]`. This represents possible breakpoints between groups.
2. Sort this `adjacentSums` array.
3. The **minimum sum** is obtained by selecting the smallest `k-1` adjacent sums.
4. The **maximum sum** is obtained by selecting the largest `k-1` adjacent sums.
5. The final answer is the difference between the maximum and minimum sums.

### **Code Implementation (Java)**
```java
import java.util.*;

class Solution {
     static public long putMarbles(int[] weights, int k) {
        int n = weights.length;
        int[] adjacentSums = new int[n - 1];
        for (int i = 0; i < n - 1; i++) adjacentSums[i] = weights[i] + weights[i + 1];
        Arrays.sort(adjacentSums);
        long minSum = 0, maxSum = 0;
        for (int i = 0; i < k - 1; i++) minSum += adjacentSums[i];
        for (int i = n - 2; i >= n - k; i--) maxSum += adjacentSums[i];
        return maxSum - minSum;
    }
}
```

---

## Step-by-Step Explanation
1. **Compute adjacent sums**: We create an array where each element represents the sum of two consecutive marbles (`weights[i] + weights[i+1]`).
2. **Sort the adjacent sums**: Sorting allows us to extract the smallest and largest values efficiently.
3. **Find the minimum score**: We take the `k-1` smallest sums.
4. **Find the maximum score**: We take the `k-1` largest sums.
5. **Compute the difference**: The final result is `maxSum - minSum`.

### **Time Complexity**
- **Computing adjacent sums:** `O(n)`
- **Sorting the array:** `O(n log n)`
- **Summing up values:** `O(k)`
- **Total Complexity:** `O(n log n)` (due to sorting being the dominant operation)

### **Space Complexity**
- **Extra storage for adjacent sums:** `O(n)`, but since sorting is in-place, we only use `O(n)` additional space.




