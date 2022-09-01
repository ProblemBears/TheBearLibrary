# Leetcodes

## Linked List
### Palindrome Linked List
- Problem
    - Given the *head* of a singly linked list, return `true` if it is a palindrome or `false` otherwise.
- My solution
    - JavaScript
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

