# Leetcodes

## Table Of Contents
- Linked Lists
    - [234. Palindrome Linked List](#234-palindrome-linked-list)
- Trees
    - Binary Tree
        - [144. Binary Tree Preorder Traversal](#144-binary-tree-preorder-traversal)
        - [102. Binary Tree Level Order Traversal](#102-binary-tree-level-order-traversal) 
    - Binary Search Tree
        - [98. Validate Binary Search](#98-validate-binary-search-tree)
        - [285. Inorder Successor in BST](#285-inorder-successor-in-bst)
        - [701. Insert into a Binary Search Tree](#701-insert-into-a-binary-search-tree)
        - [450. Delete Node in a BST](#450-delete-node-in-a-bst)
    - N-Ary
        - [589. N-ary Tree Preorder Traversal](#589-n-ary-tree-preorder-traversal)
        - [590. N-ary Tree Postorder Traversal](#590-n-ary-tree-postorder-traversal)
        - [429. N-ary Tree Level Order Traversal](#429-n-ary-tree-level-order-traversal)
- Tries
    - [208. Implement Trie (Prefix Tree)](#208-implement-trie-prefix-tree)
- Graphs
    - Disjoint Set
        - [547. Number of Provinces](#547-number-of-provinces)
    - Minimum Spanning Trees (MSTs)
        - Kruskal's Algorithm
            - [1584. Min Cost to Connect All Points](#1584-min-cost-to-connect-all-points)
        - Primm's Algorithm
            - [1584. Min Cost to Connect All Points (#2)](#1584-min-cost-to-connect-all-points)

- Stacks
- Queues
- Heaps
- Vectors
- Hash Tables
- BFS
    - [1971. Find if Path Exists in Graph (#2)](#1971-find-if-path-exists-in-graph)
    - [797. All Paths From Source to Target (#2)](#797-all-paths-from-source-to-target)
- DFS
    - [1971. Find if Path Exists in Graph](#1971-find-if-path-exists-in-graph)
    - [797. All Paths From Source to Target](#797-all-paths-from-source-to-target)
    - [133. Clone Graph](#133-clone-graph)
    - [1059. All Paths from Source Lead to Destination](#1059-all-paths-from-source-lead-to-destination)
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
    - [701. Insert into a Binary Search Tree](#701-insert-into-a-binary-search-tree)
- 9/6/2022
    - [450. Delete Node in a BST](#450-delete-node-in-a-bst)
    - [589. N-ary Tree Preorder Traversal](#589-n-ary-tree-preorder-traversal)
- 9/7/2022
    - [590. N-ary Tree Postorder Traversal](#590-n-ary-tree-postorder-traversal)
    - [429. N-ary Tree Level Order Traversal](#429-n-ary-tree-level-order-traversal)
- 9/8/2022
    - [208. Implement Trie (Prefix Tree)](#208-implement-trie-prefix-tree)
    - [547. Number of Provinces](#547-number-of-provinces)
- 9/9/2022
    - [1971. Find if Path Exists in Graph](#1971-find-if-path-exists-in-graph)
    - [797. All Paths From Source to Target](#797-all-paths-from-source-to-target)
- 9/10/2022
    - [133. Clone Graph](#133-clone-graph)
- 9/11/2022
    - [1059. All Paths from Source Lead to Destination](#1059-all-paths-from-source-lead-to-destination)
- 9/12/2022
    - [1971. Find if Path Exists in Graph (#2)](#1971-find-if-path-exists-in-graph)
    - [797. All Paths From Source to Target (#2)](#797-all-paths-from-source-to-target)
- 9/13/2022
    - [1584. Min Cost to Connect All Points]()
    - [1584. Min Cost to Connect All Points (#2)]()
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
- [Problem](https://leetcode.com/problems/insert-into-a-binary-search-tree/)
    - You are given the root node of a binary search tree (BST) and a value to insert into the tree. *Return the root node of the BST after the insertion*. It is **guaranteed** that the new value does not exist in the original BST.

        Notice that there may exist multiple valid ways for the insertion, as long as the tree remains a BST after insertion. You can return any of them.

- My Solutions
    - Approach 1 - Recursion (**Python**)
        - Explanation:
            1. **Base Case:**   
            &nbsp;Once we reach a null node, we return a pointer of a newly allocated Node(val)
            2. **Recursive Step:**  
            &nbsp;If the target is greater than the current value
                1. We traverse to the right node recursively
                2. Otherwise, we traverse left
                3. For 1 and 2. We make sure to connec the current node to the recursively returned Node
            3. **Return:**  
            &nbsp;Lastly, reutnring the root ensures we keep the structure of our tree
        ```py
        class Solution:
        def insertIntoBST(self, root: Optional[TreeNode], val: int) -> Optional[TreeNode]:
            if not root:
                return TreeNode(val)
            
            if root.val < val:
                root.right = self.insertIntoBST(root.right, val)
            else:
                root.left = self.insertIntoBST(root.left, val)
                
            return root
        ```
    - [Submissions](https://leetcode.com/problems/insert-into-a-binary-search-tree/submissions/) - Python
    - Other Approaches
        - Iteration &check;

### 450. Delete Node in a BST
- [Problem](https://leetcode.com/problems/delete-node-in-a-bst/)
    - Given a root node reference of a BST and a key, delete the node with the given key in the BST. Return the root node reference (possibly updated) of the BST.
    - Basically, the deletion can be divided into two stages:
        1. Search for a node to remove.
        2. If the node is found, delete the node.

- My Solutions
    - Approach 1 - Recursion (**Java**)
        - Explanation:
            1. Do normal recursive BST traversal until you reach the node to be deleted.
            2. If the node has no children. Then simply return **null**
            3. Otherwise, if it has a right child. Get the inorder successor's value and copy it to our current node to be "deleted", it's actually overwritten not deleted. Therefore, we still have that duplicate value we copied to delete, so we use the same delete function except we pass the "override value" for our new target and the right child as our new "root". It should delete, since the successor definetely a leaf.
            4. Otherwise, we do the same as (3) except for the predecessor
            5. Return the root
        ```py
        class Solution {
        /*
        One step right and then always left
        */
        public int successor(TreeNode root) {
            root = root.right;
            while (root.left != null) root = root.left;
            return root.val;
        }

        /*
        One step left and then always right
        */
        public int predecessor(TreeNode root) {
            root = root.left;
            while (root.right != null) root = root.right;
            return root.val;
        }

        public TreeNode deleteNode(TreeNode root, int key) {
            if (root == null) return null;

            // delete from the right subtree
            if (key > root.val) root.right = deleteNode(root.right, key);
            // delete from the left subtree
            else if (key < root.val) root.left = deleteNode(root.left, key);
            // delete the current node
            else {
            // the node is a leaf
            if (root.left == null && root.right == null) root = null;
            // the node is not a leaf and has a right child
            else if (root.right != null) {
                root.val = successor(root);
                root.right = deleteNode(root.right, root.val);
            }
            // the node is not a leaf, has no right child, and has a left child    
            else {
                root.val = predecessor(root);
                root.left = deleteNode(root.left, root.val);
            }
            }
            return root;
        }
        }
        ```
    - [Submissions](https://leetcode.com/problems/delete-node-in-a-bst/submissions/)

### 589. N-ary Tree Preorder Traversal
- [Problem](https://leetcode.com/problems/n-ary-tree-preorder-traversal/)
    - Given the root of an n-ary tree, return the preorder traversal of its nodes' values.

        Nary-Tree input serialization is represented in their level order traversal. Each group of children is separated by the null value (See examples)

- My Solutions
    - Approach 1 - Iteration (**Python**)
        - Explanation:
            1. Instantiate a result array and stack
            2. Put the root in the stack
            3. While the stack isn't empty
                1. Pop the current node from the stack
                2. Place it in the result
                3. Put all chidren of the current node into the stack in reverse order
            4. Return the result
        ```py
        class Solution:
        def preorder(self, root: 'Node') -> List[int]:
            if root is None:
                return []
            
            stack, res = [root, ], []
            while stack:
                root = stack.pop()
                res.append(root.val)
                stack.extend(root.children[::-1])
            return res
        ```
    - [Submissions](https://leetcode.com/problems/n-ary-tree-preorder-traversal/submissions/)

### 590. N-ary Tree Postorder Traversal
- [Problem](https://leetcode.com/problems/n-ary-tree-postorder-traversal/)
    - Given the root of an n-ary tree, return the postorder traversal of its nodes' values.

        Nary-Tree input serialization is represented in their level order traversal. Each group of children is separated by the null value

- My Solutions
    - Approach 1 - Recursion (**Python**)

        ```py
        class Solution:
            def postorder(self, root: 'Node') -> List[int]:
                return self.helper(root, [])
            
            def helper(self, root, res):
                if root is None:
                    return res
                
                for c in root.children:
                    res = self.helper(c, res)
                res.append(root.val)
                return res
        ```
    - [Submissions](https://leetcode.com/problems/n-ary-tree-postorder-traversal/submissions/)

### 429. N-ary Tree Level Order Traversal
- [Problem](https://leetcode.com/problems/n-ary-tree-level-order-traversal/)
    - Given an n-ary tree, return the level order traversal of its nodes' values.

        Nary-Tree input serialization is represented in their level order traversal, each group of children is separated by the null value (See examples).

- My Solutions
    - Approach 1 - BFS using a Queue (**Javascript**)
        - Explanation:
            1. Create a queue, return array, and push the root to the queue
            2. While the queue isn't empty
                1. Create a level return array, and get the current queue size (this is the size of the current level)
                2. For every node in the current level (use the level size to know the end of a level)
                    1. Pop the node from the queue
                    2. Store its value in the level return array
                    3. Store its children in the queue
                3. Store the level return array in the main return array
            5. Return the main return array

        ```js
        var levelOrder = function(root) {
            if(!root) return [];
            
            let res = [];
            let q = [];
            
            q.push(root);
            while(q.length){
                let level = [];
                let levelSize = q.length;
                
                for(let i = 0; i < levelSize; i++){
                    let cur = q.shift();
                    q = q.concat(cur.children);
                    level.push(cur.val);
                }
                res.push(level);
            }
            return res;
        };
        ```
    - [Submissions](https://leetcode.com/problems/n-ary-tree-level-order-traversal/submissions/) - C++, JavaScript, 
    - Other Approaches:
        - Simplified BFS &cross;
        - Recursion &cross;

## Tries
### 208. Implement Trie (Prefix Tree)
- [Problem](https://leetcode.com/problems/implement-trie-prefix-tree/)
    - A trie (pronounced as "try") or prefix tree is a tree data structure used to efficiently store and retrieve keys in a dataset of strings. There are various applications of this data structure, such as autocomplete and spellchecker.

        Implement the Trie class:

        - `Trie()` Initializes the trie object.
        - `void insert(String word)` Inserts the string word into the trie.
        - `boolean search(String word)` Returns true if the string word is in the trie (i.e., was - inserted before), and false otherwise.
        - `boolean startsWith(String prefix)` Returns true if there is a previously inserted string word that has the prefix prefix, and false otherwise.


- My Solutions
    - Approach 1 - Array initialized with 26 nulls (**Python**)
        - Explanation:
            - Traversal is the key to most of these functions. It starts with checking whether there's a node of the current character that we can traverse to, and if there isn't for:
                - `insert` we add a node and continue the traversal. we mark it as word at the last traversed node.
                - `search` it fails. Otherwise, at the end of a traversal it must check if the ending is a word
                - `startsWith` also fails. But, it returns true if it completes it's traversal.
        ```py
        class TrieNode:
            def __init__(self):
                self.children = [None]*26
                self.isWord = False
                
            def hasChild(self, c):
                return self.children[ord(c)-ord('a')] != None
            
            def putChild(self, c):
                self.children[ord(c)-ord('a')] = TrieNode()
                
            def getChild(self, c):
                return self.children[ord(c)-ord('a')]
            
            def markWord(self):
                self.isWord = True
                
        class Trie:

            def __init__(self):
                self.root = TrieNode()

            def insert(self, word: str) -> None:
                node = self.root
                for c in word:
                    if not node.hasChild(c):
                        node.putChild(c)
                    node = node.getChild(c)
                node.markWord()

            def search(self, word: str) -> bool:
                node = self.root
                for c in word:
                    if not node.hasChild(c):
                        return False
                    node = node.getChild(c)
                return node.isWord

            def startsWith(self, prefix: str) -> bool:
                node = self.root
                for c in prefix:
                    if not node.hasChild(c):
                        return False
                    node = node.getChild(c)
                return True
        ```
    - [Submissions](https://leetcode.com/problems/implement-trie-prefix-tree/submissions/) - C++, JavaScript, Python

## Graphs
### 547. Number of Provinces
- [Problem](https://leetcode.com/problems/number-of-provinces/https://leetcode.com/problems/number-of-provinces/)
    - There are n cities. Some of them are connected, while some are not. If city a is connected directly with city b, and city b is connected directly with city c, then city a is connected indirectly with city c.

        A province is a group of directly or indirectly connected cities and no other cities outside of the group.

        You are given an n x n matrix isConnected where isConnected[i][j] = 1 if the ith city and the jth city are directly connected, and isConnected[i][j] = 0 otherwise.

        Return the total number of provinces.

- My Solutions
    - Approach 3 - Union-Find (**Python**)
        - Explanation:
            1. UnionFind's `root[]` is a representation of connected provininces. Initially, every node's root is itself which represents all of them being seperated and not yet connected
                - `unify` forms the connections in this UnionFind
                - `find` is used by unify, to get the root of a node, and simultaneously update the roots of nodes to the root
                - `islands` returns the answer to how many provinces there are by making use of the fact that nodes with the root as their index is the head of an island, otherwise it's part of an island
            2. Therefore, we create the UnionFind representation from the adjacency matrix and get the result from it.
        ```c++
        class UnionFind {
        private:
            vector<int> root, rank;
        public:
            UnionFind(int size)
                : root(size), rank(size)
            {
                for(int i = 0; i < size; i++)
                {
                    root[i] = i;
                    rank[i] = 1;
                }
            }
            
            void unify(int x, int y)
            {
                int rootX = find(x);
                int rootY = find(y);
                if(rootX != rootY)
                {
                    if(rank[rootX] > rank[rootY])
                        root[rootY] = rootX;
                    else if(rank[rootX] < rank[rootY])
                        root[rootX] = rootY;
                    else
                    {
                        root[rootY] = rootX;
                        rank[rootX] += 1;
                    }
                }
            }
            
            int find(int x)
            {
                if( x == root[x] ) return x;
                return root[x] = find(root[x]);
            }
            
            int islands(){
                int res = 0;
                for(int i = 0; i < root.size(); i++)
                {
                    if(root[i] == i)
                        res++;
                }
                return res;
            }
        };

        class Solution {
        public:
            int findCircleNum(vector<vector<int>>& isConnected) {
                int n = isConnected.size();
                UnionFind uf = UnionFind(n);
                for(int i = 0; i < n; i++)
                {
                    for(int j = 0; j < n; j++)
                    {
                        if(isConnected[i][j])
                            uf.unify(i, j);
                    }
                }
                return uf.islands();
            }
        };
        ```
    - [Submissions](https://leetcode.com/problems/number-of-provinces/submissions/) - C++, Python
    - Other Approaches
        1. DFS &cross;
        2. BFS &cross;

### 1584. Min Cost to Connect All Points
- [Problem](https://leetcode.com/problems/min-cost-to-connect-all-points/)
    - You are given an array points representing integer coordinates of some points on a 2D-plane, where points[i] = [xi, yi].

        The cost of connecting two points [xi, yi] and [xj, yj] is the manhattan distance between them: |xi - xj| + |yi - yj|, where |val| denotes the absolute value of val.

        Return the minimum cost to make all points connected. All points are connected if there is exactly one simple path between any two points.

- My Solutions
    - Approach #1 - Kruskal's Algorithm (**C++**)
        - Explanation:
            1. We'll need the following data structures
                - **Edge** - to succinctly define the relationship between points and store their costs
                - **UnionFind** - This helps detect if points are already connected, which we can use to avoid adding edges that would create cycles
            2. The algorithm
                1. If there's no points, the min cost is 0
                2. For every point, make an edge with every other point, calculate the cost, and put it in the heap. **This simulates the ascending ordering of all edges in Kruskal's algorithm**
                3. While there are still edges int the heap **AND** the MST still doesn't have n-1 edges
                    - Pop from the heap. This is the edge we're currently deciding whether to put into the MST, and since it was popped from the min-heap it's the next best **greedy** choice.
                    - If both vertices of that edge are already connected (UnionFind graph), we should skip to the next iteration since this current edge being added would cause a cycle
                    - Make sure you update the number of edges added to break the while loop and update the value we'll return
        ```c++
        class Edge {
        public:
            int point1;
            int point2;
            int cost;
            Edge(int point1, int point2, int cost)
                : point1(point1), point2(point2), cost(cost) {}
        };

        bool operator<(const Edge& edge1, const Edge& edge2) 
        {
            return edge1.cost > edge2.cost;
        }

        class UnionFind {
        private:
            vector<int> root;
            vector<int> rank;
        public:
            UnionFind(int n)
            : root(n), rank(n)
            {
                for(int i = 0; i < n; i++)
                {
                    root[i] = i;
                    rank[i] = 1;
                }
            }
            
            void unify(int x, int y)
            {
                int rootX = find(x);
                int rootY = find(y);
                
                if(rootX != rootY)
                {
                    if( rank[rootX] > rank[rootY] )
                        root[rootY] = rootX;
                    else if( rank[rootX] < rank[rootY] )
                        root[rootX] = rootY;
                    else
                    {
                        rank[rootX] += 1;
                        root[rootY] = rootX;
                    }
                }
            }
            
            int find(int x)
            {
                if(root[x] == x) return x;
                return root[x] = find(root[x]);
            }
            
            bool connected(int x, int y)
            {
                return find(x) == find(y);
            }
        };

        class Solution {
        public:
            int minCostConnectPoints(vector<vector<int>>& points) {
                int n = points.size();
                if (n == 0) return 0;
                
                priority_queue<Edge> pq;
                UnionFind uf(n);
                
                /*
                Form Edges - that contain x, y and the calculated manhattan distance from the coordinates of the points
                Put the edges in a heap (priority queue)
                */
                for(int i = 0; i < n; i++)
                {
                    vector<int>& coord1 = points[i];
                    for(int j = i+1; j < n; j++)
                    {
                        vector<int>& coord2 = points[j];
                        int cost = abs(coord1[0] - coord2[0]) + abs(coord1[1] - coord2[1]);
                        Edge edge(i, j, cost);
                        pq.push(edge);
                    }
                }
                
                int res = 0;
                int count = n-1;
                while( !pq.empty() && count > 0)
                {
                    Edge edge = pq.top();
                    pq.pop();
                    
                    if( !uf.connected(edge.point1, edge.point2) )
                    {
                        uf.unify(edge.point1, edge.point2);
                        res += edge.cost;
                        count--;
                    }
                }
                return res;
            }
        };
        ```
    - Approach #2 - Primm's Algorithm (**C++**)
        - Explanation:
            1. 
        ```c++
        
        ```
    - [Submissions](https://leetcode.com/problems/min-cost-to-connect-all-points/submissions/) - C++ &check;

## DFS
### 1971. Find if Path Exists in Graph
- [Problem](https://leetcode.com/problems/find-if-path-exists-in-graph/)
    - There is a bi-directional graph with n vertices, where each vertex is labeled from 0 to n - 1 (inclusive). The edges in the graph are represented as a 2D integer array edges, where each edges[i] = [ui, vi] denotes a bi-directional edge between vertex ui and vertex vi. Every vertex pair is connected by at most one edge, and no vertex has an edge to itself.

        You want to determine if there is a valid path that exists from vertex source to vertex destination.

        Given edges and the integers n, source, and destination, return true if there is a valid path from source to destination, or false otherwise.

- My Solutions
    - **Approach #1 -** Iterative DFS (**C++**)
        - Explanation:
            1. Like BFS except we use a stack to simulate stack frames
        ```C++
        class Solution {
        public:
            bool validPath(int n, vector<vector<int>>& edges, int source, int destination) {
                vector<vector<int>> graph(n);
                for(vector<int> edge : edges)
                {
                    graph[edge[0]].push_back(edge[1]);
                    graph[edge[1]].push_back(edge[0]);
                }
                
                stack<int> s;
                s.push(source);
                vector<bool> visited(n);
                
                while(!s.empty())
                {
                    int cur = s.top();
                    s.pop();
                    
                    if( cur == destination ) return true;
                    
                    if (visited[cur]) continue;
                    visited[cur] = true;
                    
                    for(int neighbor : graph[cur])
                        s.push(neighbor);
                }
                return false;
            }
        };
        ```
    - **Approach #2 -** Iterative BFS (**Python**)
        - Explanation:
            1. Form a **graph representation** out of the given **edges**. Here we make an **adjacency list** to represent our graph
            2. Then we do our DFS to see if a path from **source to destination** exists :
                1. Put the source node in the queue and seen array
                2. While the queue isn't empty
                    1. Pop the front of the queue. This is our **current** node
                    2. If this is the destination return **True** because a path does exist!
                    3. Otherwise, add all of the neighbors of the current node to the queue and seen array, to continue the DFS
            3. If the DFS ends without ever reaching the destination, return false
        ```py
        class Solution:
        def validPath(self, n: int, edges: List[List[int]], source: int, destination: int) -> bool:
            adjacency_list = [[] for _ in range(n)]
            for x, y in edges:
                adjacency_list[x].append(y)
                adjacency_list[y].append(x)
                
            q = collections.deque([source])
            seen = set([source])
            
            while q:
                node = q.popleft()
                
                if node == destination:
                    return True
                
                for neighbor in adjacency_list[node]:
                    if neighbor not in seen:
                        seen.add(neighbor)
                        q.append(neighbor)
                        
            return False
        ```
    - [Submissions](https://leetcode.com/problems/find-if-path-exists-in-graph/submissions/) - C++ &cross;, Python &check;

### 797. All Paths From Source to Target
- [Problem](https://leetcode.com/problems/all-paths-from-source-to-target/)
    - Given a directed acyclic graph (DAG) of n nodes labeled from 0 to n - 1, find all possible paths from node 0 to node n - 1 and return them in any order.

        The graph is given as follows: graph[i] is a list of all nodes you can visit from node i (i.e., there is a directed edge from node i to node graph[i][j]).

- My Solutions
    - Approach #1 - Backtracking DFS (**JavaScript**)
        - Explanation:
            1. Create the target (from the graph's size; the # of nodes it has) and a res array where you will store paths to the target
            2. We create a backtrack function.
                - **Base Case :** If we reached the target. Add the current path to the result
                - **Recursive Step :** For every neighbor of the current node, add it to the current path and call the recursive function with this path. When the function returns be sure to remove the current neighbor from the path, so that we can construct another path with the next neighbor.
            3. Call the recursive function with the initial node 0 and path [0]
        ```js
        var allPathsSourceTarget = function(graph) {
            let target = graph.length - 1;
            let res = [];
            
            function backtrack(node, path) {
                if( node == target) {
                    res.push(Array.from(path));
                    return;
                }
                
                for(let neighbor of graph[node]) {
                    path.push(neighbor);
                    backtrack(neighbor, path);
                    path.pop();
                }
            }
            backtrack(0, [0]);
            return res;
        };
        ```
    - Approach #2 - Iterative BFS
        - Explanation:
            1. Use a queue and store the paths in it. We use the last node of those paths to traverse
        ```C++
        class Solution {
        public:
            vector<vector<int>> allPathsSourceTarget(vector<vector<int>>& graph) {
                vector<vector<int>> paths;
                if(graph.size() == 0) return paths;
                
                vector<int> path;
                path.push_back(0);
                queue<vector<int>> q;
                q.push(path);
                
                while(!q.empty())
                {
                    vector<int> currentPath = q.front();
                    q.pop();
                    int node = currentPath.back();
                    
                    for(int neighbor : graph[node])
                    {
                        vector<int> tmpPath(currentPath.begin(), currentPath.end());
                        tmpPath.push_back(neighbor);
                        if(neighbor == graph.size()-1)
                            paths.push_back(tmpPath);
                        else
                            q.push(tmpPath);
                    }
                }
                return paths;
            }
        };
        ```
    - [Submissions](https://leetcode.com/problems/all-paths-from-source-to-target/submissions/) - C++ &check;, Python &check; JavaScript &check;
    - Other Approaches :
        1. Top-Down Dynamic Programming

### 133. Clone Graph
- [Problem](https://leetcode.com/problems/clone-graph/)
    - Given a reference of a node in a connected undirected graph.

        Return a deep copy (clone) of the graph.

        Each node in the graph contains a value (int) and a list (List[Node]) of its neighbors.

- My Solutions
    - Approach - DFS w/ HashMap (**C++**)
        - Explanation:
            1. We traverse using the nodes that we have to copy, but since we'll be traversing recursively, we need to keep a reference to the clone node. This is why we make a map with key as the original node and the value as the cloned node.
            2. Recursion:
                - **Base Case** - if the node was visited before, then it has a clone node already (we shouln't be cloning multiple times) so we simply return the clone node
                - **Intermediate Step** - Create the clone node with the value of the current node. Put it into the HashMap
                - **Recursive Step** - We still need to add the children of the clone node. Therefore, we push to the children of our clone node, a call to the recursive function (since it) will yield a clone node.
        ```c++
        class Solution {
            unordered_map<Node*, Node*> visited;
        public:
            Node* cloneGraph(Node* node) {
                if(!node) return node;
                if( visited.find(node) != visited.end() )
                    return visited[node];
                
                Node* new_node = new Node(node->val);
                visited[node] = new_node;
                for(Node* neighbor : node->neighbors)
                {
                    new_node->neighbors.push_back(cloneGraph(neighbor));
                }
                return new_node;
            }
        };
        ```
    - [Submissions](https://leetcode.com/problems/clone-graph/submissions/) - C++ &check; Python &check; JavaScript &check;
    - Other Approaches :
        1. BFS

### 1059. All Paths from Source Lead to Destination
- [Problem](https://leetcode.com/problems/all-paths-from-source-lead-to-destination/)
    -   Given the edges of a directed graph where edges[i] = [ai, bi] indicates there is an edge between nodes ai and bi, and two nodes source and destination of this graph, determine whether or not all paths starting from source eventually, end at destination, that is:

        At least one path exists from the source node to the destination node
        If a path exists from the source node to a node with no outgoing edges, then that node is equal to destination.  

        The number of possible paths from source to destination is a finite number.

        

- My Solutions
    - Approach -  DFS (**C++**)
        - Explanation:
            - We hace to check for two things:
                1. If any leaf node is not the destination. **False**.
                2. If the *finite* directed graph is not finite then it's **cyclic** so return **False**
                    - This can be done by using the white, gray, black cycle finding algorithm, where white signifies unvisited, gray signifies currently being processed, and black is completely processed
        ```c++
        #include <vector>
        using namespace std;

        class Solution {
            
            vector<vector<int>> graph;
            static enum class StateOfNode { WHITE, GRAY, BLACK };

        public:
            bool leadsToDestination(int numberOfNodes, vector<vector<int>>& edges, int source, int destination) {
                
                vector<StateOfNode> stateOfNodes(numberOfNodes, StateOfNode::WHITE);
                initializeGraph(edges, numberOfNodes);

                return depthFirstSearch(stateOfNodes, source, destination);
            }

        private:
            bool depthFirstSearch(vector<StateOfNode>& stateOfNodes, int node, int destination) {

                if (graph[node].empty()) {
                    return node == destination;
                }
                if (stateOfNodes[node] == StateOfNode::GRAY) {
                    return false;
                }

                stateOfNodes[node] = StateOfNode::GRAY;
                for (const auto& n : graph[node]) {
                    if (!depthFirstSearch(stateOfNodes, n, destination)) {
                        return false;
                    }
                }

                stateOfNodes[node] = StateOfNode::BLACK;
                return true;
            }

            void initializeGraph(const vector<vector<int>>& edges, int numberOfNodes) {
                graph.assign(numberOfNodes, vector<int>());
                for (const auto& edge : edges) {
                    graph[edge[0]].push_back(edge[1]);
                }
            }
        ```
    - [Submissions](https://leetcode.com/problems/all-paths-from-source-lead-to-destination/submissions/) - C++ &check;
    - Other Approaches :