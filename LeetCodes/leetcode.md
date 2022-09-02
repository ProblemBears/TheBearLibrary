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
    - **C++**, **Python** also in - [Submissions](https://leetcode.com/problems/palindrome-linked-list/submissions/)
- Other Approaches:
    1. Copy into Array List and us the Two Pointer Technique
    2. Reverse Second Half In-Place
