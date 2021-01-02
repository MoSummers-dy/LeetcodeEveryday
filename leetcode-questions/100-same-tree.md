---
description: '#tree'
---

# 100 Same Tree

Written: 2020-12-31

Given the roots of two binary trees `p` and `q`, write a function to check if they are the same or not.

Two binary trees are considered the same if they are structurally identical, and the nodes have the same value.

## Method 1: Recursive Approach

```java
class Solution {
    public boolean isSameTree(TreeNode p, TreeNode q) {
        if (p == null && q == null) {
            return true;
        } else if (p == null || q == null) {
            return false;
        } else if (p.val == q.val) {
            return isSameTree(p.left, q.left) && isSameTree(p.right, q.right);
        } 
        return false;
    }
}
```

## Method 2: Iterative Approach

```java
// Initial Method
public boolean isSameTree(TreeNode p, TreeNode q) {
        Queue<TreeNode> pQueue = new LinkedList<TreeNode>();
        Queue<TreeNode> qQueue = new LinkedList<TreeNode>();
        
        if (p != null) {
            pQueue.add(p);
        }
        
        if (q != null) {
            qQueue.add(q);
        }
        
        while (pQueue.size() != 0 && qQueue.size() == pQueue.size()) {
            TreeNode pNode = pQueue.remove();
            TreeNode qNode = qQueue.remove();
            if (pNode.val != qNode.val) {
                return false;
            }
            if (pNode.left != null) {
                pQueue.add(pNode.left);
            }
            if (qNode.left != null) {
                qQueue.add(qNode.left);
            }
            if (pQueue.size() != qQueue.size()) {
                return false;
            }
            if (pNode.right != null) {
                pQueue.add(pNode.right);
            }
            
            if (qNode.right != null) {
                qQueue.add(qNode.right);
            }
        }
        
        if (pQueue.size() != qQueue.size()) {
            return false;
        }
        
        return true;
    }
```

The method above is wordy. Let me simplfy it by using one queue only, and add all the nodes \(including the `nulls`\).

```java
public boolean isSameTree(TreeNode p, TreeNode q) {
        if (p == null && q == null) {
            return true;
        }
        else if (p == null || q == null) {
            return false;
        }
        
        Queue<TreeNode> queue = new LinkedList<>();

        if (p != null && q != null) {
            queue.add(p);
            queue.add(q);
        }
        while (!queue.isEmpty()) {
            TreeNode first = queue.remove();
            TreeNode second = queue.remove();
            
            if (first == null && second == null) {
                continue;
            }
            if (first == null || second == null) {
                return false;
            }
            if (first.val != second.val) {
                return false;
            }
            queue.add(first.left);
            queue.add(second.left);
            queue.add(first.right);
            queue.add(second.right);
        }
        return true;
    }
```

