# **Graph Question-11**

# **Pacific Atlantic Water Flow**

---

## **Intuition**

> **⚠️ Note:** This intuition is taken from the **NeetCode website**.

For each cell, we try to see where water can flow if it starts there.

**Rule:** water can flow from a cell to a neighbor only if the neighbor’s height is **<= current height** (downhill or flat).

So for every **(r, c)**:

- Run DFS exploring all paths that keep going to same or lower heights.
- If during DFS we ever step out of the grid:
  - Out of **top/left boundary** ⇒ it can reach the **Pacific**.
  - Out of **bottom/right boundary** ⇒ it can reach the **Atlantic**.

- If both oceans are reachable, include **(r, c)** in the answer.

To avoid infinite loops, we temporarily mark the current cell as visited (here by setting it to `INT_MAX`) while exploring, then restore it after backtracking.

---

## **Solutions Overview**

| Solution | Approach     | Time Complexity       | Space Complexity |
| -------- | ------------ | --------------------- | ---------------- |
| 1        | Backtracking | (O(m*n*4^{m\*n}))     | (O(m\*n))        |
| 2        | DFS (Graph)  | (O(m\*n))             | (O(m\*n))        |
| 3        | BFS (Graph)  | (O(m\*n))             | (O(m\*n))        |
| 4        | DSU (Graph)  | (O(m\*n\* 4α(m\*n))) | (O(m\*n))        |

---

## **1st Solution – Backtracking**

```cpp
class Solution {
public:
    vector<vector<int>> dir={{-1,0},{1,0},{0,-1},{0,1}};
    vector<vector<int>> pacificAtlantic(vector<vector<int>>& heights) {
        int row=heights.size(), col=heights[0].size();
        vector<vector<int>> ans;
        for(int r=0;r<row;r++){
            for(int c=0;c<col;c++){
                bool pacific=false, atlantic=false;
                backtrack(heights,r,c,INT_MAX,pacific,atlantic);
                if(pacific && atlantic)
                    ans.push_back({r,c});
            }
        }
        return ans;
    }

    void backtrack(vector<vector<int>>& heights, int r, int c, int prevVal, bool& pacific, bool& atlantic){
        if(r<0 || c<0){
            pacific=true;
            return;
        }
        if(r>=heights.size() || c>=heights[0].size()){
            atlantic=true;
            return;
        }
        if(heights[r][c]>prevVal) return;

        int temp=heights[r][c];
        heights[r][c]=INT_MAX;
        for(auto it:dir){
            backtrack(heights,r+it[0],c+it[1],temp,pacific,atlantic);
            if (pacific && atlantic)
                break;
        }
        heights[r][c]=temp;
    }
};
```

### **FAQ**

**Q1 – Why convert `heights[r][c]` to `INT_MAX`?**

**Ans:** This prevents infinite recursion when neighboring cells have the same height. Without this, DFS could revisit the same cells endlessly, causing a **runtime error**. Alternatively, a `visited` matrix could be used.

---

## **2nd Solution – DFS (Graph)**

### **Intuition**

Instead of starting DFS from every cell (slow), we reverse the thinking:

- A cell can reach an ocean if water can flow from that cell to the ocean (downhill/flat).
- Reverse it: start from the ocean borders and move **uphill/flat** (to neighbors with height >= current).
- If you can climb from the ocean to a cell, then that cell can flow down to that ocean.

So we do **2 DFS runs**:

- From all **Pacific border cells** (top row + left column) → mark reachable cells
- From all **Atlantic border cells** (bottom row + right column) → mark reachable cells
- Answer = cells reachable by **both**

```cpp
class Solution {
public:
    vector<vector<int>> dir={{-1,0},{1,0},{0,-1},{0,1}};
    vector<vector<int>> pacificAtlantic(vector<vector<int>>& heights) {
        int row=heights.size(), col=heights[0].size();
        vector<vector<int>> ans;
        vector<vector<bool>> pacific(row,vector<bool>(col,false)),atlantic(row,vector<bool>(col,false));

        for(int r=0;r<row;r++){
            dfs(heights,pacific,r,0);
            dfs(heights,atlantic,r,col-1);
        }
        for(int c=0;c<col;c++){
            dfs(heights,pacific,0,c);
            dfs(heights,atlantic,row-1,c);
        }
        for(int i=0;i<row;i++){
            for(int j=0;j<col;j++){
                if(pacific[i][j] && atlantic[i][j])
                    ans.push_back({i,j});
            }
        }
        return ans;
    }

    void dfs(vector<vector<int>>& heights, vector<vector<bool>>& border, int r,int c){
        border[r][c]=true;
        for(auto it:dir){
            int newr=r+it[0];
            int newc=c+it[1];
            if(newr>=0 && newr<heights.size() && newc>=0 && newc<heights[0].size() &&
               !border[newr][newc] && heights[r][c]<=heights[newr][newc]){
                dfs(heights,border,newr,newc);
            }
        }
    }
};
```

