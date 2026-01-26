# **Arrays & Hashing Question-3**

# **Two Sum**

---

## **Intuition**

> **⚠️ Note:** This intuition is taken from the **NeetCode website**.

We are given an array of integers and a target value.  
The goal is to find **two distinct indices** such that the sum of their values equals the target.

A brute force approach would check every possible pair, but this is inefficient.  
To optimize, we can use a **hash map** to remember numbers we’ve already seen and check if the required complement exists.

---

## **Solutions Overview**

| Solution | Approach               | Time Complexity | Space Complexity |
| -------- | ---------------------- | --------------- | ---------------- |
| 1        | Brute Force            | `O(n²)`         | `O(1)`           |
| 2        | Sorting                | `O(nlogn)`      | `O(1)`           |
| 3        | HashMap (Two/One Pass) | `O(n)`          | `O(n)`           |

## **2nd Solution – Sorting**

```cpp
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        vector<pair<int,int>> temp;
        for (int i=0; i<nums.size();i++) {
            temp.push_back({nums[i],i});
        }
        sort(temp.begin(), temp.end());
        int j=nums.size()-1,i=0;
        while(j>i){
            if(temp[i].first+temp[j].first>target){
                j--;
            }
            else if(temp[i].first+temp[j].first<target){
                i++;
            }
            else{
                return {min(temp[i].second,temp[j].second),
                        max(temp[i].second,temp[j].second)};
            }
        }
        return {};
    }
};
```

## **3rd Solution – (Two Pass/One pass)Hashmap**

```cpp
class Solution {                                                class Solution {
public:                                                         public:
    vector<int> twoSum(vector<int>& nums, int target) {             vector<int> twoSum(vector<int>& nums, int target) {
        unordered_map<int,int> mp;                          =>        unordered_map<int,int> mp;
        for(int i=0;i<nums.size();i++){                               for(int i=0;i<nums.size();i++){
            mp[nums[i]]=i;                                                int comp=target-nums[i];
        }                                                                 if(mp.find(comp)!=mp.end()){
        for(int i=0;i<nums.size();i++){                                       return {mp[comp],i};
            int comp=target-nums[i];                                      }
            if(mp.find(comp)!=mp.end() && mp[comp]!=i){                   mp[nums[i]]=i;
                return {i,mp[comp]};                                   }
            }                                                          return {};
        }                                                           }
        return {};                                                };

    }
};
```
