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
- [System.Collections]()
    * [ArrayList]()
- Leetcodes
    1. [Linked List](https://leetcode.com/explore/learn/card/linked-list/)
        * Singly Linked List
            1. [Design Linked List](https://leetcode.com/problems/design-linked-list/description/)
        * Two Pointer Technique
        * Classic Problems
        * Doubly Linked List
        * More Problems
    2. [Binary Search Tree](https://leetcode.com/explore/learn/card/introduction-to-data-structure-binary-search-tree/)
    3. [N-ary Tree](https://leetcode.com/explore/learn/card/n-ary-tree/)
    4. [Tries](https://leetcode.com/explore/learn/card/trie/)
    5. [Graph](https://leetcode.com/explore/learn/card/graph/)
    6. [Queue & Stack](https://leetcode.com/explore/learn/card/queue-stack/)
    7. [Heaps](https://leetcode.com/explore/learn/card/heap/)
    8. [Array and String](https://leetcode.com/explore/learn/card/array-and-string/)
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