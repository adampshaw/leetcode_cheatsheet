# ⚡ LeetCode Master Cheatsheet
> *The only sheet you review before an interview.*

---

## 🐍 1. Python Quick Reference

### Slicing
| Syntax | Meaning |
|---|---|
| `a[:]` | Full copy |
| `a[i:j]` | Index i to j-1 |
| `a[i:]` | From i to end |
| `a[:j]` | Start to j-1 |
| `a[-1]` | Last element |
| `a[:-1]` | All except last |
| `a[::-1]` | Reversed |
| `a[::2]` | Every 2nd element |
| `a[i:j:k]` | i to j-1, step k |

### Built-in Functions
```python
sorted(iterable, key=lambda x: x[1], reverse=True)
min(iterable), max(iterable), sum(iterable)
abs(x), divmod(a, b)  # → (quotient, remainder)
any(iterable), all(iterable)
enumerate(lst)         # → (index, value)
zip(a, b)              # → paired tuples (stops at shorter)
map(fn, iterable)
filter(fn, iterable)
chr(65) → 'A'  |  ord('A') → 65
bin(x), hex(x), int('1010', 2)  # base conversion
```

### Key Data Structures
```python
# LIST — O(1) append/pop, O(n) insert/search
lst = []; lst.append(x); lst.pop(); lst.pop(0)  # pop(0) is O(n)!

# DICT — O(1) avg all ops
d = {}; d.get(k, default); d.setdefault(k, []); d.items()
from collections import defaultdict
d = defaultdict(int)   # d[k] += 1 without KeyError
d = defaultdict(list)  # d[k].append(v)
Counter("aabbcc")      # {'a':2,'b':2,'c':2}
Counter.most_common(k) # top k elements

# SET — O(1) avg add/discard/in
s = set(); s.add(x); s.discard(x); s & s2; s | s2; s - s2

# DEQUE — O(1) both ends
from collections import deque
dq = deque(); dq.append(x); dq.appendleft(x)
dq.pop(); dq.popleft()

# HEAP — min-heap by default
import heapq
h = []; heapq.heappush(h, x); heapq.heappop(h)
heapq.heapify(lst)         # O(n) in-place
heapq.nlargest(k, lst)     # top-k
heapq.nsmallest(k, lst)
# MAX-HEAP trick: push negative values (-x)
# Custom: heappush(h, (priority, item))
```

### Common Idioms
```python
# List comprehension
[x**2 for x in range(10) if x % 2 == 0]

# Nested comprehension (matrix transpose)
[[row[i] for row in matrix] for i in range(len(matrix[0]))]

# Unpacking
a, *b, c = [1, 2, 3, 4, 5]  # a=1, b=[2,3,4], c=5
(a, b), c = (1, 2), 3

# Counter / frequency
from collections import Counter
freq = Counter(nums)

# Sorting tricks
intervals.sort(key=lambda x: x[0])
words.sort(key=lambda w: (-len(w), w))  # len desc, alpha asc

# Infinity
float('inf'), float('-inf')

# Integer division and modulo
a // b, a % b

# String ops
''.join(lst), s.split(), s.strip(), s.replace(a, b)
s.startswith(p), s.endswith(p), s.isdigit(), s.isalpha()
```

### Gotchas
```
❌ def foo(lst=[]):    →  mutable default — persists between calls
✅ def foo(lst=None): lst = lst or []

❌ b = a              →  shallow copy for lists/dicts
✅ b = a[:]           →  list copy
✅ b = a.copy()       →  dict copy
✅ import copy; b = copy.deepcopy(a)  # nested structures

❌ for i in lst: lst.remove(x)  → skip elements
✅ Iterate over a copy or use list comprehension

Integer overflow: Python has arbitrary precision — no need to worry.
String immutability: Build with list, then ''.join() at end.
None comparisons: Use `is None`, not `== None`.
```

---

## 🧠 2. Problem Pattern Recognition

