# Leetcode-cheat-sheet
**Sliding Window & Two Pointers**

1. **Fixed-Length Sliding Window**
   LeetCode 3. Longest Substring Without Repeating Characters

   ```js
   function lengthOfLongestSubstring(s) {
     const last = new Map();
     let start = 0, maxLen = 0;
     for (let i = 0; i < s.length; i++) {
       if (last.has(s[i])) {
         start = Math.max(start, last.get(s[i]) + 1);
       }
       last.set(s[i], i);
       maxLen = Math.max(maxLen, i - start + 1);
     }
     return maxLen;
   }
   ```

   Representative: 3. Longest Substring Without Repeating Characters; 76. Minimum Window Substring

2. **Variable-Length Sliding Window**
   LeetCode 209. Minimum Size Subarray Sum

   ```js
   function minSubArrayLen(target, nums) {
     let left = 0, sum = 0, res = Infinity;
     for (let right = 0; right < nums.length; right++) {
       sum += nums[right];
       while (sum >= target) {
         res = Math.min(res, right - left + 1);
         sum -= nums[left++];
       }
     }
     return res === Infinity ? 0 : res;
   }
   ```

   Representative: 209. Minimum Size Subarray Sum

3. **Two Sequences / Two Pointers**
   LeetCode 11. Container With Most Water

   ```js
   function maxArea(height) {
     let i = 0, j = height.length - 1, maxA = 0;
     while (i < j) {
       maxA = Math.max(maxA, Math.min(height[i], height[j]) * (j - i));
       if (height[i] < height[j]) i++;
       else j--;
     }
     return maxA;
   }
   ```

   Representative: 11. Container With Most Water; 88. Merge Sorted Array

---

**Binary Search**

1. **Basic Template**
   LeetCode 34. Find First and Last Position of Element in Sorted Array

   ```js
   function binarySearch(arr, target) {
     let left = 0, right = arr.length - 1;
     while (left <= right) {
       let mid = Math.floor((left + right) / 2);
       if (arr[mid] === target) return mid;
       if (arr[mid] < target) left = mid + 1;
       else right = mid - 1;
     }
     return -1;
   }
   ```

2. **Search on Answer (Minimize/Maximize)**
   LeetCode 410. Split Array Largest Sum

   ```js
   function splitArray(nums, m) {
     const can = (maxSum) => {
       let count = 1, total = 0;
       for (let n of nums) {
         if (total + n > maxSum) {
           count++;
           total = 0;
         }
         total += n;
       }
       return count <= m;
     };

     let l = Math.max(...nums), r = nums.reduce((a,b)=>a+b);
     while (l < r) {
       let mid = Math.floor((l + r) / 2);
       if (can(mid)) r = mid;
       else l = mid + 1;
     }
     return l;
   }
   ```

   Representative: 410. Split Array Largest Sum; 875. Koko Eating Bananas

---

**Monotonic Stack**

1. **Next Greater Element / Largest Rectangle**
   LeetCode 84. Largest Rectangle in Histogram

   ```js
   function largestRectangleArea(heights) {
     const stack = [-1];
     let maxA = 0;
     for (let i = 0; i < heights.length; i++) {
       while (stack.length > 1 && heights[i] < heights[stack[stack.length - 1]]) {
         const h = heights[stack.pop()];
         const w = i - stack[stack.length - 1] - 1;
         maxA = Math.max(maxA, h * w);
       }
       stack.push(i);
     }
     while (stack.length > 1) {
       const h = heights[stack.pop()];
       const w = heights.length - stack[stack.length - 1] - 1;
       maxA = Math.max(maxA, h * w);
     }
     return maxA;
   }
   ```

   Representative: 84. Largest Rectangle in Histogram; 739. Daily Temperatures

---

**Grid-Based Problems (DFS / BFS)**

1. **Shortest Path in Grid (BFS)**
   LeetCode 994. Rotting Oranges

   ```js
   function orangesRotting(grid) {
     const m = grid.length, n = grid[0].length;
     const dirs = [[1,0],[-1,0],[0,1],[0,-1]];
     const q = [], fresh = { cnt: 0 };
     for (let i = 0; i < m; i++) {
       for (let j = 0; j < n; j++) {
         if (grid[i][j] === 2) q.push([i,j,0]);
         if (grid[i][j] === 1) fresh.cnt++;
       }
     }
     let time = 0;
     while (q.length && fresh.cnt) {
       const [x,y,d] = q.shift();
       for (let [dx,dy] of dirs) {
         const nx = x+dx, ny = y+dy;
         if (nx>=0 && ny>=0 && nx<m && ny<n && grid[nx][ny]===1) {
           grid[nx][ny] = 2;
           fresh.cnt--;
           q.push([nx,ny,d+1]);
           time = d+1;
         }
       }
     }
     return fresh.cnt === 0 ? time : -1;
   }
   ```

   Representative: 994. Rotting Oranges; 79. Word Search

