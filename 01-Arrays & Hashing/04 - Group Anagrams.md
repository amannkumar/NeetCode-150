# **Arrays & Hashing Question-4**

# **Group Anagrams**

---

## **Intuition**

> **⚠️ Note:** This intuition is taken from the **NeetCode website**.

Two strings are anagrams if they contain **the same characters with the same frequencies**, just arranged differently.

To group anagrams together, we need a **common representation (key)** for all strings that are anagrams of each other.

There are two common ways to create this key:

- **Sort the string** → anagrams become identical after sorting
- **Count character frequencies** → use counts as a signature

Strings with the same key belong to the same group.

---

## **Solutions Overview**

| Solution | Approach                      | Time Complexity  | Space Complexity |
| -------- | ----------------------------- | ---------------- | ---------------- |
| 1        | Sorting as Key                | `O(n * k log k)` | `O(n * k)`       |
| 2        | Character Frequency Count Key | `O(n * k)`       | `O(n * k)`       |

> **n** = number of strings  
> **k** = maximum length of a string

## **1st Solution – Sorting as Key**

```cpp
class Solution {
public:
    vector<vector<string>> groupAnagrams(vector<string>& strs) {
        unordered_map<string,vector<string>> mp;
        for(string s:strs){
            string temp=s;
            sort(temp.begin(),temp.end());
            mp[temp].push_back(s);
        }
        vector<vector<string>> ans;
        for(auto it:mp){
            ans.push_back(it.second);
        }
        return ans;
    }
};
```

## **2nd Solution – Character Frequency Count Key**

```cpp
class Solution {
public:
    vector<vector<string>> groupAnagrams(vector<string>& strs) {
        unordered_map<string,vector<string>> mp;
        for(auto s:strs){
            vector<int> bucket(26,0);
            int len=s.length();
            for(int i=0;i<len;i++){
                bucket[s[i]-'a']++;
            }
            string temp="";
            for(int i=0;i<26;i++){
                temp+="#"+to_string(bucket[i]);
            }
            mp[temp].push_back(s);
        }
        vector<vector<string>> ans;
        for(auto it:mp){
            ans.push_back(it.second);
        }
        return ans;
    }
};
```
