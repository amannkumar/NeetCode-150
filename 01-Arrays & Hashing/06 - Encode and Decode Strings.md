# **Arrays & Hashing Question-8**

# **Encode and Decode Strings**

---

## **Intuition**

> **⚠️ Note:** This intuition is taken from the **NeetCode website**.

We are given a list of strings and need to:

- **Encode** them into a single string
- **Decode** that string back into the original list

The main challenge:
👉 **Strings may contain any characters**, including delimiters like `#`, `,`, or spaces.

So we **cannot rely on a simple separator**.

---

## **Key Idea**

Instead of separating strings by a character, we **prefix each string with its length**.

This guarantees:

- Unambiguous decoding
- Works for any character set

---

## **Encoding Format**

["neet", "code"] → "4,4#neetcode"

---

## **Solutions Overview**

| Solution | Approach                  | Time Complexity | Space Complexity |
| -------- | ------------------------- | --------------- | ---------------- |
| 1        | Length Encoding (Optimal) | `O(n)`          | `O(1)`           |

> `n` = total number of characters across all strings

---

## **1st Solution – Length-Based Encoding**

```cpp
class Solution {
public:

    string encode(vector<string>& strs) {
        vector<int> sizes;
        string res = "";

        for(auto s:strs){
            //sizes.push_back(s.size());


            res+=to_string(s.size())+",";
        }
        res+="#";
        for(auto s:strs) res+=s;
        return res;

    }

    vector<string> decode(string s) {
        int i=0;
        vector<string> final;
        vector<int> sizes;
        while(s[i]!='#'){
            string st="";
            while(s[i]!=','){
                st+=s[i];
                i++;
            }
            sizes.push_back(stoi(st));
            i++;
        }
        i++;
        for(auto sz:sizes){
            final.push_back(s.substr(i,sz));
            i+=sz;

        }
        return final;
    }
};
```