---

**Bit Manipulation**

1. **XOR for Single Number**
   LeetCode 136. Single Number

   ```js
   function singleNumber(nums) {
     return nums.reduce((acc, x) => acc ^ x, 0);
   }
   ```

   Representative: 136. Single Number; 190. Reverse Bits

---

**Graph Algorithms**

1. **Topological Sort (Kahn’s Algorithm)**
   LeetCode 207. Course Schedule

   ```js
   function canFinish(numCourses, prerequisites) {
     const graph = Array.from({length: numCourses}, () => []);
     const inDeg = Array(numCourses).fill(0);
     for (let [u,v] of prerequisites) {
       graph[v].push(u);
       inDeg[u]++;
     }
     const q = [];
     inDeg.forEach((d,i) => d===0 && q.push(i));
     let seen = 0;
     while (q.length) {
       const u = q.shift();
       seen++;
       for (let v of graph[u]) {
         if (--inDeg[v] === 0) q.push(v);
       }
     }
     return seen === numCourses;
   }
   ```

   Representative: 207. Course Schedule; 210. Course Schedule II

---

**Dynamic Programming**

1. **0/1 Knapsack (Subset Sum)**
   LeetCode 416. Partition Equal Subset Sum

   ```js
   function canPartition(nums) {
     const sum = nums.reduce((a,b)=>a+b), target = sum/2;
     if (sum % 2) return false;
     const dp = Array(target+1).fill(false);
     dp[0] = true;
     for (let num of nums) {
       for (let j = target; j >= num; j--) {
         dp[j] = dp[j] || dp[j-num];
       }
     }
     return dp[target];
   }
   ```

   Representative: 416. Partition Equal Subset Sum; 139. Word Break

---

**Common Data Structures**

1. **Prefix Sum**
   LeetCode 303. Range Sum Query – Immutable

   ```js
   function buildPrefixSum(nums) {
     const pre = [0];
     for (let x of nums) pre.push(pre[pre.length-1] + x);
     return pre;
   }
   ```

2. **Union-Find**
   LeetCode 547. Number of Provinces

   ```js
   class UF {
     constructor(n) {
       this.p = Array.from({length:n}, (_,i)=>i);
     }
     find(x) { return this.p[x]===x ? x : (this.p[x]=this.find(this.p[x])); }
     union(a,b) { this.p[this.find(a)] = this.find(b); }
   }
   function findCircleNum(isConnected) {
     const n = isConnected.length, uf = new UF(n);
     for (let i=0;i<n;i++)
       for (let j=0;j<n;j++)
         if (isConnected[i][j]) uf.union(i,j);
     return new Set(uf.p.map((_,i)=>uf.find(i))).size;
   }
   ```

---

**Mathematical Algorithms**

1. **Fast Power**
   LeetCode 50. Pow(x, n)

   ```js
   function myPow(x, n) {
     if (n === 0) return 1;
     let half = myPow(x, Math.floor(n/2));
     return n % 2 === 0 ? half * half : half * half * x;
   }
   ```

   Representative: 50. Pow(x, n); 60. Permutation Sequence

---

**Greedy & Problem Solving**

1. **Interval Scheduling**
   LeetCode 435. Non-overlapping Intervals

   ```js
   function eraseOverlapIntervals(intervals) {
     intervals.sort((a,b)=>a[1]-b[1]);
     let end = -Infinity, cnt = 0;
     for (let [s,e] of intervals) {
       if (s >= end) end = e;
       else cnt++;
     }
     return cnt;
   }
   ```

   Representative: 435. Non-overlapping Intervals

---

**Linked List, Trees & Backtracking**

1. **Backtracking Subsets**
   LeetCode 78. Subsets

   ```js
   function subsets(nums) {
     const res = [];
     function dfs(start, path) {
       res.push([...path]);
       for (let i = start; i < nums.length; i++) {
         path.push(nums[i]);
         dfs(i + 1, path);
         path.pop();
       }
     }
     dfs(0, []);
     return res;
   }
   ```

   Representative: 78. Subsets; 236. Lowest Common Ancestor of a Binary Tree

---

**String Algorithms**

1. **KMP String Matching**
   LeetCode 28. Implement strStr()

   ```js
   function buildKMP(p) {
     const next = Array(p.length).fill(0);
     let j = 0;
     for (let i = 1; i < p.length; i++) {
       while (j && p[i] !== p[j]) j = next[j-1];
       if (p[i] === p[j]) j++;
       next[i] = j;
     }
     return next;
   }
   function strStr(h, n) {
     if (!n) return 0;
     const next = buildKMP(n);
     let j = 0;
     for (let i = 0; i < h.length; i++) {
       while (j && h[i] !== n[j]) j = next[j-1];
       if (h[i] === n[j]) j++;
       if (j === n.length) return i - j + 1;
     }
     return -1;
   }
   ```

   Representative: 28. Implement strStr(); 459. Repeated Substring Pattern



