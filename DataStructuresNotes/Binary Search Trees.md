Computer networks
04-07-2025

Binary search tree - Binary tree sorting items, and keys at its internal nodes that satisfy the following
- u, v, and w are nodes
- u is the left subtree of v
- w is the right subtree of v
- key(u) <= key(v) <= key(w)
i.e.
	 2 (v)
	 /\
	/  \
  1(u)     4(w)

Binary search trees visit keys in increasing order.

Lets say we want to find the lowest value, then we go all the way to the lefto

In a binary search tree a:
> predecessor is the largest connected value that is LESS than the parent node
> successor is the lowest connected value that is GREATER than the parent node

Lets say your parent node gets deleted, out of the predecessor and the successor the one closest to the node value is what replaces it.

Example problems:
Assuming that we have a recursive function int rangeSumBST(TreeNode* root, int low, int high) to traverse the tree, what should be the base:

```cpp 
class Solution {
public:
   int rangeSumBST(TreeNode* root, int low, int high) {
       if (root == nullptr) {
           return 0;
       }
       
       if (root->val >= low && root->val <= high) {
           return root->val + rangeSumBST(root->left, low, high) + rangeSumBST(root->right, low, high);
       }
       
       else if (root->val < low) {
           return rangeSumBST(root->right, low, high);
       }
       
       else {
           return rangeSumBST(root->left, low, high);
       }
   }
};
```

Another problem:
Given the root of a Binary Search Tree (BST), return the minimum absolute  
difference between the values of any two different nodes in the tree. A tree node  
is defined as follows

```cpp
class Solution {
public:
    int getMinimumDifference(TreeNode* root) {
        int minDiff = INT_MAX;
        int prevVal = -1;
        
        inOrderTraversal(root, prevVal, minDiff);
        
        return minDiff;
    }
    
private:
    void inOrderTraversal(TreeNode* root, int& prevVal, int& minDiff) {
        if (root == nullptr) {
            return;
        }
        
        inOrderTraversal(root->left, prevVal, minDiff);
        
        if (prevVal != -1) {
            minDiff = min(minDiff, root->val - prevVal);
        }
        prevVal = root->val;
        
        inOrderTraversal(root->right, prevVal, minDiff);
    }
};
```