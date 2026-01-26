# **Arrays & Hashing Question-7**

# **Product of Array Except Self**

---

## **Intuition**

> **⚠️ Note:** This intuition is taken from the **NeetCode website**.

For each index `i`, we want the **product of all elements except `nums[i]`**.

A naive approach would compute the product for each index separately, but that would be **O(n²)** — too slow.

The key observation:

- The product except `i` is:
  (product of elements to the left of i)×(product of elements to the right of i)

  We can compute these efficiently using **prefix and suffix products**.

---

## **Constraints Insight**

- Division is **not allowed** (Solution is at the end)
- The array may contain **zeroes**
- Output must be computed in **O(n)** time

---

## **Solutions Overview**

| Solution | Approach                          | Time Complexity | Space Complexity         |
| -------- | --------------------------------- | --------------- | ------------------------ |
| 1        | Prefix + Suffix Arrays            | `O(n)`          | `O(n)`                   |
| 2        | Optimized Prefix (Constant Space) | `O(n)`          | `O(1)` (output excluded) |

---

## **1st Solution – Prefix & Suffix Arrays**

```cpp
class Solution {
public:
    vector<int> productExceptSelf(vector<int>& nums) {
        vector<int> ans(nums.size(),0);
        vector<int> left(nums.size()),right(nums.size());
        left[0]=1;
        for(int i=1;i<nums.size();i++){
            left[i]=left[i-1]*nums[i-1];
        }
        right[nums.size()-1]=1;
        for(int i=nums.size()-2;i>=0;i--){
            right[i]=right[i+1]*nums[i+1];
        }
        for(int i=0;i<nums.size();i++){
            ans[i]=left[i]*right[i];
        }
        return ans;
    }
};
```

## **2nd Solution – Optimized Prefix (Constant Space)**

```cpp
class Solution {
public:
    vector<int> productExceptSelf(vector<int>& nums) {
        vector<int> ans(nums.size(),0);
        int left=1;
        for(int i=0;i<nums.size();i++){
            ans[i]=left;
            left*=nums[i];
        }
        int right=1;
        for(int i=nums.size()-1;i>=0;i--){
            ans[i]*=right;
            right*=nums[i];
        }
        return ans;
    }
};
```

## **3rd Solution – Division Solution**

```cpp
class Solution {
public:
    vector<int> productExceptSelf(vector<int>& nums) {
        vector<int> ans(nums.size(),0);
        int zeros=0,sum=1;
        for(auto i:nums){
            if(i==0) {zeros++; continue;}
            sum*=i;
        }
        if(zeros>1) return ans;
        for(auto i=0;i<nums.size();i++){
            if(zeros>0){
                if(nums[i]==0) ans[i]=sum;
            }
            else{
                ans[i]=sum/nums[i];
            }
        }
        return ans;
    }
};

```
