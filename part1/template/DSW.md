## DSW (DAY, STOUT & WARREN)

Creating balanced binary tree search.
(https://leetcode.com/problems/balance-a-binary-search-tree/discuss/541785/C%2B%2BJava-with-picture-DSW-O(n)orO(1))


```c++

// return total number of nodes in the BST
int makeVine(TreeNode *root, int cnt = 0) {
  TreeNode* n = root->right;
  while (n != nullptr) {
    if (n->left != nullptr) { // rotate all n's left child to its parent (parent->right = n)
      TreeNode* tmp = n;
      n = n->left;
      tmp->left = n->right;
      n->right = tmp;
      root->right = n;
    } // every time only rotate one node to its parent
    else {      
        ++cnt;
        root = n;
        n = n->right;
    }
  }
  return cnt;
}
```

Undone yet...

```c++

```