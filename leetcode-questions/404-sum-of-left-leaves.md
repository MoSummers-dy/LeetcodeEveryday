---
description: '#Tree #DFS #BFS'
---

# 404 Sum of Left Leaves

Written: 2020-12-22

Find the sum of all left leaves in a given binary tree.

## Method 1: DFS with Iteration

```java
class Solution {
    public int sumOfLeftLeaves(TreeNode root) {
        // Step 1: initialize sum
        int sum = 0;
        
        // Special case
        if (root == null) {
            return sum;
        }
        
        // Step 2: Set up the stack
        Stack<TreeNode> stack = new Stack<>();
        stack.push(root);
        
        // Step 3: Traverse every node in tree
        while (!stack.isEmpty()) {
            TreeNode node = stack.pop();
            // Add to sum if it has a left leaf
            if (node.left != null && node.left.left == null && node.left.right == null) {
                sum += node.left.val;
            }
            
            if (node.left != null) {
                stack.push(node.left);
            }
            if (node.right != null) {
                stack.push(node.right);
            }
        }
        // Step 4: return the sum
        return sum;
    }
}
```

## Method 2: BFS with Iteration

```java
class Solution {
    public int sumOfLeftLeaves(TreeNode root) {
        // Step 1: initialize sum
        int sum = 0;
        
        // Special case
        if (root == null) {
            return sum;
        }
        
        // Step 2: Set up the queue
        Queue<TreeNode> queue = new LinkedList<>();
        queue.add(root);
        
        // Step 3: Traverse every node in tree
        while (!queue.isEmpty()) {
            TreeNode node = queue.remove();
            // Add to sum if it has a left leaf
            if (node.left != null && node.left.left == null && node.left.right == null) {
                sum += node.left.val;
            }
            
            if (node.left != null) {
                queue.add(node.left);
            }
            if (node.right != null) {
                queue.add(node.right);
            }
        }
        // Step 4: return the sum
        return sum;
    }
}
```

## Method 3: DFS with Recursion

```java
class Solution {
    public int sumOfLeftLeaves(TreeNode root) {
        // initialize sum
        int sum = 0;
        
        // Special case
        if (root == null) {
            return sum;
        }
        
        return helper(root, false);
    }
    
    private int helper(TreeNode node, boolean isLeft) {
        if (node == null) {
            return 0;
        }
        
        // Check for leaf node
        if (node.left == null && node.right == null) {
            return isLeft ? node.val : 0;
        }
        
        return helper(node.left, true) + helper(node.right, false);
    }
}
```

