---
description: '#Two Pointers #Array'
---

# 11 Container With Most Water

Written: 2020-12-24

Given `n` non-negative integers `a1, a2, ..., an` , where each represents a point at coordinate `(i, ai)`. `n` vertical lines are drawn such that the two endpoints of the line `i` is at `(i, ai)` and `(i, 0)`. Find two lines, which, together with the x-axis forms a container, such that the container contains the most water.

**Notice** that you may not slant the container.

## Method 1: Brute Force

The idea behind this method is to compare the area made by every single possible pair.

```java
class Solution {
    public int maxArea(int[] height) {
        // Set up the max result
        int max = 0;
        for (int i = 0; i < height.length; i++)
            for (int j = i + 1; j < height.length; j++) {
                // Compare each possible pair
                max = Math.max(max, Math.min(height[i], height[j]) * (j - i));
            }
        return max;
    }
}
```

## Method 2: Two Pointers

We set up two pointers and move the shorter edge every time.

{% hint style="info" %}
If we move the longer edge, then the area cannot increase, since the overall area is bounded by the shorter edge always, and we are decreasing r - l as well.
{% endhint %}

```java
class Solution {
    public int maxArea(int[] height) {
        // Set up the max result
        int max = 0;
        
        // Set up the two pointers
        int l = 0;
        int r = height.length - 1;
        while (l < r) {
            max = Math.max(max, Math.min(height[l], height[r]) * (r - l));
            // Move the shorter edge to achieve a larger area
            if (height[l] < height[r]) {
                l++;
            } else {
                r--;
            }
        }
        return max;
    }
}
```

