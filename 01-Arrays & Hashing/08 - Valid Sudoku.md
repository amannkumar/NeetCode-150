# **Arrays & Hashing Question-6**

# **Valid Sudoku**

---

## **Intuition**

> **⚠️ Note:** This intuition is taken from the **NeetCode website**.

A Sudoku board is valid if **no number repeats** in:

- Any **row**
- Any **column**
- Any **3 × 3 sub-box**

We **do not need to solve** the Sudoku.
We only need to **verify constraints**.

The key idea is to **track seen numbers** efficiently while traversing the board.

---

## **Key Observations**

- Board size is fixed: **9 × 9**
- Valid digits are `'1'` to `'9'`
- Empty cells are represented by `'.'`
- Checking duplicates is perfect for **hash sets**

---

## **Solutions Overview**

| Solution | Approach                      | Time Complexity | Space Complexity |
| -------- | ----------------------------- | --------------- | ---------------- |
| 1        | HashSet (Row + Col + Box)     | `O(1)`          | `O(1)`           |
| 2        | 2D Boolean Arrays (Optimized) | `O(1)`          | `O(1)`           |

> **Why O(1)?**  
> Because the board size is constant (9×9), runtime and memory do not scale with input size.

---

## **1st Solution – HashSet Based**

### **Idea**

- Use **3 hash sets** per row:
  - One for **rows**
  - One for **columns**
  - One for **3×3 boxes**
- While traversing the board:
  - Skip `'.'`
  - If a number already exists in any set → invalid

### **Box Index Formula**

```text
boxIndex = (row / 3) * 3 + (col / 3)
```

```cpp
class Solution {
public:
    bool isValidSudoku(vector<vector<char>>& board) {
        unordered_set<int> col[9],row[9],box[9];
        for(int i=0;i<board.size();i++){
            for(int j=0;j<board[0].size();j++){
                int val=board[i][j];
                if(val=='.') continue;
                int boardInd=(i/3)*3+(j/3);
                if(row[i].count(val)>0 ||
                col[j].count(val)>0 || box[boardInd].count(val)>0)
                    return false;

                row[i].insert(val);
                col[j].insert(val);
                box[boardInd].insert(val);
            }
        }
        return true;
    }
};

```

## **2nd Solution – 2D Boolean Arrays**

```cpp
class Solution {
public:
    bool isValidSudoku(vector<vector<char>>& board) {
        unordered_set<string> seen;

        for (int r = 0; r < 9; r++) {
            for (int c = 0; c < 9; c++) {
                char val = board[r][c];
                if (val == '.') continue;

                string rowKey = "row" + to_string(r) + val;
                string colKey = "col" + to_string(c) + val;
                string boxKey = "box" + to_string((r / 3) * 3 + (c / 3)) + val;

                if (seen.count(rowKey) || seen.count(colKey) || seen.count(boxKey))
                    return false;

                seen.insert(rowKey);
                seen.insert(colKey);
                seen.insert(boxKey);
            }
        }
        return true;
    }
};

```
