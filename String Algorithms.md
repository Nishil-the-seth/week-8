# **String Algorithms**


## **1. Introduction to String Algorithms**

Many string problems can be easily solved in O(n 2 ) time, but the challenge is to find algorithms that work in O(n) or O(nlogn) time. For example, a fundamental string processing problem is the pattern matching problem: given a string of length n and a pattern of length m, our task is to find the occurrences of the pattern in the string. For example, the pattern ABC occurs two times in the string ABABCBABC. The pattern matching problem can be easily solved in O(nm) time by a brute force algorithm that tests all positions where the pattern may occur in the string. However, in this chapter, we will see that there are more efficient algorithms that require only O(n+ m) time. 


---


## **2. Basic String Operations**

Before diving into advanced algorithms, ensure familiarity with:



* Comparing strings 

* Reversing strings 

* Substring extraction 

* Prefix and suffix handling 


These can be done using built-in functions in C++, Python, or JavaScript.


---


## **5. Z Algorithm**

The Z-array z of a string s of length n contains for each k = 0,1,...,n − 1 the length of the longest substring of s that begins at position k and is a prefix of 247 s. Thus, z[k] = p tells us that s[0... p − 1] equals s[k...k + p − 1]. Many string processing problems can be efficiently solved using the Z-array. For example, the Z-array of ACBACDACBACBACDA is as follows:

| i         | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 |10 |11 |12 |13 |14 |15 |
|-----------|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| s[i]      | A | C | B | A | C | D | A | C | B | A | C | B | A | C | D | A |
| z[i]      |   |   |   |   |   |   | 5 |   |   |   |   |   |   |   |   |   |


In this case, for example, z[6] = 5, because the substring ACBAC of length 5 is a prefix of s, but the substring ACBACB of length 6 is not a prefix of s.

Algorithm description Next we describe an algorithm, called the Z-algorithm2 , that efficiently constructs the Z-array in O(n) time. The algorithm calculates the Z-array values from left to right by both using information already stored in the Z-array and comparing substrings character by character. To efficiently calculate the Z-array values, the algorithm maintains a range [x, y] such that s[x... y] is a prefix of s and y is as large as possible. Since we know that s[0... y− x] and s[x... y] are equal, we can use this information when calculating Z-values for positions x+1, x+2,..., y. At each position k, we first check the value of z[k − x]. If k +z[k − x] &lt; y, we know that z[k] = z[k − x]. However, if k +z[k − x] ≥ y, s[0... y− k] equals s[k... y], and to determine the value of z[k] we need to compare the substrings character by character. Still, the algorithm works in O(n) time, because we start comparing at positions y− k +1 and y+1. For example, let us construct the following Z-array: 

| i         | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 |10 |11 |12 |13 |14 |15 |
|-----------|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| s[i]      | A | C | B | A | C | D | A | C | B | A | C | B | A | C | D | A |
| z[i]      |   |   |   |   |   |   | 5 |   |   |   |   |   |   |   |   |   |
| [x, y]    |                       | 6 → 10                         |


After calculating the value z[6] = 5, the current [x, y] range is [6,10]:

| i         | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 |10 |11 |12 |13 |14 |15 |
|-----------|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| s[i]      | A | C | B | A | C | D | A | C | B | A | C | B | A | C | D | A |
| z[i]      |   |   |   |   |   |   | 5 | 0 | 0 |   |   |   |   |   |   |   |


Now we can calculate subsequent Z-array values efficiently, because we know that s[0...4] and s[6...10] are equal. First, since z[1] = z[2] = 0, we immediately know that also z[7] = z[8] = 0: 

| i         | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 |10 |11 |12 |13 |14 |15 |
|-----------|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| s[i]      | A | C | B | A | C | D | A | C | B | A | C | B | A | C | D | A |
| z[i]      |   |   |   |   |   |   | 5 | 0 | 0 | 2 |   |   |   |   |   |   |


Then, since z[3] = 2, we know that z[9] ≥ 2:

| i         | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 |10 |11 |12 |13 |14 |15 |
|-----------|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| s[i]      | A | C | B | A | C | D | A | C | B | A | C | B | A | C | D | A |
| z[i]      |   |   |   |   |   |   | 5 | 0 | 0 | 2 |   |   |   |   |   |   |
|           |                             ↑             ↑                   |
| matched   |       A C B A C   =   A C B A C                               |


