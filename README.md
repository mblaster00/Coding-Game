# Coding-Game

# HackerRank Medium & Hard Problems - Complete Guide
## AWS Online Assessment Preparation

---

## Table of Contents
1. [Essential Data Structures](#essential-data-structures)
2. [Core Algorithms](#core-algorithms)
3. [Advanced Problem-Solving Patterns](#advanced-problem-solving-patterns)
4. [Time & Space Complexity Analysis](#time--space-complexity-analysis)
5. [Common Medium Problems & Solutions](#common-medium-problems--solutions)
6. [Common Hard Problems & Solutions](#common-hard-problems--solutions)
7. [AWS-Specific Problem Types](#aws-specific-problem-types)
8. [Optimization Techniques](#optimization-techniques)
9. [Debugging & Testing Strategies](#debugging--testing-strategies)
10. [Practice Roadmap](#practice-roadmap)

---

## Essential Data Structures

### 1. Arrays and Strings
**Key Concepts:**
- Two-pointer technique
- Sliding window
- Prefix sums
- Kadane's algorithm

**Example Problem: Maximum Subarray Sum**
```python
def max_subarray_sum(arr):
    max_sum = current_sum = arr[0]
    for i in range(1, len(arr)):
        current_sum = max(arr[i], current_sum + arr[i])
        max_sum = max(max_sum, current_sum)
    return max_sum
```

### 2. Hash Maps and Sets
**Key Applications:**
- Frequency counting
- Fast lookups
- Caching intermediate results
- Finding complements

**Example Problem: Two Sum Variants**
```python
def two_sum_all_pairs(nums, target):
    seen = {}
    pairs = []
    for i, num in enumerate(nums):
        complement = target - num
        if complement in seen:
            pairs.append([seen[complement], i])
        seen[num] = i
    return pairs
```

### 3. Stacks and Queues
**Key Patterns:**
- Monotonic stack for next/previous greater elements
- Queue for BFS
- Stack for DFS and expression evaluation

**Example Problem: Largest Rectangle in Histogram**
```python
def largest_rectangle_area(heights):
    stack = []
    max_area = 0
    
    for i, h in enumerate(heights):
        while stack and heights[stack[-1]] > h:
            height = heights[stack.pop()]
            width = i if not stack else i - stack[-1] - 1
            max_area = max(max_area, height * width)
        stack.append(i)
    
    while stack:
        height = heights[stack.pop()]
        width = len(heights) if not stack else len(heights) - stack[-1] - 1
        max_area = max(max_area, height * width)
    
    return max_area
```

### 4. Trees and Graphs
**Essential Algorithms:**
- DFS (recursive and iterative)
- BFS
- Tree traversals (inorder, preorder, postorder)
- Dijkstra's algorithm
- Union-Find (Disjoint Set Union)

**Example Problem: Lowest Common Ancestor**
```python
def lowest_common_ancestor(root, p, q):
    if not root or root == p or root == q:
        return root
    
    left = lowest_common_ancestor(root.left, p, q)
    right = lowest_common_ancestor(root.right, p, q)
    
    if left and right:
        return root
    return left or right
```

---

## Core Algorithms

### 1. Dynamic Programming
**Key Patterns:**
- 1D DP (Fibonacci-like)
- 2D DP (Grid problems)
- Knapsack variants
- LIS (Longest Increasing Subsequence)
- Edit distance

**Example Problem: Coin Change**
```python
def coin_change(coins, amount):
    dp = [float('inf')] * (amount + 1)
    dp[0] = 0
    
    for coin in coins:
        for i in range(coin, amount + 1):
            dp[i] = min(dp[i], dp[i - coin] + 1)
    
    return dp[amount] if dp[amount] != float('inf') else -1
```

### 2. Binary Search
**Applications:**
- Search in sorted arrays
- Finding boundaries
- Search in answer space
- Peak finding

**Example Problem: Search in Rotated Sorted Array**
```python
def search_rotated_array(nums, target):
    left, right = 0, len(nums) - 1
    
    while left <= right:
        mid = (left + right) // 2
        
        if nums[mid] == target:
            return mid
        
        # Left half is sorted
        if nums[left] <= nums[mid]:
            if nums[left] <= target < nums[mid]:
                right = mid - 1
            else:
                left = mid + 1
        # Right half is sorted
        else:
            if nums[mid] < target <= nums[right]:
                left = mid + 1
            else:
                right = mid - 1
    
    return -1
```

### 3. Sorting Algorithms
**Key Algorithms:**
- Merge Sort (O(n log n) guaranteed)
- Quick Sort (average O(n log n))
- Heap Sort
- Counting Sort (for limited range)

**Custom Sorting Example:**
```python
def custom_sort_intervals(intervals):
    # Sort by start time, then by end time
    return sorted(intervals, key=lambda x: (x[0], x[1]))
```

---

## Advanced Problem-Solving Patterns

### 1. Backtracking
**Template:**
```python
def backtrack(path, choices):
    if is_valid_solution(path):
        result.append(path[:])  # Make a copy
        return
    
    for choice in choices:
        if is_valid_choice(choice, path):
            path.append(choice)
            backtrack(path, get_next_choices(choice))
            path.pop()  # Backtrack
```

**Example Problem: N-Queens**
```python
def solve_n_queens(n):
    def is_safe(board, row, col):
        for i in range(row):
            if board[i] == col or \
               board[i] - i == col - row or \
               board[i] + i == col + row:
                return False
        return True
    
    def backtrack(board, row):
        if row == n:
            result.append(['.' * i + 'Q' + '.' * (n - i - 1) for i in board])
            return
        
        for col in range(n):
            if is_safe(board, row, col):
                board.append(col)
                backtrack(board, row + 1)
                board.pop()
    
    result = []
    backtrack([], 0)
    return result
```

### 2. Greedy Algorithms
**Key Principle:** Make locally optimal choices

**Example Problem: Activity Selection**
```python
def activity_selection(activities):
    # Sort by end time
    activities.sort(key=lambda x: x[1])
    
    selected = [activities[0]]
    last_end_time = activities[0][1]
    
    for start, end in activities[1:]:
        if start >= last_end_time:
            selected.append((start, end))
            last_end_time = end
    
    return selected
```

### 3. Divide and Conquer
**Template:**
```python
def divide_and_conquer(problem):
    if problem_is_small(problem):
        return solve_directly(problem)
    
    subproblems = divide(problem)
    subresults = [divide_and_conquer(sub) for sub in subproblems]
    return combine(subresults)
```

---

## Time & Space Complexity Analysis

### Big O Notation Guide
- **O(1)** - Constant time
- **O(log n)** - Logarithmic (binary search, balanced trees)
- **O(n)** - Linear (single loop)
- **O(n log n)** - Linearithmic (efficient sorting)
- **O(n²)** - Quadratic (nested loops)
- **O(2ⁿ)** - Exponential (recursive without memoization)

### Space Complexity Considerations
- **Input space** - doesn't count toward space complexity
- **Auxiliary space** - extra memory used by algorithm
- **Recursive stack space** - counts toward space complexity

---

## Common Medium Problems & Solutions

### 1. String Manipulation
**Problem: Longest Palindromic Substring**
```python
def longest_palindrome(s):
    def expand_around_center(left, right):
        while left >= 0 and right < len(s) and s[left] == s[right]:
            left -= 1
            right += 1
        return s[left + 1:right]
    
    longest = ""
    for i in range(len(s)):
        # Odd length palindromes
        palindrome1 = expand_around_center(i, i)
        # Even length palindromes
        palindrome2 = expand_around_center(i, i + 1)
        
        current_longest = palindrome1 if len(palindrome1) > len(palindrome2) else palindrome2
        if len(current_longest) > len(longest):
            longest = current_longest
    
    return longest
```

### 2. Array Problems
**Problem: Product of Array Except Self**
```python
def product_except_self(nums):
    n = len(nums)
    result = [1] * n
    
    # Forward pass
    for i in range(1, n):
        result[i] = result[i - 1] * nums[i - 1]
    
    # Backward pass
    right_product = 1
    for i in range(n - 1, -1, -1):
        result[i] *= right_product
        right_product *= nums[i]
    
    return result
```

### 3. Linked List Problems
**Problem: Remove Nth Node From End**
```python
def remove_nth_from_end(head, n):
    dummy = ListNode(0)
    dummy.next = head
    first = second = dummy
    
    # Move first n+1 steps ahead
    for _ in range(n + 1):
        first = first.next
    
    # Move both pointers until first reaches end
    while first:
        first = first.next
        second = second.next
    
    second.next = second.next.next
    return dummy.next
```

---

## Common Hard Problems & Solutions

### 1. Advanced Dynamic Programming
**Problem: Edit Distance (Levenshtein Distance)**
```python
def min_distance(word1, word2):
    m, n = len(word1), len(word2)
    dp = [[0] * (n + 1) for _ in range(m + 1)]
    
    # Initialize base cases
    for i in range(m + 1):
        dp[i][0] = i
    for j in range(n + 1):
        dp[0][j] = j
    
    for i in range(1, m + 1):
        for j in range(1, n + 1):
            if word1[i - 1] == word2[j - 1]:
                dp[i][j] = dp[i - 1][j - 1]
            else:
                dp[i][j] = 1 + min(
                    dp[i - 1][j],      # Delete
                    dp[i][j - 1],      # Insert
                    dp[i - 1][j - 1]   # Replace
                )
    
    return dp[m][n]
```

### 2. Complex Graph Problems
**Problem: Shortest Path in Weighted Graph (Dijkstra's)**
```python
import heapq

def dijkstra(graph, start):
    distances = {node: float('inf') for node in graph}
    distances[start] = 0
    pq = [(0, start)]
    visited = set()
    
    while pq:
        current_distance, current_node = heapq.heappop(pq)
        
        if current_node in visited:
            continue
        
        visited.add(current_node)
        
        for neighbor, weight in graph[current_node].items():
            distance = current_distance + weight
            
            if distance < distances[neighbor]:
                distances[neighbor] = distance
                heapq.heappush(pq, (distance, neighbor))
    
    return distances
```

### 3. Advanced Tree Problems
**Problem: Serialize and Deserialize Binary Tree**
```python
def serialize(root):
    def preorder(node):
        if not node:
            return "null,"
        return str(node.val) + "," + preorder(node.left) + preorder(node.right)
    
    return preorder(root)

def deserialize(data):
    def build_tree():
        val = next(values)
        if val == "null":
            return None
        
        node = TreeNode(int(val))
        node.left = build_tree()
        node.right = build_tree()
        return node
    
    values = iter(data.split(','))
    return build_tree()
```

---

## AWS-Specific Problem Types

### 1. System Design Problems
**Load Balancing Algorithm:**
```python
class LoadBalancer:
    def __init__(self, servers):
        self.servers = servers
        self.current = 0
    
    def get_server(self):
        # Round-robin algorithm
        server = self.servers[self.current]
        self.current = (self.current + 1) % len(self.servers)
        return server
```

### 2. Distributed Systems
**Consistent Hashing Implementation:**
```python
import hashlib
import bisect

class ConsistentHash:
    def __init__(self, nodes=None, replicas=3):
        self.replicas = replicas
        self.ring = {}
        self.sorted_keys = []
        
        if nodes:
            for node in nodes:
                self.add_node(node)
    
    def _hash(self, key):
        return int(hashlib.md5(key.encode()).hexdigest(), 16)
    
    def add_node(self, node):
        for i in range(self.replicas):
            key = self._hash(f"{node}:{i}")
            self.ring[key] = node
            bisect.insort(self.sorted_keys, key)
    
    def get_node(self, key):
        if not self.ring:
            return None
        
        hash_key = self._hash(key)
        idx = bisect.bisect_right(self.sorted_keys, hash_key)
        
        if idx == len(self.sorted_keys):
            idx = 0
        
        return self.ring[self.sorted_keys[idx]]
```

### 3. Optimization Problems
**Resource Allocation:**
```python
def optimize_resource_allocation(tasks, resources):
    # Sort tasks by priority/deadline
    tasks.sort(key=lambda x: (x.deadline, -x.priority))
    
    allocation = {}
    for task in tasks:
        best_resource = min(resources, 
                          key=lambda r: r.current_load + task.estimated_time)
        
        if best_resource.can_handle(task):
            allocation[task.id] = best_resource.id
            best_resource.assign_task(task)
    
    return allocation
```

---

## Optimization Techniques

### 1. Memoization and Caching
```python
from functools import lru_cache

@lru_cache(maxsize=None)
def fibonacci(n):
    if n <= 1:
        return n
    return fibonacci(n - 1) + fibonacci(n - 2)
```

### 2. Bit Manipulation
**Common Operations:**
```python
def bit_operations(n):
    # Check if nth bit is set
    is_set = lambda num, n: bool(num & (1 << n))
    
    # Set nth bit
    set_bit = lambda num, n: num | (1 << n)
    
    # Clear nth bit
    clear_bit = lambda num, n: num & ~(1 << n)
    
    # Toggle nth bit
    toggle_bit = lambda num, n: num ^ (1 << n)
    
    # Count set bits
    count_bits = lambda num: bin(num).count('1')
    
    return {
        'is_set': is_set(n, 2),
        'set_bit': set_bit(n, 2),
        'clear_bit': clear_bit(n, 2),
        'toggle_bit': toggle_bit(n, 2),
        'count_bits': count_bits(n)
    }
```

### 3. Mathematical Optimizations
```python
def gcd(a, b):
    while b:
        a, b = b, a % b
    return a

def lcm(a, b):
    return (a * b) // gcd(a, b)

def sieve_of_eratosthenes(n):
    primes = [True] * (n + 1)
    primes[0] = primes[1] = False
    
    for i in range(2, int(n**0.5) + 1):
        if primes[i]:
            for j in range(i * i, n + 1, i):
                primes[j] = False
    
    return [i for i in range(2, n + 1) if primes[i]]
```

---

## Debugging & Testing Strategies

### 1. Edge Cases to Consider
- Empty input
- Single element
- Very large input
- Negative numbers
- Duplicate elements
- Already sorted/reverse sorted arrays

### 2. Testing Template
```python
def test_solution():
    # Test cases
    test_cases = [
        (input1, expected_output1),
        (input2, expected_output2),
        # Edge cases
        ([], expected_empty),
        ([1], expected_single),
    ]
    
    for i, (input_data, expected) in enumerate(test_cases):
        result = your_solution(input_data)
        assert result == expected, f"Test case {i+1} failed: expected {expected}, got {result}"
    
    print("All test cases passed!")
```

### 3. Performance Testing
```python
import time
import random

def benchmark_solution(solution_func, input_generator, sizes):
    for size in sizes:
        test_input = input_generator(size)
        
        start_time = time.time()
        result = solution_func(test_input)
        end_time = time.time()
        
        print(f"Size {size}: {end_time - start_time:.4f} seconds")
```

---

## Practice Roadmap

### Week 1-2: Foundation
1. Arrays and Strings (20 problems)
2. Hash Maps and Two Pointers (15 problems)
3. Basic recursion and backtracking (10 problems)

### Week 3-4: Intermediate
1. Trees and Graphs (25 problems)
2. Dynamic Programming basics (20 problems)
3. Sorting and searching (15 problems)

### Week 5-6: Advanced
1. Advanced DP (15 problems)
2. Complex graph algorithms (10 problems)
3. System design problems (10 problems)

### Week 7-8: AWS Focus
1. Distributed systems problems
2. Optimization problems
3. Mock interviews and time-boxed practice

### Key Problem Categories for AWS:
- **Array/String manipulation**: 30% of problems
- **Trees/Graphs**: 25% of problems
- **Dynamic Programming**: 20% of problems
- **System Design**: 15% of problems
- **Math/Logic**: 10% of problems

### Recommended Daily Practice:
- 2-3 medium problems (45-60 minutes each)
- 1 hard problem (90-120 minutes)
- Review and optimize previous solutions
- Practice explaining your approach out loud

---

## Final Tips for Success

### During the Interview:
1. **Clarify requirements** - Ask about edge cases, constraints
2. **Think out loud** - Explain your thought process
3. **Start with brute force** - Then optimize
4. **Test your code** - Walk through examples
5. **Analyze complexity** - Discuss time and space complexity

### Code Quality:
- Use meaningful variable names
- Add comments for complex logic
- Handle edge cases explicitly
- Write clean, readable code

### Time Management:
- Spend 5-10 minutes understanding the problem
- 10-15 minutes designing the solution
- 20-30 minutes implementing
- 5-10 minutes testing and debugging

Remember: Consistent practice is key. Focus on understanding patterns rather than memorizing solutions. Good luck with your AWS Online Assessment!