| Keyword / Clue | Pattern | Notes |
|---|---|---|
| Shortest path, unweighted | BFS | Level-by-level |
| Shortest path, weighted | Dijkstra | Use min-heap |
| All paths, explore everything | DFS | Backtrack if needed |
| Subarray sum equals K | Prefix sum + HashMap | `prefix[i] - prefix[j] = k` |
| Sliding window (contiguous) | Two pointers / Window | Fixed vs variable size |
| Kth largest/smallest | Heap or Quickselect | Heap = O(n log k), QS = O(n) avg |
| Sorted array / search | Binary search | Mid = lo + (hi-lo)//2 |
| Rotate / circular | Modulo `% n` | Index wrap-around |
| Parentheses, valid brackets | Stack | Match pairs |
| Next greater/smaller element | Monotonic stack | Decreasing = next greater |
| Combinations / permutations | Backtracking | Prune early |
| Optimization (max/min of choices) | DP | Overlapping subproblems? |
| Greedy optimal local choice | Greedy | Prove exchange argument |
| Connected components | Union-Find or BFS/DFS | Graph connectivity |
| Dependency ordering | Topological sort | Kahn's or DFS |
| String matching | Sliding window / Two ptr | anagram, substring |
| Top K frequent | Bucket sort or Heap | Bucket = O(n) |
| Palindrome check | Two pointers | Expand from center |
| Tree path sum | DFS recursion | Pass running sum |
| Matrix traversal | BFS/DFS | Mark visited! |
| Intervals (overlap/merge) | Sort + sweep | Sort by start |
| Duplicate detection | HashSet | Or XOR, Floyd's |
| Linked list cycle | Fast & slow pointers | Floyd's algorithm |
| In-place rearrangement | Two pointers / index tricks | No extra space |
| Lexicographically smallest | Monotonic stack / Greedy | — |
| Count distinct subsequences | DP | Exponential state space |
| Game theory / optimal play | DP (minimax) | — |
| Random access + frequent update | Segment tree / BIT | — |

---

## 🔁 3. Core DSA Patterns

### Sliding Window
**Use:** Contiguous subarray/substring; optimize over a range.

```python
# Variable window — max window satisfying condition
left = 0
window_state = ...
for right in range(len(s)):
    # expand window: include s[right]
    update(window_state, s[right])
    while not valid(window_state):
        # shrink: remove s[left]
        update(window_state, s[left], remove=True)
        left += 1
    ans = max(ans, right - left + 1)

# Fixed window of size k
for right in range(len(arr)):
    if right >= k:
        remove arr[right - k] from window
    add arr[right] to window
    if right >= k - 1:
        update ans
```
**Complexity:** O(n) time, O(k) or O(alphabet) space.
**Pitfalls:** Forgetting to shrink; wrong condition for shrink; off-by-one in fixed window.

---

### Two Pointers
**Use:** Sorted arrays, pairs summing to target, palindrome, partition.

```python
# Pair sum in sorted array
lo, hi = 0, len(arr) - 1
while lo < hi:
    s = arr[lo] + arr[hi]
    if s == target: return [lo, hi]
    elif s < target: lo += 1
    else: hi -= 1

# Partition / overwrite in-place
write = 0
for read in range(len(arr)):
    if condition(arr[read]):
        arr[write] = arr[read]
        write += 1
```
**Complexity:** O(n) time, O(1) space.
**Pitfalls:** Must be sorted (or use other property); `lo < hi` not `lo <= hi` for pairs.

---

### Fast & Slow Pointers
**Use:** Cycle detection, middle of linked list, duplicate in array.

```python
slow = fast = head
while fast and fast.next:
    slow = slow.next
    fast = fast.next.next
    if slow == fast:  # cycle detected
        # find entry: reset slow to head
        slow = head
        while slow != fast:
            slow = slow.next
            fast = fast.next
        return slow  # cycle start
```
**Complexity:** O(n) time, O(1) space.
**Pitfalls:** Null check `fast and fast.next` before advancing fast.

---

### Binary Search
**Use:** Sorted array search; search-space minimization (answer is monotonic).

```python
# Standard: find exact target
lo, hi = 0, len(arr) - 1
while lo <= hi:
    mid = lo + (hi - lo) // 2
    if arr[mid] == target: return mid
    elif arr[mid] < target: lo = mid + 1
    else: hi = mid - 1

# Left bound: first position where arr[i] >= target
lo, hi = 0, len(arr)
while lo < hi:
    mid = (lo + hi) // 2
    if arr[mid] < target: lo = mid + 1
    else: hi = mid
return lo  # insertion point

# Right bound: last position where arr[i] <= target
lo, hi = 0, len(arr)
while lo < hi:
    mid = (lo + hi) // 2
    if arr[mid] <= target: lo = mid + 1
    else: hi = mid
return lo - 1

# Binary search on answer (e.g., "minimum max", "feasible capacity")
def feasible(mid): ...
lo, hi = min_val, max_val
while lo < hi:
    mid = (lo + hi) // 2
    if feasible(mid): hi = mid
    else: lo = mid + 1
return lo
```
**Complexity:** O(log n) time, O(1) space.
**Pitfalls:** `lo + (hi - lo)//2` prevents overflow (not needed in Python but good habit); wrong loop condition (`<=` vs `<`) depending on variant.

