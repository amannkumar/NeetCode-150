# **Arrays & Hashing Question-2**

# **Valid Anagrams**

---

## **Intuition**

> **⚠️ Note:** This intuition is taken from the **NeetCode website**.

If two strings are anagrams, they must contain **exactly the same characters with the same frequencies**.

There are multiple ways to verify this:

- By **sorting** both strings and comparing them
- By **counting character frequencies** using hash maps

If all characters and their counts match, the strings are anagrams.

---

## **Solutions Overview**

| Solution | Approach                | Time Complexity | Space Complexity |
| -------- | ----------------------- | --------------- | ---------------- |
| 1        | Sorting (Brute Force)   | `O(n log n)`    | `O(1)`           |
| 2        | Two HashMaps            | `O(n)`          | `O(n)`           |
| 3        | One HashMap (Optimized) | `O(n)`          | `O(n)`           |

---

## **1st Solution – Brute Force (Sorting)**

### **Idea**

- If the lengths are different → not an anagram
- Sort both strings
- Compare the sorted strings

## If they are identical, the strings are anagrams.

## **1st Solution – Brute Force (Sorting)**

```cpp
class Solution {
public:
    bool isAnagram(string s, string t) {
        if(s.length()!=t.length())
            return false;

        sort(s.begin(),s.end());
        sort(t.begin(),t.end());

        return s==t;
    }
};
```

## **2nd Solution – Using 2 HashMap**

```cpp
class Solution {
public:
    bool isAnagram(string s, string t) {
        if(s.length()!=t.length())
            return false;
        unordered_map<char,int> s_map,t_map;

        for(int i=0;i<s.length();i++){
            s_map[s[i]]++;
            t_map[t[i]]++;
        }
        return s_map==t_map;
    }
};
```

## **3rd Solution – Using 1 HashMap**

```cpp
class Solution {
public:
    bool isAnagram(string s, string t) {
        if(s.length()!=t.length())
            return false;
        unordered_map<char,int> common_map;

        for(int i=0;i<s.length();i++){
            common_map[s[i]]++;
            common_map[t[i]]--;
        }
        for(auto it:common_map){
            if(it.second!=0) return false;
        }
        return true;
    }
};
```