However, we have no information about the string after position 10, so we need to compare the substrings character by character: 

| i         | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 |10 |11 |12 |13 |14 |15 |
|-----------|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| s[i]      | A | C | B | A | C | D | A | C | B | A | C | B | A | C | D | A |
| z[i]      |   |   |   |   |   |   | 5 | 0 | 0 | 2 | 7 |   |   |   |   |   |
| [x, y]    |                                   | 9 → 15                   |


It turns out that z[9] = 7, so the new [x, y] range is [9,15]:

| i         | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 |10 |11 |12 |13 |14 |15 |
|-----------|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| s[i]      | A | C | B | A | C | D | A | C | B | A | C | B | A | C | D | A |
| z[i]      |   |   |   |   |   |   | 5 | 0 | 0 | 2 | 7 |   |   |   |   |   |


After this, all the remaining Z-array values can be determined by using the information already stored in the Z-array:

| i         | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 |10 |11 |12 |13 |14 |15 |
|-----------|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| s[i]      | A | C | B | A | C | D | A | C | B | A | C | B | A | C | D | A |
| z[i]      |   |   |   |   |   |   | 5 | 0 | 0 | 2 | 7 | 0 | 0 | 0 | 0 | 0 |


Using the Z-array It is often a matter of taste whether to use string hashing or the Z-algorithm. Unlike hashing, the Z-algorithm always works and there is no risk for collisions. On the other hand, the Z-algorithm is more difficult to implement and some problems can only be solved using hashing. As an example, consider again the pattern matching problem, where our task is to find the occurrences of a pattern p in a string s. We already solved this problem efficiently using string hashing, but the Z-algorithm provides another way to solve the problem. A usual idea in string processing is to construct a string that consists of multiple strings separated by special characters. In this problem, we can construct a string p#s, where p and s are separated by a special character # that does not occur in the strings. The Z-array of p#s tells us the positions where p occurs in s, because such positions contain the length of p. For example, if s =HATTIVATTI and p =ATT, the Z-array is as follows: A T T # H A T T I V A T T I – 0 0 0 0 3 0 0 0 0 3 0 0 0 0 1 2 3 4 5 6 7 8 9 10 11 12 13 The positions 5 and 10 contain the value 3, which means that the pattern ATT occurs in the corresponding positions of HATTIVATTI. The time complexity of the resulting algorithm is linear, because it suffices to construct the Z-array and go through its values. 


### **Implementation**

 Here is a short implementation of the Z-algorithm that returns a vector that corresponds to the Z-array.

**C++:**


```
vector<int> z_array(const string& s) {
    int n = s.size();
    vector<int> z(n);
    int x = 0, y = 0;

    for (int i = 1; i < n; i++) {
        if (i <= y)
            z[i] = min(z[i - x], y - i + 1);
        while (i + z[i] < n && s[z[i]] == s[i + z[i]]) {
            x = i;
            y = i + z[i];
            z[i]++;
        }
    }

    return z;
}
```


**Python:**


```
def z_array(s):
    n = len(s)
    z = [0] * n
    x = y = 0
    for i in range(1, n):
        if i <= y:
            z[i] = min(z[i - x], y - i + 1)
        while i + z[i] < n and s[z[i]] == s[i + z[i]]:
            z[i] += 1
            x = i
            y = i + z[i] - 1
    return z
```


**Javascript;**


```
function zArray(s) {
    const n = s.length;
    const z = Array(n).fill(0);
    let x = 0, y = 0;

    for (let i = 1; i < n; i++) {
        if (i <= y) {
            z[i] = Math.min(z[i - x], y - i + 1);
        }
        while (i + z[i] < n && s[z[i]] === s[i + z[i]]) {
            z[i]++;
            x = i;
            y = i + z[i] - 1;
        }
    }
    return z;
}
```



---


## **6. Trie (Prefix Tree)**

A trie is a rooted tree that maintains a set of strings. Each string in the set is stored as a chain of characters that starts at the root. If two strings have a common prefix, they also have a common chain in the tree. For example, consider the following trie: 

Pic