---

### DFS / BFS

```python
# BFS — level-order, shortest path (unweighted)
from collections import deque
def bfs(start, target, graph):
    queue = deque([start])
    visited = {start}
    dist = 0
    while queue:
        for _ in range(len(queue)):  # process level
            node = queue.popleft()
            if node == target: return dist
            for nei in graph[node]:
                if nei not in visited:
                    visited.add(nei)
                    queue.append(nei)
        dist += 1

# DFS — recursive
def dfs(node, visited, graph):
    visited.add(node)
    for nei in graph[node]:
        if nei not in visited:
            dfs(nei, visited, graph)

# DFS — iterative
stack = [start]; visited = {start}
while stack:
    node = stack.pop()
    for nei in graph[node]:
        if nei not in visited:
            visited.add(nei)
            stack.append(nei)

# Tree DFS (no visited needed)
def dfs(root):
    if not root: return base_val
    left = dfs(root.left)
    right = dfs(root.right)
    return combine(left, right, root.val)
```
**BFS:** Shortest path, level-order, multi-source expansion.
**DFS:** All paths, topological, connectivity, subtree properties.
**Pitfalls:** Forgetting `visited` in graphs; BFS uses queue, DFS uses stack/recursion.

---

### Backtracking
**Use:** All combinations/permutations/subsets; constraint satisfaction.

```python
def backtrack(start, path, result):
    if is_complete(path):
        result.append(path[:])
        return
    for i in range(start, len(candidates)):
        if should_prune(i, path): continue
        path.append(candidates[i])
        backtrack(i + 1, path, result)  # i+1 = no reuse; i = reuse
        path.pop()

result = []
backtrack(0, [], result)
```
**Complexity:** O(2^n) or O(n!) depending on problem.
**Pitfalls:** Append a copy `path[:]` not `path`; prune early to avoid TLE; duplicates → sort + skip `if i > start and candidates[i] == candidates[i-1]`.

---

### Dynamic Programming

```python
# 1D DP — Fibonacci style
dp = [0] * (n + 1)
dp[0], dp[1] = base0, base1
for i in range(2, n + 1):
    dp[i] = dp[i-1] + dp[i-2]  # recurrence

# 2D DP — Grid / String
dp = [[0] * (m+1) for _ in range(n+1)]
for i in range(1, n+1):
    for j in range(1, m+1):
        if condition: dp[i][j] = dp[i-1][j-1] + 1
        else: dp[i][j] = max(dp[i-1][j], dp[i][j-1])

# Knapsack (0/1)
dp = [0] * (W + 1)
for weight, value in items:
    for w in range(W, weight - 1, -1):  # REVERSE to avoid reuse
        dp[w] = max(dp[w], dp[w - weight] + value)

# LIS (Longest Increasing Subsequence)
# O(n^2):
dp = [1] * n
for i in range(n):
    for j in range(i):
        if nums[j] < nums[i]:
            dp[i] = max(dp[i], dp[j] + 1)
# O(n log n): patience sort with bisect

# Memoization (top-down)
from functools import lru_cache
@lru_cache(maxsize=None)
def dp(i, j, ...):
    if base_case: return val
    return recurrence(dp(i-1,...), dp(i,j-1,...))
```
**DP decision tree:**
- Choices at each step? → DP
- Overlapping subproblems? → Memoize
- Order matters? → 2D DP on sequence
- Choices are reusable? → Unbounded knapsack (forward loop)
- Choices NOT reusable? → 0/1 knapsack (reverse loop)

**Pitfalls:** Wrong base case; wrong iteration order; confusing 0/1 vs unbounded knapsack direction.

---

### Greedy
**Use:** Local optimal → global optimal. Scheduling, intervals, min cost.

