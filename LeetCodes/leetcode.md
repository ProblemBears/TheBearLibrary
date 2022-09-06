# Leetcodes

## Table Of Contents
- Linked Lists
    - [234. Palindrome Linked List](#234-palindrome-linked-list)
- Trees
    - Traversal
        - [144. Binary Tree Preorder Traversal](#144-binary-tree-preorder-traversal)
        - [102. Binary Tree Level Order Traversal](#102-binary-tree-level-order-traversal) 
    - Binary Search Tree
        - [98. Validate Binary Search](#98-validate-binary-search-tree)
        - [285. Inorder Successor in BST](#285-inorder-successor-in-bst)

- Tries
- Graphs
- Stacks
- Queues
- Heaps
- Vectors
- Hash Tables
- BFS
- DFS
- Binary Search
- Merge Sort
- Quick Sort
- Bit Manipulation
- Recursion
- Dynamic Programming

## Days
- 9/1/2022
    - [234. Palindrome Linked List](#234-palindrome-linked-list)
- 9/2/2022
    - [144. Binary Tree Preorder Traversal](#144-binary-tree-preorder-traversal)
    - [102. Binary Tree Level Order Traversal](#102-binary-tree-level-order-traversal)
- 9/3/2022
    - [98. Validate Binary Search](#98-validate-binary-search-tree)
- 9/4/2022
    - [285. Inorder Successor in BST](#285-inorder-successor-in-bst)
- 9/5/2022
    - [285. Inorder Successor in BST (#2)](#285-inorder-successor-in-bst)
    - 

## Linked List
### 234. Palindrome Linked List
[Home](#table-of-contents)
- [Problem](https://leetcode.com/problems/palindrome-linked-list/)
    - Given the *head* of a singly linked list, return `true` if it is a palindrome or `false` otherwise.
- My solutions
    - Recursive Approach (**JavaScript**)
        - Explanation:
            1. Copy the head to another variable - this will be our Front Pointer
            2. Recursively, get the head to the end of the list - this is our Back Pointer
            3. Each returning stack frame decrements the back pointer, and we have to compare the front and back pointer values to decide whether to return false completely or increment the front pointer.
        ```js
        var isPalindrome = function(head) {
            let frontHead = head;
            
            function recursive_helper(currHead = head)
            {
                if(currHead !== null)
                {
                    if(!recursive_helper(currHead.next)) return false;
                    if(currHead.val != frontHead.val) return false;
                    frontHead = frontHead.next;
                }   
                return true;
            }
            return recursive_helper();
        };
        ```
    - [Submissions](https://leetcode.com/problems/palindrome-linked-list/submissions/)
- Other Approaches:
    - Copy into Array List and us the Two Pointer Technique &check;
    - Reverse Second Half In-Place &cross;

## Trees
### 144. Binary Tree Preorder traversal
[Home](#table-of-contents)
- [Problem](https://leetcode.com/problems/binary-tree-preorder-traversal/)
    - Given the root of a binary tree, return the preorder traversal of its nodes' values.
- My Solutions
    - Iterative Approach (**Python**)
        - Explanation:
            1. We need a **stack**. To simulate stack frames like in the simple Recursion Approach
            2. We insert root into the stack
            3. While the stack isn't empty 
                1. We pop the top and insert it's value into the **result array**
                2. If it has a right child push it to the stack
                3. if it had a left child push it to the stack
        ```py
        class Solution:
            def preorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
                stack = []
                res = []
                
                if root is None:
                    return res
                
                stack.append(root)

                while len(stack) != 0:
                    cur = stack.pop()
                    res.append(cur.val)
                    if cur.right is not None:
                        stack.append(cur.right)
                    if cur.left is not None:
                        stack.append(cur.left)
                return res
        ```
        - [Submissions](https://leetcode.com/problems/binary-tree-preorder-traversal/submissions/)
    - Other Approaches
        1. Recursion &check;
        2. Morris traversal &cross;
    - Related Practice
        - [94. Binary Tree Inorder Traversal](https://leetcode.com/problems/binary-tree-inorder-traversal/)
        - [145. Binary Tree Postorder Traversal](https://leetcode.com/problems/binary-tree-postorder-traversal/)

### 102. Binary Tree Level Order Traversal
- [Problem](https://leetcode.com/problems/binary-tree-level-order-traversal/)
    - Given the root of a binary tree, return the level order traversal of its nodes' values. (i.e., from left to right, level by level).
- My Solutions
    - Iterative Breadth First Search Approach (**C++**)
        - Explanation:
            1. Create a result storage and check if **root** is null.
            2. Create a **queue** and push the **root** in
            3. While the queue is not empty
                1. Get the size of the current level (everything that is currently in the queue)
                2. Create a storage for the current level
                3. Iterate the amount of the level size
                    1. Pop the front of the queue and store it in our level storage
                    2. Check if the current node has left and right pointers and store them in that order if they exist
                4. Push back the stored level
            4. Return the result storage
        ```c++
        class Solution {
        public:
            vector<vector<int>> levelOrder(TreeNode* root) {
                vector<vector<int>> res;
                if(!root) return res;
                queue<TreeNode*> q;
                
                q.push(root);
                
                while(!q.empty())
                {
                    int levelSize = q.size();
                    vector<int> level;
                    
                    for(int i = 0; i < levelSize; i++)
                    {
                        TreeNode* node = q.front();
                        q.pop();
                        if (node->left) q.push(node->left);
                        if (node->right) q.push(node->right);
                        level.push_back(node->val);
                    }
                    res.push_back(level);
                }
                return res;
            }
        };
        ```
        - [Submissions](https://leetcode.com/problems/binary-tree-level-order-traversal/submissions/)
    - Other Approaches
        - Recursive &cross;

### 98. Validate Binary Search Tree
- [Problem](https://leetcode.com/problems/validate-binary-search-tree/)
    - Given the root of a binary tree, determine if it is a valid binary search tree (BST).  
  
        A valid BST is defined as follows:
        - The left subtree of a node contains only nodes with keys less than the node's key.
        - The right subtree of a node contains only nodes with keys greater than the node's key.
        - Both the left and right subtrees must also be binary search trees.

- My Solutions
    - Approach 1 - Recursive Traversal With Valid Range (**C++**)
        - Explanation:
            - At first, it may seem that you can trivially just compare the left and right child. But this will not work because it is possible that the node may have something like a great great grandchild that ends up being bigger than the grandparent.
            - Therefore, we have to Top-Down Recursively pack our recursive function with a low and a high range, and if any node is not within that exclusive range then the whole thing fails and returns false.
        ```c++
        class Solution {
        public:
            bool isValidBST(TreeNode* root) 
            {
                return helper(root, nullptr, nullptr);
            }
            
            bool helper(TreeNode* root, TreeNode* lo, TreeNode* hi )
            {
                if(!root) return true;
                
                if( 
                    (lo && !(lo->val < root->val) ) 
                    or
                    (hi && !(root->val <  hi->val) ) 
                )
                    return false;
                
                return helper(root->right, root, hi) and helper(root->left, lo, root);
                
            }
        };
        ```
        - [Submissions](https://leetcode.com/problems/validate-binary-search-tree/submissions/) - C++
    - Other Approaches
        - Iterative Traversal with Valid Range &cross;
        - Recursive Inorder Traversal &cross;
        - Iterative Inorder Traversal &cross;

### 285. Inorder Successor in BST
- [Problem](https://leetcode.com/problems/inorder-successor-in-bst/)
    - Given the root of a binary search tree and a node p in it, return the in-order successor of   that node in the BST. If the given node has no in-order successor in the tree, return null.  
    The successor of a node p is the node with the smallest key greater than p.val.

- My Solutions
    - Approach 1 - Without Using BST Properties (**C++**)
        - Explanation:
            - For any Binary Tree we can find the inorder successor of a node by implementing
            the following two cases in order:
                1. We simply need to find the leftmost node in the subtree rooted at
                p->right **(NOTICE p IS NOT THE ROOT)**  
                Because we need to find the
                successor of p, so if p has a right child then it would be that child's left most
                descendant (if it has no left than it's simply p->right)
                2. Otherwise, we do an inorder traversal
                where we continuously update our previous pointer. When our previous
                equals the p TreeNode*, since it is our "previous", we can assume that
                the recursive stack frame we are currently in is our Successor. Of course,
                we have to make sure not to overwrite Case 1's answer if there is any, 
                because that case is always logically true and takes precedent over this case.
        ```c++
        class Solution {
        public:
            TreeNode* previous = nullptr;
            TreeNode* res = nullptr;
        public:
            TreeNode* inorderSuccessor(TreeNode* root, TreeNode* p) {
                //Case 1
                if(p->right)
                {
                    TreeNode* leftMost = p->right;
                    while(leftMost->left)
                        leftMost = leftMost->left;
                    res = leftMost;
                }
                //Case 2
                else
                    case2Inorder(root, p);
                return res;
            }
            
            void case2Inorder(TreeNode* node, TreeNode* p)
            {
                if(!node) return;
                case2Inorder(node->left, p);
                if(previous == p && !res)
                {
                    res = node;
                    return;
                }
                previous = node;
                case2Inorder(node->right, p);
            }
        };
        ```
    - Approach 2 - Using BST properties
        - Explanation:
            - We iteratively traverse the tree in A BST manner ( therefore if the BST is balanced we can get our result in O(logN) instead of Approach 1's O(N) ).
            - While the traversal doesn't end.  
             We check if **p.val** is **equal to or greater than** the **current node in our traversal's val**
                - **If yes then -** our answer is on the right subtree
                - **Otherwise** -The **current node** is a potential successor and we move to the left subtree for the next iteration.
        ```c++
        class Solution {
        public:
            TreeNode* inorderSuccessor(TreeNode* root, TreeNode* p) {
                TreeNode* successor = nullptr;
                
                while(root)
                {
                    if( p->val >= root->val)
                        root = root->right;
                    else
                    {
                        successor = root;
                        root = root->left;
                    }
                }
                
                return successor;
            }
        };
        ```  
    - [Submissions](https://leetcode.com/problems/inorder-successor-in-bst/submissions/) - C++

### 701. Insert into a Binary Search Tree
- [Problem](https://leetcode.com/problems/inorder-successor-in-bst/)
    - You are given the root node of a binary search tree (BST) and a value to insert into the tree. *Return the root node of the BST after the insertion*. It is **guaranteed** that the new value does not exist in the original BST.

        Notice that there may exist multiple valid ways for the insertion, as long as the tree remains a BST after insertion. You can return any of them.

- My Solutions
    - Approach 1 - Without Using BST Properties (**C++**)
        - Explanation:
            
        ```py
        ```
    - [Submissions](https://leetcode.com/problems/insert-into-a-binary-search-tree/submissions/) - C++
    - Other Approaches
        - Iteration &check;