This trie corresponds to the set {CANAL,CANDY,THE,THERE}. The character * in a node means that a string in the set ends at the node. Such a character is needed, because a string may be a prefix of another string. For example, in the above trie, THE is a prefix of THERE. We can check in O(n) time whether a trie contains a string of length n, because we can follow the chain that starts at the root node. We can also add a string of length n to the trie in O(n) time by first following the chain and then adding new nodes to the trie if necessary. Using a trie, we can find the longest prefix of a given string such that the prefix belongs to the set. Moreover, by storing additional information in each node, we can calculate the number of strings that belong to the set and have a given string as a prefix. A trie can be stored in an array `int trie[N][A];`

where N is the maximum number of nodes (the maximum total length of the strings in the set) and A is the size of the alphabet. The nodes of a trie are numbered 0,1,2,... so that the number of the root is 0, and trie[s][c] is the next node in the chain when we move from node s using character c.

**C++ implementation:**


```
#include <iostream>
#include <unordered_map>
using namespace std;

struct TrieNode {
    unordered_map<char, TrieNode*> children;
    bool isEndOfWord = false;
};

class Trie {
public:
    TrieNode* root;

    Trie() {
        root = new TrieNode();
    }

    void insert(string word) {
        TrieNode* node = root;
        for (char ch : word) {
            if (!node->children.count(ch))
                node->children[ch] = new TrieNode();
            node = node->children[ch];
        }
        node->isEndOfWord = true;
    }

    bool search(string word) {
        TrieNode* node = root;
        for (char ch : word) {
            if (!node->children.count(ch))
                return false;
            node = node->children[ch];
        }
        return node->isEndOfWord;
    }
};
```


**Python implementation:**


```
class TrieNode:
    def __init__(self):
        self.children = {}
        self.is_end_of_word = False

class Trie:
    def __init__(self):
        self.root = TrieNode()

    def insert(self, word):
        node = self.root
        for ch in word:
            if ch not in node.children:
                node.children[ch] = TrieNode()
            node = node.children[ch]
        node.is_end_of_word = True

    def search(self, word):
        node = self.root
        for ch in word:
            if ch not in node.children:
                return False
            node = node.children[ch]
        return node.is_end_of_word
```


**JavaScript Implementation:**


```
class TrieNode {
    constructor() {
        this.children = {};
        this.isEndOfWord = false;
    }
}

class Trie {
    constructor() {
        this.root = new TrieNode();
    }

    insert(word) {
        let node = this.root;
        for (let ch of word) {
            if (!node.children[ch])
                node.children[ch] = new TrieNode();
            node = node.children[ch];
        }
        node.isEndOfWord = true;
    }

    search(word) {
        let node = this.root;
        for (let ch of word) {
            if (!node.children[ch])
                return false;
            node = node.children[ch];
        }
        return node.isEndOfWord;
    }
}

```
# Practice Problems

## Easy
1. [strStr (First Occurrence)](https://leetcode.com/problems/find-the-index-of-the-first-occurrence-in-a-string/)
2. [String Matching in an Array](https://leetcode.com/problems/string-matching-in-an-array/)
3. [Naive Pattern Searching (GfG)](https://www.geeksforgeeks.org/naive-algorithm-for-pattern-searching/)
4. [Repeated Substring Pattern](https://leetcode.com/problems/repeated-substring-pattern/)
5. [Remove All Occurrences of a Substring](https://leetcode.com/problems/remove-all-occurrences-of-a-substring/)

## Medium
1. [Search Pattern (KMP) – GfG](https://www.geeksforgeeks.org/problems/search-pattern0205/1)
2. [Z‑Algorithm Pattern Search – GfG](https://www.geeksforgeeks.org/dsa/z-algorithm-linear-time-pattern-searching-algorithm/)
3. [Longest Happy Prefix](https://leetcode.com/problems/longest-happy-prefix/)
4. [Implement Trie (Prefix Tree)](https://leetcode.com/problem-list/trie/)

## Hard
1. [Shortest Palindrome](https://leetcode.com/problems/shortest-palindrome/)
2. [Stamping the Sequence](https://leetcode.com/problems/stamping-the-sequence/)
3. [Repeated String Match](https://leetcode.com/problems/repeated-string-match/)