```
Proof strategy: Exchange argument — swapping two adjacent elements can't improve.
Common patterns:
  - Sort by end time → max non-overlapping intervals
  - Always pick smallest/largest greedily (Huffman, MST)
  - "Jump game" → track max reach
```
**Pitfalls:** Greedy doesn't always work — verify with exchange argument or small counterexample.

---

### Union-Find (Disjoint Set)
**Use:** Connected components, detect cycle in undirected graph, grouping.

```python
class UnionFind:
    def __init__(self, n):
        self.parent = list(range(n))
        self.rank = [0] * n
    
    def find(self, x):
        if self.parent[x] != x:
            self.parent[x] = self.find(self.parent[x])  # path compression
        return self.parent[x]
    
    def union(self, x, y):
        px, py = self.find(x), self.find(y)
        if px == py: return False  # already connected
        if self.rank[px] < self.rank[py]: px, py = py, px
        self.parent[py] = px
        if self.rank[px] == self.rank[py]: self.rank[px] += 1
        return True
```
**Complexity:** O(α(n)) ≈ O(1) per operation with compression + rank.
**Pitfalls:** Forgetting path compression; misidentifying directed vs undirected.

---

### Topological Sort
**Use:** Dependency ordering, course scheduling, build systems.

```python
# Kahn's Algorithm (BFS-based)
from collections import deque, defaultdict
def topo_sort(n, edges):
    graph = defaultdict(list)
    indegree = [0] * n
    for u, v in edges:
        graph[u].append(v)
        indegree[v] += 1
    
    queue = deque([i for i in range(n) if indegree[i] == 0])
    order = []
    while queue:
        node = queue.popleft()
        order.append(node)
        for nei in graph[node]:
            indegree[nei] -= 1
            if indegree[nei] == 0:
                queue.append(nei)
    
    return order if len(order) == n else []  # [] = cycle exists
```
**Pitfalls:** Cycle = not all nodes in result. Use `len(order) == n` to detect.

---

### Monotonic Stack / Queue
**Use:** Next greater/smaller element, largest rectangle, sliding window max.

```python
# Monotonic decreasing stack → next greater element
stack = []  # stores indices
result = [-1] * len(nums)
for i, num in enumerate(nums):
    while stack and nums[stack[-1]] < num:
        result[stack.pop()] = num
    stack.append(i)

# Monotonic queue → sliding window max (deque of indices)
from collections import deque
dq = deque()  # decreasing, stores indices
for i in range(len(nums)):
    # remove elements outside window
    while dq and dq[0] < i - k + 1:
        dq.popleft()
    # remove smaller elements from back
    while dq and nums[dq[-1]] < nums[i]:
        dq.pop()
    dq.append(i)
    if i >= k - 1:
        result.append(nums[dq[0]])
```
**Complexity:** O(n) amortized — each element pushed/popped once.
**Pitfalls:** Deciding stack direction (increasing vs decreasing) based on what you're finding; storing indices not values when you need window bounds.

---

### Heap / Priority Queue
**Use:** Kth element, merge k sorted, task scheduling, Dijkstra.

```python
import heapq
# Min-heap
heapq.heappush(h, x)
heapq.heappop(h)    # returns min
h[0]                # peek min

# Max-heap: negate values
heapq.heappush(h, -x)
-heapq.heappop(h)

# Fixed-size max-heap → Kth smallest
h = []
for num in nums:
    heapq.heappush(h, -num)
    if len(h) > k:
        heapq.heappop(h)
return -h[0]  # Kth smallest = min of k largest

# Dijkstra
dist = {node: float('inf') for node in graph}
dist[src] = 0
heap = [(0, src)]
while heap:
    d, u = heapq.heappop(heap)
    if d > dist[u]: continue
    for v, w in graph[u]:
        if dist[u] + w < dist[v]:
            dist[v] = dist[u] + w
            heapq.heappush(heap, (dist[v], v))
```
**Complexity:** Push/pop O(log n), peek O(1), heapify O(n).

---

## ⏱️ 4. Time & Space Complexity

### Common Operations
| Operation | Array | Dict/Set | Heap | Sorted Array |
|---|---|---|---|---|
| Access by index | O(1) | — | O(1) peek | O(1) |
| Search | O(n) | O(1) avg | O(n) | O(log n) |
| Insert (end) | O(1) amort | O(1) avg | O(log n) | O(n) |
| Insert (middle) | O(n) | — | — | O(n) |
| Delete | O(n) | O(1) avg | O(log n) | O(n) |
| Min/Max | O(n) | — | O(1) | O(1) |

