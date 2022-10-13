<h1 align = "center"> Java </h2>

<h2 align="center"> Table of Contents </h2>

<h2 align="center"> Standard Library </h2>

- [This](http://docs.oracle.com/javase/7/docs/api/index.html) is a link to the Java libraries the creators made. Here are things to know:
    - `java.lang` 
        * Is for all the basic classes that are actually imported implicitly (String, Integer, Double, etc)
    - `java.util`
        * Contains all of the popular data structures (stacks, lists, heaps, etc)
    - `java.io` 
        * For file reading
        * Look into `java.util.Scanner` for simple file reading, but for any more complicated, low level file reading info, use `java.io`, its built for efficiency, while `Scanner` is for simplicity
    - `java.math` 
        * If you ever need to use arbitrary precision values (they're built-in in python, not in java)
    - `java.net` 
        * for sockets, connections, etc
    - `javax.swing` 
        * for GUI, which is an extension of the older `java.awt`
- *Effective Java, Josh Bloch* - who wrote most of these libraries in the first place - advises that you should know all of 
lang and util, and most of io

<h2 align="center" > Tidbits </h2>

### Iterate an array with a for() loop 
- `for(char c : char[] someArray)`

List (an interface = A complete abstract class that is `implemeted` by other classes as opposed to `extended`)

### Create a HashMap
```java
Map<K, V> varName = Map.of('someKey', "someVal", 'someKey1', "someVal1", ... etc.. up to 10);
```

LinkedList (https://docs.oracle.com/javase/7/docs/api/java/util/LinkedList.html): Doubly linked list implementation of List/Deque interfaces
-pollFirst() = retrieves/removes first element of the list. Returns null if empty (pollLast() is antithesis)
-addFirst/addLast(E ele)- adds an element to the start or end of the list

String:
-charAt(int index) = returns a char at this index
-toCharArray() = produces an array of chars

StringBuilder:
-append(char someChar) = adds the character at the end.
-deleteCharAt(int index) = removes a character from the given index

Arrays: A class that provides methods for array manipulation (e.g - sorting and searching)
-Arrays.copyOfRange(<originalArray>, fromIndexInclusive, toIndexExclusive)

Stack<Type> s = new Stack();
-push(e)
-pop() - get and remove
-peek() - get top element without removing