---
description: '#tree traversal #morris traversal'
---

# 94 Binary Tree Inorder Traversal

Written: 2020-12-29

Given the `root` of a binary tree, return _the inorder traversal of its nodes' values_.

## Method 1: Recursion

```java
class Solution {
    // recall that inorder goes: left -> self -> right
    public List<Integer> inorderTraversal(TreeNode root) {
        // create the output list
        List<Integer> result = new ArrayList<>();
        inorderHelper(root, result);
        return result;
    }
    
    private void inorderHelper(TreeNode node, List<Integer> output) {
        // if the node is null, skip it
        if (node!= null) {
            // explore left
            inorderHelper(node.left, output);
            // self
            output.add(nodeot.val);
            // explore right
            inorderHelper(node.right, output);
        }
    }
}
```

* Time complexity: O\(n\)
* Space complexity: O\(n\).

## Method 2: Iterative

Same idea, but using a stack instead.

```java
class Solution {
    // recall that inorder goes: left -> self -> right
    public List<Integer> inorderTraversal(TreeNode root) {        
        List<Integer> result = new ArrayList<>();
        Stack<TreeNode> stack = new Stack<>();
    
        TreeNode curr = root;
        while (curr != null || !stack.isEmpty()) {
            // Explore left
            while (curr != null) {
                stack.push(curr);
                curr = curr.left;
            }
            curr = stack.pop();
            // Self
            result.add(curr.val);
            // Explore right
            curr = curr.right;
        }
        return result;
    }
}
```

* Time complexity: O\(n\)
* Space complexity: O\(n\).

## Method 3: Morris Traversal

In this method, we are going to use a new data structure: threaded binary tree.

{% embed url="https://www.educative.io/edpresso/what-is-morris-traversal" caption="Read More about Morris Traversal" %}

1. Initialize current as root
2. while the current is not null
   1. if the current has a left child
      1. in the current's left subtree, make current the right child of the rightmost node
      2. go to this left child: current = current.left
   2. if the current has no left child
      1. add current's value to the result list
      2. go to the right: current = current.right

{% hint style="info" %}
The end picture of this tree is leaning \(AKA, looks like a line\).
{% endhint %}

The whole point of Morris traversal is to eliminate the need for extra space.

```java
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        // Initialize current as root
        TreeNode current = root;
        TreeNode pre;
        while (current != null) {
            if (current.left != null) {
                //if the current has a left child
                pre = current.left;
                // find the rightmost node
                while (pre.right != null) {
                    pre = pre.right;
                }
                // make current the right child of the rightmost node
                pre.right = current;
                // go to this left child: current = current.left
                TreeNode temp = current;
                current = current.left;
                temp.left = null;// avoid infinite loop
            } else {
                // if the current has no left child
                result.add(current.val);
                current = current.right;
            }
        }
        return result;
    }
}
```

* Time complexity: O\(n\)
* Space complexity: O\(1\).

