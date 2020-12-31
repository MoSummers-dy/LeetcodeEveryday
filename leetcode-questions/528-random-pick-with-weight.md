---
description: '#binary search'
---

# 528 Random Pick with Weight

Written: 2020-12-30

Original Question:

You are given an array of positive integers `w` where `w[i]` describes the weight of `ith` index \(0-indexed\).

We need to call the function `pickIndex()` which **randomly** returns an integer in the range `[0, w.length - 1]`. `pickIndex()` should return the integer proportional to its weight in the `w` array. For example, for `w = [1, 3]`, the probability of picking the index `0` is `1 / (1 + 3) = 0.25` \(i.e 25%\) while the probability of picking the index `1` is `3 / (1 + 3) = 0.75` \(i.e 75%\).

More formally, the probability of picking index `i` is `w[i] / sum(w)`.

### [More explanation: ](https://leetcode.com/problems/random-pick-with-weight/discuss/671445/Question-explained)

The problem is, we need to randomly pick an index proportional to its weight. What does his mean? We have a weights array, each weights\[i\] represents the weight of index i. The more the weight is, the higher chances of getting that index randomly.

Suppose weights = \[1, 3\] then 3 is larger, so there are high chances to get index 1.

We can know the chances of selecting each index by knowing their probability.

`P(i) = weight[i]/totalWeight`

totalWeight = 1 + 3 = 4. So, for index 0, P\(0\) = 1/4 = 0.25 = 25% for index 1, P\(1\) = 3/4 = 0.75 = 75%

So, there are 25% of chances to pick index 0 and 75% chances to pick index 1.

## Method 1: Prefix Sum with Linear Search

```java
class Solution {
    private int[] prefixSum; // prefix sum
    private int sumWeights; // sum of all the weights

    public Solution(int[] w) {
        // create an array for prefix sum (length matches)
        prefixSum = new int[w.length];
        int sum = 0;
        for (int i = 0; i < w.length; i++) {
            sum += w[i];
            prefixSum[i] = sum;
        }
        sumWeights = sum;
    }
    
    public int pickIndex() {
        // randomly pick a number from all possible weights
        double target = this.sumWeights * Math.random();
        // find the specific value
        for (int i = 0; i < prefixSum.length; i++) {
            if (target < prefixSum[i]) {
                return i;
            }
        }
        return 0;
    }
}
```

## Method 2: Prefix Sum with Binary Search

Because the array is sorted, we can make it simple by doing a binary search to save some time.

```java
class Solution {
    private int[] prefixSum; // prefix sum
    private int sumWeights; // sum of all the weights

    public Solution(int[] w) {
        // create an array for prefix sum (length matches)
        prefixSum = new int[w.length];
        int sum = 0;
        for (int i = 0; i < w.length; i++) {
            sum += w[i];
            prefixSum[i] = sum;
        }
        sumWeights = sum;
    }
    
    public int pickIndex() {
        // randomly pick a number from all possible weights
        double target = this.sumWeights * Math.random();
        // find the specific value
        int l = 0;
        int h = prefixSum.length - 1;
        while (l <= h) {
            int mid = l + (h - l) / 2;
            if (target > prefixSum[mid]) {
                l = mid + 1;
            } else {
                h = mid - 1;
            }
        }
        return l;
    }
}
```

{% hint style="info" %}
* if you use `while (lo <= hi)` you use `lo=mid+1` and `hi=mid-1` 
* if you use `while (lo < hi)` you use `lo = mid+1` and `hi=mid`
{% endhint %}



