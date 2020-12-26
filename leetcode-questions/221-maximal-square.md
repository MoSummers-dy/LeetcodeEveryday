---
description: '#dynamic programming'
---

# 221 Maximal Square

Written: 2020-12-25

Given an `m x n` binary `matrix` filled with `0`'s and `1`'s, _find the largest square containing only_ `1`'s _and return its area_.  


## Method 1: Dynamic Programming

{% hint style="info" %}
Set up another matrix dp. dp\(i,j\) represents the side length of the maximum square whose bottom right corner is the cell with index \(i,j\) in the original matrix.
{% endhint %}

```java
class Solution {
    public int maximalSquare(char[][] matrix) {
        int rowNum = matrix.length;
        int colNum = matrix[0].length;
        
        int max = 0; // max side length
        
        // Set up matrx dp
        int[][] dp = new int[rowNum + 1][colNum + 1];
        for (int i = 1; i <= rowNum; i++) {
            for (int j = 1; j <= colNum; j++) {
                if (matrix[i - 1][j - 1] == '1') {
                    // for every entry of '1', form new squares if possible
                    dp[i][j] = Math.min(Math.min(dp[i][j - 1], dp[i-1][j]), dp[i - 1][j - 1]) + 1;
                    max = Math.max(max, dp[i][j]);
                }
            }
        }
        
        return max * max;
    }
}
```

Time Complexity: O\(mn\)

Space Complexity: O\(mn\)

## Method 2: Better Dynamic Programming

{% hint style="info" %}
Since we only care about the row before and the previous element, we can keep track of a 1D array not a 2D array.
{% endhint %}

```java
class Solution {
    public int maximalSquare(char[][] matrix) {
        int rowNum = matrix.length;
        int colNum = matrix[0].length;
        
        int max = 0; // max side length
        int prev = 0; // dp[j - 1] of the previous row
        
        // Set up matrx dp
        int[] dp = new int[colNum + 1];
        for (int i = 1; i <= rowNum; i++) {
            for (int j = 1; j <= colNum; j++) {
                int temp = dp[j]; // the entry right above
                if (matrix[i - 1][j - 1] == '1') {
                    // for every entry of '1', form new squares if possible
                    dp[j] = Math.min(Math.min(dp[j - 1], prev), dp[j]) + 1;
                    max = Math.max(max, dp[j]);
                } else {
                    dp[j] = 0;
                }
                prev = temp;
            }
        }
        
        return max * max;
    }
}
```

Time Complexity: O\(mn\)

Space Complexity: O\(n\)