### Growth Rates (Intuition)
| Complexity | n=10 | n=100 | n=1000 | n=10^6 | Viable? |
|---|---|---|---|---|---|
| O(1) | 1 | 1 | 1 | 1 | ✅ Always |
| O(log n) | 3 | 7 | 10 | 20 | ✅ Always |
| O(n) | 10 | 100 | 1K | 1M | ✅ |
| O(n log n) | 33 | 664 | 10K | 20M | ✅ Usually |
| O(n²) | 100 | 10K | 1M | 10^12 | ⚠️ n ≤ 10^4 |
| O(n³) | 1K | 1M | 10^9 | — | ⚠️ n ≤ 500 |
| O(2^n) | 1024 | 10^30 | — | — | ⚠️ n ≤ 20 |
| O(n!) | 3.6M | — | — | — | ⚠️ n ≤ 10 |

**Interview thumb rules:**
- n ≤ 10^6 → O(n) or O(n log n)
- n ≤ 10^4 → O(n²) is ok
- n ≤ 20 → exponential is acceptable
- Multiple passes of O(n) = O(n), not O(kn)

---

## 🧩 5. Common Problem Archetypes

| Archetype | Approach(es) | Key Insight |
|---|---|---|
| Find duplicates | HashSet / XOR / Floyd's | XOR: appears odd times; Floyd's: O(1) space |
| Kth largest | Max-heap of size k / Quickselect | Heap = O(n log k), QS = O(n) avg |
| Anagram / permutation | Counter / sorted / freq array | freq array faster for lowercase alpha |
| Palindrome | Two ptrs from center / DP | Expand from center O(n²) no extra space |
| Two sum variants | HashMap / Two ptrs (sorted) | Unsorted → hashmap; sorted → two ptrs |
| Subarray with property | Prefix sum + HashMap | `sum(i..j) = prefix[j] - prefix[i-1]` |
| Interval overlap | Sort by start, check end | Greedy merge or count overlaps |
| String parsing | Stack / State machine | Nested structures → stack |
| Graph connectivity | BFS/DFS / Union-Find | UF faster for dynamic edge addition |
| Cycle detection | Floyd's / DFS with colors | Colors: WHITE/GRAY/BLACK for directed |
| Serialize/deserialize | BFS level-order / DFS | DFS with sentinel for nulls |
| LRU Cache | OrderedDict / Doubly-linked + HashMap | O(1) all ops |
| Task scheduling | Greedy sort / Heap | Cooling: max count formula |
| Buy/sell stock | Track min so far / DP states | States: hold, sold, rest |
| Coin change | DP unbounded knapsack | Forward iteration |
| Word break | DP / BFS with Trie | Trie speeds up prefix lookup |
| Matrix rotation | Transpose + reverse / ring swap | 90° = transpose → reverse rows |
| Jump game | Greedy (max reach) | Is current index ≤ max reach? |

---

## 🧱 6. Templates to Memorize

### BFS
```python
from collections import deque
def bfs(root):
    if not root: return []
    queue = deque([root])
    result = []
    while queue:
        level_size = len(queue)
        level = []
        for _ in range(level_size):
            node = queue.popleft()
            level.append(node.val)
            if node.left: queue.append(node.left)
            if node.right: queue.append(node.right)
        result.append(level)
    return result
```

### DFS (Tree)
```python
def dfs(node, memo={}):
    if not node: return 0
    if node in memo: return memo[node]
    res = compute(dfs(node.left), dfs(node.right), node.val)
    memo[node] = res
    return res
```

### Binary Search (Left Bound)
```python
def left_bound(arr, target):
    lo, hi = 0, len(arr)
    while lo < hi:
        mid = (lo + hi) // 2
        if arr[mid] < target: lo = mid + 1
        else: hi = mid
    return lo  # first index where arr[lo] >= target
```

### Sliding Window
```python
from collections import defaultdict
def sliding_window(s, k):
    counts = defaultdict(int)
    left = res = 0
    for right in range(len(s)):
        counts[s[right]] += 1
        while not_valid(counts, k):
            counts[s[left]] -= 1
            if counts[s[left]] == 0: del counts[s[left]]
            left += 1
        res = max(res, right - left + 1)
    return res
```

