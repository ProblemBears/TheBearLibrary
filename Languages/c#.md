<h1 align = "center"> C# </h2>

<h2 align="center"> Table of Contents </h2>

- [Basic Syntax](#basics)
    * [Objects](#object-boxing-and-unboxing)
    * [Arrays](#arrays)
    * [Strings](#pointers)
    * [Classes](#classes)
    * [Type Parameters](#type-parameters)
    * [Structs](#structs)
    * [Interface](#interfaces)
    * [Enums](#enums)
    * [Tuples](#tuples)
- [System.Collections](#std)
    * [ArrayList](#arraylist)
    * [Hashtable](#hashtable)
    * [Dictionary](#dictionary)
    * [Queue](#queue)
- [System.Collections.Generic](#std-generic)
    * [HashSet](#hashset)
    * [Stack](#stack)
    * [Queue](#queue)
    * [List](#list)
    * [Dictionary](#dictionary)
    * [Heap](#heap)
- Leetcodes
    1. [Linked List](https://leetcode.com/explore/learn/card/linked-list/)
        * Singly Linked List
            1. [Design Linked List](https://leetcode.com/problems/design-linked-list/description/) (GOOD PRACTICE)
        * Two Pointer Technique
            1. [Linked List Cycle](https://leetcode.com/problems/linked-list-cycle/description/) &check;
            2. [Linked List Cycle 2](https://leetcode.com/problems/linked-list-cycle-ii/description/) &check;
            3. [Intersection of Two Linked Lists](https://leetcode.com/problems/intersection-of-two-linked-lists/description/) &check;
            4. [Remove Nth Node from End of List](https://leetcode.com/problems/remove-nth-node-from-end-of-list/description/) &cross;
        * Classic Problems
            1. [Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/description/) &nbsp;(GOOD PRACTICE)
            2. [Remove Linked List Elements](https://leetcode.com/problems/remove-linked-list-elements/description/)
            3. [Odd Even Linked List](https://leetcode.com/problems/odd-even-linked-list/description/)
            4. [Palindrome Linked List](https://leetcode.com/problems/palindrome-linked-list/description/)
        * Doubly Linked List
        * More Problems
    2. [Binary Search Tree](https://leetcode.com/explore/learn/card/introduction-to-data-structure-binary-search-tree/)
    3. [N-ary Tree](https://leetcode.com/explore/learn/card/n-ary-tree/)
    4. [Tries](https://leetcode.com/explore/learn/card/trie/)
    5. [Graph](https://leetcode.com/explore/learn/card/graph/)
    6. [Queue & Stack](https://leetcode.com/explore/learn/card/queue-stack/)
        * Queue
            1. [Design Circular Queue](https://leetcode.com/problems/design-circular-queue/) &check;
            2. [Moving Average from Data stream](https://leetcode.com/problems/moving-average-from-data-stream/) &check;
        * Queue & BFS
        * Stack
            1. [Min Stack](https://leetcode.com/problems/min-stack/) &check;
            2. [Valid Parentheses](https://leetcode.com/problems/valid-parentheses/)
            3. [Daily Temperatures](https://leetcode.com/problems/daily-temperatures/)
            4. [Evaluate Reverse Polish Notation](https://leetcode.com/problems/evaluate-reverse-polish-notation/)
        * Stack & DFS
            1. [Clone Graph](https://leetcode.com/problems/clone-graph/)
            2. [Binary Tree Inorder Traversal](https://leetcode.com/problems/binary-tree-inorder-traversal/)
    7. [Heaps](https://leetcode.com/explore/learn/card/heap/)
    8. [Array and String](https://leetcode.com/explore/learn/card/array-and-string/)
        * Array
            1. [Find Pivot Index](https://leetcode.com/problems/find-pivot-index/description/)
            2. [Largest Number At Least Twice of Others](https://leetcode.com/problems/largest-number-at-least-twice-of-others/)
            3. [Plus One](https://leetcode.com/problems/plus-one/)
        * 2D Array
            1. [Pascals Triangle](https://leetcode.com/problems/pascals-triangle/)
        * String
            1. [Add Binary](https://leetcode.com/problems/add-binary/)
        * Two Pointer Technique
    9. [Hash Table](https://leetcode.com/explore/learn/card/hash-table/)
    10. [Binary Search](https://leetcode.com/explore/learn/card/binary-search/)
    11. [Sorting](https://leetcode.com/explore/learn/card/sorting/)
    12. [Bit Manipulation](https://leetcode.com/explore/learn/card/bit-manipulation/)
    13. [Recursion 1](https://leetcode.com/explore/learn/card/recursion-i/)
    14. [Recursion 2](https://leetcode.com/explore/learn/card/recursion-ii/)
    15. [Dynamic Programming](https://leetcode.com/explore/learn/card/dynamic-programming/)


<h2 align="center" id="basics"> Basic Syntax</h2>

### Object: Boxing and Unboxing
- All types, whether pre-defined or user-defined, inherit from a base type called **Object**
- Casting to an Object is considered "boxing" a value, while casting an object to a specific type is called "unboxing" a value

### Arrays
- Different ways of initializing arrays with values -
    ```c#
    int[] intArray1 = new int[5]; 

    int[] intArray2 = new int[5]{1, 2, 3, 4, 5};

    int[] intArray2 = {1, 2, 3, 4, 5};
    ```
    * You can use `foreach(int i in intArray)`
    * Arrays also have a built-in `Length` property (useful for loops)
- Multi-dimensional Arrays
    ```c#
    // creates a two-dimensional array of 
    // four rows and two columns.
    int[, ] intarray = new int[4, 2];
    ```
- Array of Arrays (aka "Jagged Arrays")
    ```c#
    // Declare the array of two elements:
    int[][] arr = new int[2][];

    // Initialize the elements:
    arr[0] = new int[5] { 1, 3, 5, 7, 9 };
    arr[1] = new int[4] { 2, 4, 6, 8 };
    ```

### Strings
- `System.String` = `string` keyword 
- Along with objects, this is a reference data type since it's a reference to a contigious sequence of unicode characters

### Pointers
- `& Address Operator` : used to determine the address of a variable
- `* Indirection Operator` : used to access the value of an address
- You can access the data from Pointers by doing one of the following -
    * `somePointer->someValue Arrow Operator`
    * `(*somePointer).someValue Dereference operator`
```c#
// declare variable
int n = 10;
    
// store variable n address 
// location in pointer variable p
int* p = &n;
Console.WriteLine("Value :{0}", n);
Console.WriteLine("Address :{0}", (int)p);
```

### Classes
- Creating a Simple Class
    ```c#
    public class Point
    {
        public int X { get; }
        public int Y { get; }
        
        public Point(int x, int y) => (X, Y) = (x, y);
    }

    var p1 = new Point(0, 0);
    var p2 = new Point(10, 20);
    ```
- Inheritance
    ```c#
    using System;
    class SomeBaseClass 
    {
        public string name;
        public string subject;

        public void readers(string name, string subject)
        {
            this.name = name;
            this.subject = subject;
            Console.WriteLine("Myself: " + name);
            Console.WriteLine("My Favorite Subject is: " + subject);
        }
    }

    class ClassThatInherits : SomeBaseClass 
    {
        public ClassThatInherits()
        {
            Console.WriteLine("ClassThatInherits constructor!");
        }
    }

    class DriverClass 
    {
        static void Main(string[] args)
        {

            // creating object of derived class
            ClassThatInherits c = new ClassThatInherits();

            // calling the method of base class
            // using the derived class object
            c.readers("Kirti", "C#");
        }
    }
    ```

### Type Parameters
- If you define parameters inside brackets after a class, you can think of those names as generic types that can be defined later
- Struct, interface, and delegate types can also be generic
- `Pair<int, string>` below is called a `constructed type`
```c#
public class Pair<TFirst, TSecond>
{
    public TFirst First { get; }
    public TSecond Second { get; }
    
    public Pair(TFirst first, TSecond second) => 
        (First, Second) = (first, second);
}

var pair = new Pair<int, string>(1, "two");
int i = pair.First;     //TFirst int
string s = pair.Second; //TSecond string
```

### Structs
```c#
public struct Point
{
    public double X { get; }
    public double Y { get; }
    
    public Point(double x, double y) => (X, Y) = (x, y);
}
```

### Interfaces
- NEED PRACTICE TO SOLIDIFY KNOWLEDGE

### Enums
```c#
public enum SomeRootVegetable
{
    HorseRadish,
    Radish,
    Turnip
}

[Flags]
public enum Seasons
{
    None = 0,
    Summer = 1,
    Autumn = 2,
    Winter = 4,
    Spring = 8,
    All = Summer | Autumn | Winter | Spring
}

var turnip = SomeRootVegetable.Turnip;

var spring = Seasons.Spring;
var startingOnEquinox = Seasons.Spring | Seasons.Autumn;
var theYear = Seasons.All;
```

### Tuples
```c#
(double Sum, int Count) t2 = (4.5, 3);
Console.WriteLine($"Sum of {t2.Count} elements is {t2.Sum}.");
//Output:
//Sum of 3 elements is 4.5.
```

<h2 align="center" id="std"> System.Collections </h2>

### [ArrayList](https://www.geeksforgeeks.org/c-sharp-arraylist-class/)
```c#
using System;
using System.Collections;
using System.Collections.Generic;

class Solution 
{
	public static void Main()
	{
        //Initialize
		ArrayList arrayList = new ArrayList();

		// Important Methods
		arrayList.Add("Hello");
        arrayList.AddRange(arrayList); // Adds the elements of an ICollection to the end of this list
        arrayList.Clear();
        arrayList.Contains("Hello");
        arrayList.GetRange(0, 6); // Returns a sub-arrayList of this range
        arrayList.IndexOf("Hello");
        arrayList.LastIndexOf("Hello");
        arrayList.Sort(); //Uses quick sort to sort the array
        arrayList.BinarySearch("Hello");
        arrayList.TrimToSize(); //Sets the capacity to the # of elements
        arrayList.Max();
        arrayList.Min();

        //Static Methods
        Array.IndexOf(arrayList, someVal);

        // Important Properties
		arrayList.Count;
        arrayList.Capacity;
        arrayList[0] = "NO HELLO";

        //Loop
        foreach(String str in arrayList)
        {
            Console.WriteLine(str);
        }
	}
}
```

### Queue
```c#
using System;
using System.Collections;

class Solution 
{
    static void Main(string[] args)
    {
        Queue q = new Queue();

        /* Properties */
        q.Count;

        /* Methods */
        q.Enqueue("one"); //Adds to the end of the queue
        string curr = q.Dequeue(); //Removes and Returns from the start of the q
        curr = q.Peek(); //Returns the front of the q without Removing it

        q.Contains(someObject);
        q.CopyTo([1,2,3], someIndex);
        q.ToArray();
        q.Clear();
    }
}
```

### [Hashtable](https://www.geeksforgeeks.org/c-sharp-hashtable-class/)
```c#
using System;
using System.Collections;

class Solution 
{
    static void Main(string[] args)
    {
        /*Initialization*/
        Hashtable hashMap = new Hashtable();

        /*Important Methods*/
        hashMap.Add("1", "Hello,");
        hashMap.Add("2", "World!");

        hashMap.Contains("1");
        hashMap.ContainsKey("1");
        hashMap.ContainsValue("Hello,");

        hashMap.Remove("1");
        hashMap.Clear():

        /*Important Properties*/
        hashMap.Count;
        ICollection keys = hashMap.Keys;
        ICollection vals = hashMap.Values;

        foreach(var key in keys)
            Console.WriteLine(key + ": " + hashMap[key]);
    }
}
```

### Dictionary
```c#
using System.Collections.Generic;

Dictionary<string, int> ages = new Dictionary<string, int>();

// Add some key-value pairs to the dictionary
ages.Add("Alice", 25);
ages.Add("Bob", 30);
ages.Add("Charlie", 35);

// Look up a value by key
int age = ages["Alice"];

// Modify a value
ages["Bob"] = 31;

//Properties
ages.Count;
ICollection keys = ages.Keys;
ICollection vals = ages.Values;

//Methods
ages.Add("ME", 26);
ages.Clear();
ages.Remove("ME");
ages.ContainsKey("ME");
ages.ContainsValue(26);

// Iterate through the dictionary
foreach (KeyValuePair<string, int> person in ages) {
   Console.WriteLine($"{person.Key} is {person.Value} years old");
}

```

<h2 align="center" id="std-generic"> System.Collections.Generic </h2>

### [HashSet](https://www.geeksforgeeks.org/hashset-in-c-sharp-with-examples/)
```c#
using System;
using System.Collections.Generic;

class Solution 
{
    static public void Main()
    {
        HashSet<string> hashSet1 = new HashSet<string>();
        HashSet<int> hashSet2 = new HashSet<int>() {5, 2, 4, 8};

        hashSet.Add("C");
        hashSet.Add("C++");
        hashSet.Add("C#");
        hashSet.Add("Java");
        hashSet.Add("Ruby");

        foreach(var key in hashSet1)
            Console.WriteLine(key);

        hashSet.Contains("Ruby");
        hashSet.Remove("Ruby");
        hashSet.Clear();

        // you have to do "using System.Linq" ANY Class that implements IEnumerable can use these methods. Most Collections do.
        hashSet.First();
        hashSet.FirstOrDefault();
    }
}
```

### Stack
```c#
using System.Collections.Generic;

Stack<int> s = new Stack<int>();

//Methods
s.Clear();
s.Contains(5);
s.Push(1);
int top = s.Peek();
int popped = s.Pop();

//Properties
s.Count;
```

