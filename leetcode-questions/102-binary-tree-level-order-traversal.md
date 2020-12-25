---
description: '#Tree #BFS'
---

# 102 Binary Tree Level Order Traversal

## 102 Binary Tree Level Order Traversal

Written: 2020-12-23

Given a binary tree, return the level order traversal of its nodes' values. \(ie, from left to right, level by level\).

### Method 1: BFS with Recursion

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public List<List<Integer>> levelOrder(TreeNode root) {
        // Step 1: Set up the output
        List<List<Integer>> result = new ArrayList<>();
        
        // Special Case
        if (root == null) {
            return result;
        }
        
        bfsHelper(root, 0, result);
        return result;
    }
    
    private void bfsHelper(TreeNode node, int level, List<List<Integer>> result) {
        // check if this the first explored node at this level
        if (level == result.size()) {
            result.add(new ArrayList<Integer>());
        }
        
        // add current node to its level
        result.get(level).add(node.val);
        
        // add next levels (if not empty)
        if (node.left != null) {
            bfsHelper(node.left, level + 1, result);
        }
        
        if (node.right != null) {
            bfsHelper(node.right, level + 1, result);
        }
    }
}
```

### Method 2: BFS with Iteration  

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public List<List<Integer>> levelOrder(TreeNode root) {
        // Step 1: Set up the output
        List<List<Integer>> result = new ArrayList<>();
        
        // Special Case
        if (root == null) {
            return result;
        }
        
        // Set up a queue
        Queue<TreeNode> queue = new LinkedList<>();
        queue.add(root);
        while (!queue.isEmpty()) {
            // Step 2: Build up the list at each level
            List<Integer> currLevel = new ArrayList<>();
            
            int currLevelNum = queue.size();
            for (int i = 0; i < currLevelNum; i++) {
                TreeNode node = queue.remove();
                currLevel.add(node.val);
                
                // add next levels (if not empty)
                if (node.left != null) {
                    queue.add(node.left);
                }
                if (node.right != null) {
                    queue.add(node.right);
                }
            }
            
            // Step 3: Add current level list to the output
            result.add(currLevel);
        }
        
        return result;
    }
}
```