### Backtracking
```python
def solve(candidates):
    candidates.sort()
    res = []
    def bt(start, path):
        res.append(path[:])  # or check completion condition
        for i in range(start, len(candidates)):
            if i > start and candidates[i] == candidates[i-1]: continue  # skip dups
            if candidates[i] > remaining: break  # prune
            path.append(candidates[i])
            bt(i + 1, path)
            path.pop()
    bt(0, [])
    return res
```

### DP with Memoization
```python
from functools import lru_cache
def solve(n):
    @lru_cache(maxsize=None)
    def dp(i, j):
        if base_case(i, j): return base_value
        return max(dp(i-1, j), dp(i, j-1)) + cost[i][j]
    return dp(n-1, m-1)
```

### Prefix Sum
```python
def subarray_sum_k(nums, k):
    count = 0
    prefix = 0
    seen = defaultdict(int)
    seen[0] = 1  # empty prefix
    for num in nums:
        prefix += num
        count += seen[prefix - k]
        seen[prefix] += 1
    return count
```

---

## ⚠️ 7. Common Mistakes & Traps

### Off-By-One
```
Binary search: lo <= hi vs lo < hi (know which you're using!)
Sliding window: right - left + 1 is window size
Array bounds: range(n) vs range(n+1) in DP
Substring: s[i:j] excludes j — length is j-i
```

### Edge Cases Not Handled
```
Empty input → check before indexing
Single element → often special-cases the loop
Integer overflow → Python immune, but think in other languages
Negative numbers → affect sum comparisons, min/max logic
Duplicate values → affect binary search, heap, backtracking
Disconnected graph → BFS/DFS must iterate all nodes
```

### Wrong Data Structure
```
Need O(1) lookup → use set/dict, not list
Need ordering + lookup → use sortedcontainers (SortedList)
Need FIFO → use deque, not list (pop(0) = O(n))
Need Kth element repeatedly → heap, not re-sorting
Need prefix queries → prefix sum or BIT, not O(n) each time
```

### Overcomplicating
```
Reach for simple first:
  Two passes > one complex pass if same complexity
  Brute force → then optimize
  If n ≤ 1000, O(n²) is fine — don't over-engineer
  Python's sorted() is O(n log n) and often good enough
```

### DP-Specific
```
Forgetting to copy dp row before updating (2D rolling array)
Wrong order: 0/1 knapsack must go RIGHT-TO-LEFT
Memoizing mutable arguments → convert to tuple
Not including 0 in dp initialization (off-by-one in dp size)
```

---

## 🚀 8. Interview Strategy

### Live Problem Solving (Step-by-Step)
```
1. READ CAREFULLY (2 min)
   - Restate in your own words
   - Identify: input type, output type, constraints

2. EXAMPLES & EDGE CASES (2 min)
   - Walk through provided example manually
   - Think: empty? single? duplicates? negatives?

3. PATTERN RECOGNITION (1-2 min)
   - What keywords do you see? (→ see Section 2 table)
   - What data structure is natural here?
   - Have you seen a similar problem?

4. BRUTE FORCE FIRST (1 min)
   - State the naive solution verbally
   - Gives you a correctness baseline and interviewer signal

5. OPTIMIZE (3-5 min)
   - Bottleneck: where is the O(n²)?
   - Can you precompute? Cache? Sort?
   - Think: is there monotonicity to exploit? (binary search / greedy)

6. CODE (10-15 min)
   - Write clean, readable code
   - Use helper functions for clarity
   - Narrate what you're doing

7. TEST (3-5 min)
   - Trace through your example
   - Test edge cases manually
   - State time/space complexity
```

### Pattern Recognition Speed Tips
```
- Scan for: "minimum", "maximum", "count", "all", "exists"
- Count constraints: n ≤ 20 → exponential ok; n ≤ 10^6 → must be O(n)
- "At most K" / "exactly K" → sliding window
- "All subsets" / "all permutations" → backtracking
- Sorted input → binary search or two pointers
- Tree problem → think recursively (subtree return values)
- Graph problem → think visited set + BFS or DFS
```

### When Stuck
```
1. Re-read the problem — often missed constraint
2. Draw it out — visualize for arrays, trees, graphs
3. Try small example by hand
4. Relax a constraint — what if array was sorted?
5. Think: what information do I need at each step?
6. Can I solve a simpler subproblem?
7. State your confusion aloud — interviewer may hint
```

