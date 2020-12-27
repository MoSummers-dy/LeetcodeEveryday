---
description: '#Array #Two Pointers'
---

# 287 Find the Duplicate Number

_Written: 2020-12-27_

Given an array of integers `nums` containing `n + 1` integers where each integer is in the range `[1, n]` inclusive.

There is only **one duplicate number** in `nums`, return _this duplicate number_.

## Method 1: Count and Find

The basic idea is to count the occurences of each letter, then pick the number that appears more than once.

```java
public int findDuplicate(int[] nums) {
    int[] freqs = new int[nums.length];
    // put into different buckets
    for (int num : nums) {
        freqs[num]++;
    }
    for (int i = 0; i < freqs.length; i++) {
        if (freqs[i] > 1) {
            return i;
        }
    }
    return -1;
}
```

Time Complexity: O\(n\)

Space Complexity: O\(n\)

## Method 2: Sorting

```java
public int findDuplicate(int[] nums) {
    Arrays.sort(nums);
    for (int i = 1; i < nums.length; i++) {
        if (nums[i] == nums[i - 1]) {
            return nums[i];
        }
    }
    return -1;
}
```

Time Compexity: O\(n log n\) because of the sorting, the later for loop is O\(n\) only.

Space Complexity: O\(1\) because the sorting is in-place. If we cannot modify the input array, then the space needed is O\(n\)

## Method 3: Set

Using the property that set cannot have duplicates.

```java
public int findDuplicate(int[] nums) {
    Set<Integer> set = new HashSet<>();
    for (int i = 0; i < nums.length; i++) {
        if (set.contains(nums[i])) {
            return nums[i];
        }
        set.add(nums[i]);
    }
    return -1;
}
```

Time Complexity: O\(n\)

Space Complexity: O\(n\)

## **Method 4: Floyd's Tortoise and Hare \(Cycle Detection\)**

I didn't come up with this, but I think it is good to mention since it has a time complexity of `O(n)` and space of `O(1)`.

The basic idea is from [Leetcode 141](https://leetcode.com/problems/linked-list-cycle-ii/): where we have two pointers to find the entrance of the cycle.

 We are chaining everything up using the function `f(x) = nums[x]`. So we will have a sequence of  `x, nums[x], nums[nums[x]], nums[nums[nums[x]]], ...`.

We know this would form a cycle because we are guarantted that the array has a duplicate, so at least two entries would point to the same place in the array. The duplicate node would be a cycle entrance.

Two pointers are used, one goes two steps each time, and the other goes one step each time.

```java
public int findDuplicate(int[] nums) {
        if (nums.length == 0) {
            return -1;
        }
        
        int fast = nums[0];
        int slow = nums[0];
        
        // Phase 1: Find the intersection
        do {
            fast = nums[nums[fast]];
            slow = nums[slow];
        } while (fast != slow);
        
        // Phase 2: Meet at the cycle entrance
        slow = nums[0];
        while (fast != slow) {
            fast = nums[fast];
            slow = nums[slow];
        }
        return slow;
    }
```

Time Compexity: O\(n\)

Space Complexity: O\(1\)

