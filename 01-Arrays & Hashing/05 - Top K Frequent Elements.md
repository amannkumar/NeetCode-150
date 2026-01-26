# **Arrays & Hashing Question-5**

# **Top K Frequent Elements**

---

## **Intuition**

> **⚠️ Note:** This intuition is taken from the **NeetCode website**.

To find the **k most frequent elements**, we must:

1. **Count the frequency** of each number
2. **Select the top k elements** based on their frequencies

The challenge is to do this efficiently **without sorting the entire array**.

We can optimize using:

- **Bucket Sort** (frequency-based grouping)
- **Min Heap** (keep top k elements only)

---

## **Solutions Overview**

| Solution | Approach                  | Time Complexity | Space Complexity |
| -------- | ------------------------- | --------------- | ---------------- |
| 1        | Sorting by Frequency      | `O(n log n)`    | `O(n)`           |
| 2        | Min Heap (Priority Queue) | `O(n log k)`    | `O(n)`           |
| 3        | Bucket Sort (Optimal)     | `O(n)`          | `O(n)`           |

---

## **1st Solution – Sorting by Frequency**

```cpp
class Solution {
public:
    vector<int> topKFrequent(vector<int>& nums, int k) {
        unordered_map<int, int> count;
        for (int num : nums) {
            count[num]++;
        }

        vector<pair<int, int>> arr;
        for (const auto& p : count) {
            arr.push_back({p.second, p.first});
        }
        sort(arr.rbegin(), arr.rend());

        vector<int> res;
        for (int i = 0; i < k; ++i) {
            res.push_back(arr[i].second);
        }
        return res;
    }
};
```

## **2nd Solution – Min Heap**

```cpp
class Solution {
public:
    vector<int> topKFrequent(vector<int>& nums, int k) {
        unordered_map<int,int> mp;
        for(auto i:nums) mp[i]++;
        priority_queue<pair<int,int>,vector<pair<int,int>>,greater<pair<int,int>>> minh;
        for(auto it:mp){
            minh.push({it.second,it.first});
            if(minh.size()>k) minh.pop();
        }
        vector<int> ans;
        while(!minh.empty()){
            ans.push_back(minh.top().second);
            minh.pop();
        }
        return ans;
    }
};
```

## **3rd Solution – Bucket Sort**

```cpp
class Solution {
public:
    vector<int> topKFrequent(vector<int>& nums, int k) {
        unordered_map<int,int> mp;
        for(auto i:nums) mp[i]++;
        vector<vector<int>> st(nums.size() + 1);
        for(auto it:mp){
            st[it.second].push_back(it.first);
        }
        vector<int> ans;

        for(int i=nums.size();i>=0;i--){
            for(int j=0;j<st[i].size();j++){
                ans.push_back(st[i][j]);
                if(ans.size()==k) break;
            }
            if(ans.size()==k) break;
        }
        return ans;
    }
};
```
