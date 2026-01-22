# **Arrays & Hashing Question-1**

# **Contains Duplicates**

---

## **Intuition**

> **⚠️ Note:** This intuition is taken from the **NeetCode website**.

We can check every pair of different elements in the array and return true if any pair has equal values.
This is the most intuitive approach because it directly compares all possible pairs, but it is also the least efficient since it examines every combination.

## **Approaches & Complexity Analysis**

### **1️⃣ Brute Force**

- **Time Complexity:** `O(n²)`
- **Space Complexity:** `O(1)`

---

### **2️⃣ Sort the Array**

- **Time Complexity:** `O(n log n)`
- **Space Complexity:** `O(1)`

---

### **3️⃣ Use HashMap / HashSet**

- **Time Complexity:** `O(n)`
- **Space Complexity:** `O(n)`

---

## **2️⃣ Sort the Array**

````cpp
class Solution {
public:
    bool containsDuplicate(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        for (int i = 1; i < nums.size(); i++) {
            if (nums[i] == nums[i - 1])
                return true;
        }
        return false;
    }
};


## **3️⃣ Solution Using HashMap / HashSet**
```cpp
class Solution {                                                    class Solution {
public:                                                             public:
    bool containsDuplicate(vector<int>& nums) {        =>              bool containsDuplicate(vector<int>& nums) {
        unordered_set<int> s;                                             unordered_map<int,int> mp;
        for(int num:nums){                                                for(int num:nums){
            if(s.count(num)) return true;                                      if(mp[num]>0) return true;
            s.insert(num);                                                     mp[num]++;
        }                                                                   }
        return false;                                                       return false;
    }                                                                    }
};                                                                    };
````
