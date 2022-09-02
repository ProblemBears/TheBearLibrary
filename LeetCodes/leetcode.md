# Leetcodes

## Table Of Contents
- Linked Lists
    - [234. Palindrome Linked List](#234-palindrome-linked-list)
- Trees
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

## Linked List
### 234. Palindrome Linked List
[Home](#table-of-contents)
- Problem
    - Given the *head* of a singly linked list, return `true` if it is a palindrome or `false` otherwise.
- My solutions
    - **JavaScript** - Recursive Approach
        - Explanation:
            1. Copy the head to another variable - this will be our Front Pointer
            2. Recursively, get the head to the end of the list - this is our Back Pointer
            3. Each stack frame decrements the back pointer, and we have to compare the front and back pointer values to decide whether to return false completely or increment the front pointer.
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
- Problem
    - Given the root of a binary tree, return the preorder traversal of its nodes' values.
- My Solutions
    - Iterative Approach
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