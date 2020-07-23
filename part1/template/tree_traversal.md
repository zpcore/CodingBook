## Tree Traversal
---

### Idea behind using a stack
This depends on when do we add the node to the result. There are two case:  
1) Add the treenode to the result whenever a node is **pushed** into the stack.  
2) ..............................................    **poped** out of the stack.  
First case follows the traverse sequence;  
Second case follows likely a reversed traversed sequence.
t
Key point of using the stack:  
1) push: when discovering a new node, and we need to traverse it later;  
2) pop: when we don't need the info of this node any more.  

Key point of tracing:
1) All traverse follow the order from ancestor to child node, no exception! (that's why we always dive into the deepest left first.)  
2) Difference is that when we add the node to result. right after push or pop?  

Thus I provide the following template for inorder and preorder:  
```c++
std::vector<TreeNode*> goldenTraversal(TreeNode* root) {
    // key: keep diving to the left
    std::vector<TreeNode*> res;
    std::stack<TreeNode*> stack; // stores the node that hasn't traced the right branch 
    TreeNode* curNode = root;
    while(!stack.empty() || curNode) {
        while (curNode) {
            stack.push(curNode);
            //res.push_back(curNode); // preorder use this one
            curNode = curNode->left;
        }
        curNode = stack.top();
        stack.pop();
        //res.push_back(curNode); // inorder use this one
        curNode = curNode->right;
    }
    return res;
}
```
For postorder, key point is to pop the node only when right child has been traversed.
---

```c++
std::vector<TreeNode*> inorderTraversal(TreeNode* root) {
    // key: keep diving to the left
    std::vector<TreeNode*> res;
    std::stack<TreeNode*> stack; // stores the node that hasn't traced the right branch 
    TreeNode* curNode = root;
    while(!stack.empty() || curNode) {
        while (curNode) {
            stack.push(curNode); 
            curNode = curNode->left;
        }
        curNode = stack.top();
        stack.pop();
        res.push_back(curNode); // inorder use this one
        curNode = curNode->right;
    }
    return res;
}

// not suggested
std::vector<TreeNode*> preorderTraversal(TreeNode* root) {
    // key: layer by layer
    std::vector<TreeNode*> res;
    std::stack<TreeNode*> stack; // stores the node that hasn't traced the right branch 
    stack.push(root);    
    while(!stack.empty()) {
        TreeNode* curNode = stack.top();
        stack.pop();
        res.push_back(curNode);
        if (curNode->right)
            stack.push(curNode->right);
        if (curNode->left)
            stack.push(curNode->left);
        
    }
    return res;
}
```

```c++
std::vector<TreeNode*> inorderTraversal(TreeNode* root) {
    std::vector<TreeNode*> res;
    std::stack<TreeNode*> stack;
    TreeNode* curNode = root;
    while (!stack.empty() || curNode!=NULL) {
        if (curNode) {
            stack.push(curNode);
            // res.push_back(curNode); // preorder use this one
            curNode = curNode->left;
        } else {
            TreeNode* tmp = stack.top();
            stack.pop();
            res.push_back(tmp); // inorder use this one
            curNode = curNode->right;
        }
    }
}

std::vector<TreeNode*> preorderTraversal(TreeNode* root) {
    std::vector<TreeNode*> res;
    std::stack<TreeNode*> stack;
    TreeNode* curNode = root;
    while(!stack.empty() || curNode) {
        if (curNode!=NULL) {
            stack.push(curNode);
            res.push_back(curNode);
            curNode = curNode->left;
        } else {
            TreeNode* tmp = stack.top();
            stack.pop();
            curNode = tmp->right;
        }
    }
    return res;
}
```

```c++
std::vector<TreeNode*> postorderTraversal(TreeNode* root) {
    // Key:  when pop a node from stack, ensure its children have already been explored .
    std::vector<TreeNode*> res;
    std::stack<TreeNode*> stack;
    TreeNode* pre = NULL;
    TreeNode* cur = root;
    while(cur || !stack.empty()) {
        while(cur) {
            stack.push(cur);
            cur = cur->left;
        }
        cur = stack.top();
        if (cur->right && pre != cur->right) {
            cur = cur->right;
        } else {
            stack.pop();
            res.push_back(cur);
            pre = cur;
            cur = NULL;
        }
    }
    return res;
}
```