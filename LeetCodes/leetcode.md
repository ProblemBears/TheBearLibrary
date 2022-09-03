# Leetcodes

## Table Of Contents
- Linked Lists
    - [234. Palindrome Linked List](#234-palindrome-linked-list)
- Trees
    - [144. Binary Tree Preorder Traversal](#144-binary-tree-preorder-traversal)
    - [102. Binary Tree Level Order Traversal](#102-binary-tree-level-order-traversal)
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