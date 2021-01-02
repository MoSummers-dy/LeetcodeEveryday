---
description: '#array #hash table'
---

# 1 Two Sum

Written: 2021-01-01

Given an array of integers `nums` and an integer `target`, return _indices of the two numbers such that they add up to `target`_.

You may assume that each input would have _**exactly**_ **one solution**, and you may not use the _same_ element twice.

You can return the answer in any order.

## Method 1: Brute Force

find every possible pair and do the check.

```java
public int[] twoSum(int[] nums, int target) {
    for (int i = 0; i < nums.length; i++) {
        for (int j = i + 1; j < nums.length; j++) {
            if (nums[j] == target - nums[i]) {
                return new int[] { i, j };
            }
        }
    }
    return null;
}
```

## Method 2: Hash Table

```java
public int[] twoSum(int[] nums, int target) {
        Map<Integer, Integer> remain = new HashMap<>();
        for (int i = 0; i < nums.length; i++) {
            if (remain.containsKey(nums[i])) {
                return new int[]{remain.get(nums[i]), i};
            }
            // we put in what remains, so if it appears there is a valid pair
            remain.put(target - nums[i], i);
        }
        return null;
    }
```

Nothing really special about this question. But we can achieve an optimal runtime of O\(n\).

