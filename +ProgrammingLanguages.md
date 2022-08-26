# Language Comparisons

## Table of Contents
---
1. [Dynamic Arrays](#1)
    - [Instantiation](#1-a)
    - [Essential Operations](#1-b)

---

<div id='1'/>

## 1. Dynamic Arrays

<div id='1-a'/>

### Instantiation/Libraries
- C++
    ```c++
    #include <vector>

    vector<int> = a;
    ```
- Python
    ```python
    a = []              #built-in
    ```
- JavaScript
    ```js
    let a = [];         //built-in
    ```

<div id='1-b' />

### Essential Operations
- C++
    ```c++
    /* Element Access */
    a[someIndex];
    a.front();
    a.back();

    /* Capacity */
    a.empty();
    a.size();

    /* Modifiers */
    a.clear();
    a.erase(a.begin() + 2); //arg2 : Erase range (excl)
    a.push_back(32);
    a.pop_back();
    ```
- Python
    ```python
    # Element Access
        a[i]

    # Capacity
        len(a)

    # Modifiers
        ## Add
        a.append(32)    # push to the end
        a.insert(i, 32) # add to any index

        ## Delete
        del a[i]        # removes without holding
        a.pop()         # pop from the end
        a.pop(i)
        a.remove(val)   # remove by value

        ## Organize
        a.sort()        # permanent sort (reverse=true)        
        sorted(a)       # temporary sort
        a.reverse()     # permanent "flip"

    ```
- JavaScript
    ```js
    /* Element Access */
        a[i]
        a.indexOf(v)        //returns index of v (-1)
        a.lastIndexOf(v)    //if none. last is reversed
                            //Optional 2nd arg for index

    /* Capacity */
        a.length()

    /* Modifiers */
        //Add
        a.push(val);
        a.unshift(v1, v2);  //adds to start

        //Delete
        a.pop();
        a.shift();          //removes from start

        //Subset Operations
        a.slice(inclusiveIndex, exlusiveIndex);
        a.concat(otherA)    //concatanates 2 arrays

        //Search
        a.includes(val);    //checks if val exists
    ```