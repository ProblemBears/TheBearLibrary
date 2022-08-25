# Language Comparisons

## Table of Contents
---
1. [Dynamic Arrays](#1)
    - [Instantiation](#1-a)
    - [Push](#1-b)

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

### Push / Pop
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
    a.erase(a.begin() + 2); //arg2 : Erase range
    a.push_back(32);
    a.pop_back();
    ```
- Python
    ```python
    a.append(32)
    ```
- JavaScript
    ```js
    a.push(32);
    ```