### Communication
```
✅ "I'm going to start with a brute force approach..."
✅ "I notice the array is sorted, so binary search might apply..."
✅ "Let me trace through this example to verify..."
✅ "The time complexity here is O(n log n) because..."
✅ "One edge case I should handle is..."
❌ Long silence without talking
❌ Jumping to code without explanation
❌ "I don't know" without attempting to reason
```

---

## 🧪 9. Edge Case Checklist

Before coding, ask yourself:
```
□ Empty input ([], "", None, n=0)
□ Single element
□ Two elements (min case for two-pointer logic)
□ All same elements
□ Already sorted / reverse sorted
□ Duplicates
□ Negative numbers / zero
□ Large n (will O(n²) TLE?)
□ Integer extremes (INT_MIN / INT_MAX — less issue in Python)
□ Cycles in graph
□ Disconnected graph (BFS/DFS might miss nodes)
□ Float precision (usually avoid floats in LC)
□ Overflow in intermediate computation
□ Empty string / whitespace-only string
□ Null / None in tree nodes
```

---

## 🔥 10. Advanced Insights

### When Patterns Overlap
```
Sliding window + HashMap → substring/subarray with constraints (freq map)
BFS + DP → shortest path with states (dp[node][state])
Binary search + Greedy → "minimize the maximum" problems
DFS + Memoization → DP on trees
Heap + Greedy → interval scheduling, task assignment
Two pointers + Sorting → 3Sum, k-Sum variants
```

### Heap vs Quickselect
```
Heap (k elements):
  - O(n log k) time, O(k) space
  - Stable, works for streaming data
  - Always correct

Quickselect:
  - O(n) avg, O(n²) worst
  - In-place, O(1) extra space
  - Use when: static array, one-time query, n is large
  - Random pivot minimizes worst case
```

### BFS vs DFS
```
BFS:
  - Guaranteed shortest path (unweighted)
  - Uses more memory (queue can hold entire level)
  - Better for: shortest path, level-order, closest target

DFS:
  - Lower memory (stack depth = height)
  - Better for: all paths, topological, strongly connected
  - Can get stuck in infinite paths without visited set
```

### Disguised DP Problems
```
"Can we reach...?" → reachability DP
"Min cost to reach..." → cost DP
"Number of ways..." → counting DP
"Longest/shortest subsequence..." → sequence DP
"Partition into groups..." → subset DP
"String matching with wildcards..." → 2D string DP

Tip: If a recursive solution with memoization works, it IS DP.
```

### Optimization Tricks
```
Rolling array: Reduce 2D DP to 1D if only prev row needed
Early termination: return immediately on found/invalid
Pre-sort: O(n log n) upfront enables O(n) linear scan
Bit manipulation: Subset enumeration 2^n with bits
String deduplication: Use tuple(sorted(word)) as dict key
Coordinate compression: Map values to 0..n for sparse arrays
Two-pass: L→R then R→L for "see both sides" problems (e.g. product except self)
```

### Key Recurrences to Know by Heart
```
Fibonacci: dp[i] = dp[i-1] + dp[i-2]
Climbing stairs: same as Fibonacci
Coin change (min): dp[i] = min(dp[i - coin] + 1) for coin in coins
Knapsack (0/1): dp[w] = max(dp[w], dp[w-wt] + val) [reverse loop]
LCS: dp[i][j] = dp[i-1][j-1]+1 if match else max(dp[i-1][j], dp[i][j-1])
Edit distance: dp[i][j] = dp[i-1][j-1] if match else 1 + min(dp[i-1][j], dp[i][j-1], dp[i-1][j-1])
Matrix chain: dp[i][j] = min(dp[i][k] + dp[k+1][j] + cost) for k in [i,j-1]
Unique paths: dp[i][j] = dp[i-1][j] + dp[i][j-1]
```

### The 80/20 of LeetCode Topics
```
Master these → cover ~80% of interview problems:
  1. Arrays + HashMaps (Two Sum, subarray problems)
  2. Sliding Window (substring, subarray)
  3. Binary Search (including on answer)
  4. DFS/BFS on trees and graphs
  5. 1D and 2D DP (top-down with memo)
  6. Backtracking (subsets, permutations, combinations)
  7. Heap (top-K, merge K, Dijkstra)
  8. Two Pointers (sorted arrays, linked lists)
```

---

*Built for speed. Reviewed before every interview. Good luck. ⚡*
