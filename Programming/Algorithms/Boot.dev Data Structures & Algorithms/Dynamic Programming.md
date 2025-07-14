# Dynamic Programming

## What is it?

Dynamic Programming (DP) is like solving a jigsaw puzzle by remembering which pieces you've already tried. Instead of solving the same subproblem multiple times, you solve it once and store the answer for future use.

Think of it as smart recursion with a memory - you break down complex problems into simpler subproblems, but you remember the solutions to avoid doing the same work twice.

## Key Properties

- **Overlapping Subproblems**: The same smaller problems appear multiple times
- **Optimal Substructure**: The optimal solution contains optimal solutions to subproblems
- **Memoization**: Store results of expensive function calls
- **No redundant work**: Each subproblem is solved exactly once

## Time/Space Complexity

- **Time Complexity**: Usually O(n²) or O(n³) instead of exponential
- **Space Complexity**: O(n) to O(n²) for storing subproblem solutions
- **Trade-off**: Use more memory to achieve dramatically better time complexity

## Two Main Approaches

### 1. Memoization (Top-Down)
- Start with the original problem
- Break it down recursively
- Store results in a cache (usually a map or array)

### 2. Tabulation (Bottom-Up)
- Start with the smallest subproblems
- Build up to the original problem
- Fill a table systematically

## Implementation Examples

### Classic Example: Fibonacci Sequence

**Naive Recursive (Exponential Time):**
```go
func fibNaive(n int) int {
    if n <= 1 {
        return n
    }
    return fibNaive(n-1) + fibNaive(n-2)
}
// Time: O(2^n) - VERY SLOW!
```

**Memoization Approach:**
```go
func fibMemo(n int, memo map[int]int) int {
    if n <= 1 {
        return n
    }
    
    if val, exists := memo[n]; exists {
        return val
    }
    
    memo[n] = fibMemo(n-1, memo) + fibMemo(n-2, memo)
    return memo[n]
}

func fibonacci(n int) int {
    memo := make(map[int]int)
    return fibMemo(n, memo)
}
// Time: O(n), Space: O(n)
```

**Tabulation Approach:**
```go
func fibTabulation(n int) int {
    if n <= 1 {
        return n
    }
    
    dp := make([]int, n+1)
    dp[0] = 0
    dp[1] = 1
    
    for i := 2; i <= n; i++ {
        dp[i] = dp[i-1] + dp[i-2]
    }
    
    return dp[n]
}
// Time: O(n), Space: O(n)
```

**Space-Optimized Tabulation:**
```go
func fibOptimized(n int) int {
    if n <= 1 {
        return n
    }
    
    prev2 := 0
    prev1 := 1
    
    for i := 2; i <= n; i++ {
        current := prev1 + prev2
        prev2 = prev1
        prev1 = current
    }
    
    return prev1
}
// Time: O(n), Space: O(1)
```

### Advanced Example: Coin Change Problem

**Problem**: Find minimum coins needed to make a target amount.

```go
func coinChange(coins []int, amount int) int {
    // dp[i] = minimum coins needed to make amount i
    dp := make([]int, amount+1)
    
    // Initialize with impossible value
    for i := 1; i <= amount; i++ {
        dp[i] = amount + 1
    }
    
    dp[0] = 0 // Base case: 0 coins needed for amount 0
    
    // Fill the dp table
    for i := 1; i <= amount; i++ {
        for _, coin := range coins {
            if coin <= i {
                dp[i] = min(dp[i], dp[i-coin]+1)
            }
        }
    }
    
    if dp[amount] > amount {
        return -1 // Impossible to make the amount
    }
    return dp[amount]
}

func min(a, b int) int {
    if a < b {
        return a
    }
    return b
}

// Example usage
func main() {
    coins := []int{1, 3, 4}
    amount := 6
    result := coinChange(coins, amount)
    fmt.Printf("Minimum coins needed: %d\n", result) // Output: 2 (3+3)
}
```

### 2D DP Example: Longest Common Subsequence

```go
func longestCommonSubsequence(text1, text2 string) int {
    m, n := len(text1), len(text2)
    
    // dp[i][j] = LCS length for text1[0...i-1] and text2[0...j-1]
    dp := make([][]int, m+1)
    for i := range dp {
        dp[i] = make([]int, n+1)
    }
    
    for i := 1; i <= m; i++ {
        for j := 1; j <= n; j++ {
            if text1[i-1] == text2[j-1] {
                dp[i][j] = dp[i-1][j-1] + 1
            } else {
                dp[i][j] = max(dp[i-1][j], dp[i][j-1])
            }
        }
    }
    
    return dp[m][n]
}

func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}
```

## When to Use Dynamic Programming

**Look for these signs:**
- **Optimization problems** (minimize/maximize something)
- **Counting problems** (how many ways to do something)
- **Decision problems** (is it possible to achieve something)
- **Recursive solution exists** but with repeated subproblems
- **Overlapping subproblems** and **optimal substructure**

**Common DP Problem Types:**
- **Knapsack problems** (0/1 knapsack, unbounded knapsack)
- **Path problems** (unique paths, minimum path sum)
- **String problems** (edit distance, palindrome partitioning)
- **Sequence problems** (longest increasing subsequence)
- **Game theory problems** (stone game, predict the winner)

## Problem-Solving Steps

1. **Identify if it's a DP problem**
   - Does it have overlapping subproblems?
   - Does it have optimal substructure?

2. **Define the state**
   - What parameters uniquely identify a subproblem?
   - Usually becomes your dp array dimensions

3. **Write the recurrence relation**
   - How does the current state relate to previous states?

4. **Identify base cases**
   - What are the simplest cases you can solve directly?

5. **Decide on approach**
   - Top-down (memoization) or bottom-up (tabulation)?

6. **Implement and optimize**
   - Can you reduce space complexity?

## Common Pitfalls

- **Not identifying the correct state**: Your dp table structure is crucial
- **Wrong recurrence relation**: Double-check your logic
- **Forgetting base cases**: Always handle the simplest cases
- **Off-by-one errors**: Be careful with array indices
- **Not considering all transitions**: Make sure you explore all possibilities
- **Overcomplicating**: Start with the recursive solution first

## Memory Optimization Tricks

- **Rolling arrays**: If you only need the previous row/column
- **Space-optimized**: Use variables instead of arrays when possible
- **In-place DP**: Modify the input array if allowed

## DP vs Other Approaches

| Approach | Time | Space | When to Use |
|----------|------|-------|-------------|
| Brute Force | O(2^n) | O(n) | Very small inputs |
| Memoization | O(n²) | O(n²) | Natural recursive solution |
| Tabulation | O(n²) | O(n²) | Iterative preference |
| Optimized DP | O(n²) | O(n) | Space is critical |

## Related Concepts

- **[[Big O notation]]**: Understanding time complexity improvements
- **[[Recursion]]**: The foundation of DP thinking
- **[[Memoization]]**: Caching technique used in top-down DP
- **[[Graphs]]**: Many DP problems can be viewed as graph problems