---

## **3rd Solution – BFS (Graph)**

**Same intuition as DFS**, just implemented using BFS.

```cpp
class Solution {
public:
    vector<vector<int>> dir={{-1,0},{1,0},{0,-1},{0,1}};
    vector<vector<int>> pacificAtlantic(vector<vector<int>>& heights) {
        int row=heights.size(), col=heights[0].size();
        vector<vector<int>> ans;
        vector<vector<bool>> pacific(row,vector<bool>(col,false)),atlantic(row,vector<bool>(col,false));

        for(int r=0;r<row;r++){
            bfs(heights,pacific,r,0);
            bfs(heights,atlantic,r,col-1);
        }
        for(int c=0;c<col;c++){
            bfs(heights,pacific,0,c);
            bfs(heights,atlantic,row-1,c);
        }

        for(int i=0;i<row;i++){
            for(int j=0;j<col;j++){
                if(pacific[i][j] && atlantic[i][j])
                    ans.push_back({i,j});
            }
        }
        return ans;
    }

    void bfs(vector<vector<int>>& heights, vector<vector<bool>>& border,int init_r,int init_c){
        queue<pair<int,int>> q;
        q.push({init_r,init_c});
        border[init_r][init_c] = true;

        while(!q.empty()){
            int r=q.front().first;
            int c=q.front().second;
            q.pop();
            for(auto it:dir){
                int newr=r+it[0];
                int newc=c+it[1];
                if(newr>=0 && newr<heights.size() && newc>=0 && newc<heights[0].size() &&
                   !border[newr][newc] && heights[r][c]<=heights[newr][newc]){
                    border[newr][newc]=true;
                    q.push({newr,newc});
                }
            }
        }
    }
};
```

---

## **4th Implementation – DSU (Incorrect Approach)**

### **Attempted DSU Implementation (Wrong Answer)**

```cpp
// DSU Implementation
class DisjointSet{
    vector<int> rank,parent,size;
public:
    DisjointSet(int n){
        size.resize(n+1,1);
        parent.resize(n+1);
        for(int i=0;i<=n;i++) parent[i]=i;
    }
    int findUpar(int node){
        if(node==parent[node]) return node;
        return parent[node]=findUpar(parent[node]);
    }
    void unionBySize(int u,int v){
        int ulp_u=findUpar(u);
        int ulp_v=findUpar(v);
        if(ulp_u==ulp_v) return;
        if(size[ulp_u]>size[ulp_v]){
            parent[ulp_v]=ulp_u;
            size[ulp_u]+=size[ulp_v];
        }
        else{
            parent[ulp_u] =ulp_v;
            size[ulp_v]+=size[ulp_u];
        }
    }
};

class Solution {
public:
    vector<vector<int>> pacificAtlantic(vector<vector<int>>& heights) {
        int row=heights.size(),col=heights[0].size();
        int totalCells=row*col;
        vector<vector<int>> dir={{-1,0},{1,0},{0,-1},{0,1}};
        vector<vector<int>> ans;

        int pacRoot=totalCells;
        int atlRoot=totalCells+1;

        DisjointSet pacDSU(totalCells+1);
        DisjointSet atlDSU(totalCells+1);

        for(int r=0;r<row;r++){
            for(int c=0;c<col;c++){
                int curr=r*col+c;
                if(r==0 || c==0) pacDSU.unionBySize(curr,pacRoot);
                if(r==row-1 || c==col-1) atlDSU.unionBySize(curr,atlRoot);

                for(auto it:dir){
                    int newr=r+it[0];
                    int newc=c+it[1];
                    if(newr>=0 && newr<row && newc>=0 && newc<col && heights[r][c]<=heights[newr][newc]){
                        int newind=newr*col+newc;
                        pacDSU.unionBySize(curr, newind);
                        atlDSU.unionBySize(curr, newind);
                    }
                }
            }
        }

        for(int i=0;i<row;i++){
            for(int j=0;j<col;j++){
                int curr=i*col+j;
                if(pacDSU.findUpar(curr)==pacDSU.findUpar(pacRoot) &&
                   atlDSU.findUpar(curr) == atlDSU.findUpar(atlRoot))
                    ans.push_back({i,j});
            }
        }
        return ans;
    }
};
```

---

## **❌ Why DSU Breaks Conceptually Here**

- **DSU is undirected**
- **Loses direction information**
- **Cannot model reachability constraints properly**

In this problem:

- Water flow is **directed**
- DSU merges too aggressively
- Cells get connected even when water cannot actually flow

### **Result**

❌ Cells incorrectly appear reachable from both oceans

---

## **❌ Final Verdict: DSU Is the Wrong Tool**

Even if fixed:

- DSU cannot track reachability
- Only connectivity
- Edge cases still fail

### **This problem requires:**

- Graph traversal
- Direction-aware reachability
