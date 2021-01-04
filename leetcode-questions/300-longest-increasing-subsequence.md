---
description: '#dynamic programming'
---

# 300 Longest Increasing Subsequence

Written: 2021-01-03

Given an integer array `nums`, return the length of the longest strictly increasing subsequence.

A **subsequence** is a sequence that can be derived from an array by deleting some or no elements without changing the order of the remaining elements. For example, `[3,6,2,7]` is a subsequence of the array `[0,3,1,6,2,2,7]`.

## Method 1: **Dynamic Programming**

```java
public int lengthOfLIS(int[] nums) {
        int[] dp = new int[nums.length];
        // dp[i] = length of the longest increasing subsequence, ending at nums[i]
        int maxLen = 0;
        for (int i = 0; i < nums.length; i++) {
            // shortest subsequence is the character only, with length 1
            dp[i] = 1;
            for (int j = 0; j < i; j++) {
                if (nums[j] < nums[i]) {
                    // can append nums[i] to the end of the subsequnce ending at nums[j]
                    dp[i] = Math.max(dp[i], dp[j] + 1);
                }
            }
            // store the maximum length
            maxLen = Math.max(maxLen, dp[i]);
        }
        return maxLen;
    }
```

* Time complexity: O\(n^2\). Two loops of n are there.
* Space complexity: O\(n\). dp array of size n is used.

## Method 2: Patient Sort with Binary Sort

Patient sort: [https://youtu.be/22s1xxRvy28](https://youtu.be/22s1xxRvy28)

from _wasd5960_:

> ### This method is NOT based on binary search in DP 
>
> swipe through the array, maintain a subsequence array s of INCREASING numbers for each new element x 
>
> if x larger than s\[s.size\(\)-1\], append x to s 
>
> else find an element that's just larger than x \(whose previous is smaller than x\) replace that element with x \(above can be done with binary search\) 
>
> ### Why is this correct? 
>
> s is NOT the increasing subsequence we're looking for 
>
> **However, the length of s is the correct answer** 
>
> when we replace s\[i\] with x we don't change the length of answer, but **we changed the potential best candidate** 
>
> the idea is to try to make each position's number as small as possible 
>
> the actual sequence only changes when we append a number, otherwise it's just a "virtual change", meaning we don't change the current sequence, but we try to make each number small so we'll have a larger chance to append more numbers

```java
public int lengthOfLIS(int[] nums) {
        int[] piles = new int[nums.length]; // store the smallest number(or top card) of each pile.
        int numPiles = 0;
        for (int num: nums) {
            int destPile = Arrays.binarySearch(piles, 0, numPiles, num);
            if (destPile < 0) {
                destPile = -(destPile + 1); // Arrays.binarySearch returns -(insertion point) - 1 if not found
            }
            piles[destPile] = num;
            if (destPile == numPiles) {
                numPiles++;
            }
        }
        return numPiles;
    }
```

* Time complexity: O\(nlog n\). Binary search takes logn time and it is called n times.
* Space complexity: O\(n\). dp array of size n is used.

