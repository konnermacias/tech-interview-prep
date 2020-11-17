# Dynamic Programming Problems

## Fibonacci
```python
def fib(n):
  dp = [0]*(n+1)
  dp[0] = 0
  dp[1] = 1
  for i in range(2, n+1):
    dp[i] = (dp[i-1] + dp[i-2])
  return dp[n]
```
Notes:
- Caching result in array. Mark your base cases and don't repeat yourself
## Coin Change
[Link](https://leetcode.com/problems/coin-change/)
```python
def coinChange(coins, amount):
  maxVal = amount + 1
  dp = [maxVal]*maxVal
  dp[0] = 0
  for coin in coins:
    for i in range(coin, maxVal):
      dp[i] = min(dp[i], dp[i-coin]+1)
  return dp[amount] if dp[amount] <= amount else -1
```
Notes:
- Sub-problem: find minimum number of coins needed towards amount `i` where `1 <= i < amount`
- Decision at ith step: select to use current coin, or don't use coin and keep what's there.

## Max Dot Product of Two Subsequences
[Link](https://leetcode.com/problems/max-dot-product-of-two-subsequences/)
```python
def maxDotProduct(nums1, nums2):
  n, m = len(nums1), len(nums2)
  dp = [[0]*m for _ in range(n)]
  for i in range(n):
    for j in range(m):
      dp[i][j] = nums1[i]*nums2[j]
      if i and j: dp[i][j] += max(dp[i-1][j-1],0)
      if i: dp[i][j] = max(dp[i][j], dp[i-1][j])
      if j: dp[i][j] = max(dp[i][j], dp[i][j-1])
  return dp[-1][-1]
```
Notes:
- Sub-problem: `F(i,j)` is the max dot product of subsequences from `A[0:i]` and `B[0:j]`.
- Decisions: `F(i,j) = max(F(i-1,j), F(i,j-1), F(i-1,j-1) + A[i]*B[j])`
- Note these represent the 3 top-left surrounding cells as you move top down! These are our previous histories we can use.

## Maximum subarray
[Link](https://leetcode.com/problems/maximum-subarray/)
```python
def maxSubArray(nums):
  n = len(nums)
  dp = [0]*n
  for i in range(n):
    dp[i] = max(dp[i-1]+nums[i], nums[i])
  return max(dp)
```
Notes:
- Sub-problem: find max subarray for index `i` for in `A[:i]`
- Decision at `i`th step: `F(i) = max(F(i-1) + nums[i], nums[i])`
- We can either use our history, or start fresh with the current value.

## Maximum Product Subarray
[Link](https://leetcode.com/problems/maximum-product-subarray/)
```python
def maxProduct(A):
  n = len(A)
  B = A[::-1]
  for i in range(n):
    A[i] *= A[i-1] or 1
    B[i] *= B[i-1] or 1
  return max(A+B)
```
Notes:
- This isn't straight forward across since with multiplication, we don't get the same products going right to left vs. left to right
- Go both directions (Ex: [2,3,4] left-> 6 * 24 vs right <- 24 * 12)
- Choose to use our history or start fresh by multiplying by 1.

## Longest Palindromic Substring
[Link](https://leetcode.com/problems/longest-palindromic-substring/)
```python
def longestPalindrome(s):
  n = len(s)
  dp = [[False]*n for _ in range(n)]
  for i in range(n):
    dp[i][i] = True

  head, maxDis = 0. 1
  for i in range(n-1,-1,-1):
    for dis in range(1, n-i):
      j = i + dis
      dp[i][j] = s[i] == s[j] if dis == 1 else (s[i] == s[j] and dp[i+1][j-1])
      if dp[i][j] and (j-i+1) > maxDis:
        maxDis = j-i+1
        head = i
  return s[head:head+maxDis]
```
Notes:
- So much a salsa! Intuitively note that we will need an `i` and `j` as we're messing with a substring
- Sub-problem: Indicate if `i` and `j` produce a palindromic substirng. Indicate true/false
- Decision: if dis is 1, check equal. Otherwise use history by looking back by moving the `i` and `j` indices
- Move `i` backwards while you move `dis` towards the end. Update head if true and new max distance

## Maximal Rectangle
[Link](https://leetcode.com/problems/maximal-rectangle/)
```python
def maximalRectangle(matrix):
  if not matrix: return 0
  n, m = len(matrix), len(matrix[0])
  # (right(i,j)-left(i,j))*height(i,j)
  right, left, height = [m]*m, [0]*m, [0]*m
  maxArea = 0
  for i in range(n):
    cur_left, cur_right = 0, m
    for j in range(m-1,-1,-1):
      if matrix[i][j] == '1':
        right[j] = min(right[j], cur_right)
    else:
      right[j] = m
      cur_right = j
    
    # left and height
    for j in range(m):
      if matrix[i][j] == '1':
        height[j] += 1
        left[j] = max(left[j], cur_left)
        maxArea = max(maxArea, (right[j]-left[j])*height[j])
      else:
        height[j] = 0
        left[j] = 0
        cur_left = j+1
  return maxArea
```
Notes:
- So much a salsa! Ask what's needed for a rectangle: base*height. Base is (right-left).
- `F(i,j) = (right(i,j) - left(i,j))*height(i,j)`
- `right(i,j) = min(right(i-1,j), cur_right)`
- `left(i,j) = max(left(i-1,j), cur_left)`
- `height(i,j) = height(i-1,j)+1 if mat[i][j] else 0`
- For calculating right, we need to move from `m-1` to `0` which is why we take the min each time.
- You can define 3, 1-D arrays instead

## Unique Binary Search Trees
[Link](https://leetcode.com/problems/unique-binary-search-trees/)
```python
def numTrees(n):
  dp = [0]*(n+1)
  dp[0] = dp[1] = 1
  for i in range(2, n+1):
    for j in range(1, i+1):
      dp[i] += dp[j-1]*dp[i-j]
  return dp[n]
```
Notes:
- Trying to solve: `G(n)` = #unique BSTs of length `n`. Use `F(i,n)` = #unique BSTs where number `i` is root of BST from ranges `1 <= i <= n`
- `G(n) = F(1,n) + ... + F(n,n)`. We split up the array into left & right subtrees. (utilize previous history)
- `F(i,n) = G(i-1)*G(n-i)`. So `G(n) = G(0)*G(n-1) + ... + G(n-1)*G(0)`
- Thus we loop through like so (beautiful math)

## Minimum Distance to Type a Word using Two Fingers
[Link](https://leetcode.com/problems/minimum-distance-to-type-a-word-using-two-fingers/)
```python
def minimumDistance(word):
  def d(a,b):
    return a and abs(a//6 - b//6) + abs(a%6 - b%6)
  
  dp, dp2 = {(0,0): 0}, {}
  for ch in (ord(ch)+1 for ch in word):
    for a, b in dp:
      # move finger 1 to ch
      dp2[ch,b] = min(dp2.get((ch,b),3000), dp[a,b] + d(a,ch))
      dp2[a,ch] = min(dp2.get((a,ch),3000), dp[a,b] + d(b,ch))
    dp, dp2 = dp2, {}
  
  return min(dp.values())
```
Notes:
- Decision: either keep the minimum which is there. Or take the minimum distance from before and add your new distance.
- return `a` is a clever way of returing 0 if `a == 0`

## Minimum Knight Moves
[Link](https://leetcode.com/problems/minimum-knight-moves/)
```python
def minKnightMoves(x,y):
  def dfs(x,y):
    if (x,y) not in memo:
      x, y = abs(x), abs(y)
      if x == y == 0:
        return 0
      if x + y == 2:
        return 2
      ans = min(dfs(x-1,y-2), dfs(x-2,y-1)) + 1
      memo[(x,y)] = ans
    return memo[(x,y)]
  
  memo = {}
  return dfs(x,y)
```
Notes:
- Math deduction. Notice the problem can be solved with 1/8th of the board.
- Make a decision to either move backwards -1 and -2 for x and y (not at the same time)
- Store result of going to that location in memoized dictionary

## Longest String Chain
[Link](https://leetcode.com/submissions/detail/344547771/)
```python
def longestStringChain(words):
  dp = {}
  for word in sorted(words, key=len):
    dp[word] = max(dp.get(word[:i]+word[i+1:],0)+1 for i in range(len(word)))
  return max(dp.values())
```
Notes:
- Method of going through all possibilities of having just one char removed from string: `word[:i]+word[i+1:]` looped
- For each word store the longest path to get there. Use history if you have it, or just mark a 1 there.


## Common Substrings between Two Strings
[Link](https://leetcode.com/discuss/interview-question/653576/Google-or-Phone-or-Common-substrings-between-two-strings)
```python
def solve(s1, s2):
  res = []
  A, B = s1.split(' '), s2.split(' ')
  dp = [[0]*(len(s2)+1) for _ in range(len(s1)+1)]
  
  # indicate how many times in a row we've seen
  for i in range(1, len(A)):
    for j in range(1, len(B)):
      if A[i-1] == B[j-1]:
        dp[i][j] = dp[i-1][j-1] + 1
      else:
        dp[i][j] = 0
  
  for i in range(len(dp)-1,-1,-1):
    for j in range(len(dp[0])-1,-1,1):
      # sub-sentence requires at least 3 words
      if dp[i][j] >= 3:
        s = ""
        ni, nj = i, j
        while (dp[ni][nj] > 0):
          dp[ni][nj] = 0
          s = A[ni-1] + " " + s
          
          ni -= 1
          nj -= 1
        
        s = s[:len(s)-1]
        res.append(s)
  
  return res
```
Notes:
- Two sentences => 2D DP. Each word let's keep a count of if it equals to a word in B
  - Since it's in a row, we can indicate 'in a row' by doing `dp[i-1][j-1]`

- Scan through again and find the words which had 3 in a row, go backwards to build up the string


## Maximal Square
[Link](https://leetcode.com/problems/maximal-square/)
```python
def maximalSquare(matrix):
  if not matrix: return 0
  m, n = len(matrix), len(matrix[0])
  dp = [[0]*n for _ in range(m)]
  maxSize = 0
  for i in range(m):
    for j in range(n):
      if i == 0 or j == 0 or matrix[i][j] == '0':
        dp[i][j] = int(matrix[i][j])
      else:
        dp[i][j] = min(dp[i-1][j-1],dp[i-1][j],dp[i][j-1]) + 1

      maxSize = max(maxSize, dp[i][j])
  return maxSize*maxSize
```
Notes:
- Can just keep track of the one side of a square.

## Minimum Falling Path Sum
[Link](https://leetcode.com/problems/minimum-falling-path-sum/)
```python
def minFallingPathSum(self, A: List[List[int]]) -> int:
  m, n = len(A), len(A[0])
  dp = [[0]*(n) for _ in range(m)]
  dp[0] = A[0]
  
  for i in range(1,m):
    for j in range(n):
      if j == 0:
        dp[i][j] = min(dp[i-1][j+1], dp[i-1][j]) + A[i][j]
      elif j == n-1:
        dp[i][j] = min(dp[i-1][j-1], dp[i-1][j]) + A[i][j]
      else:
        dp[i][j] = min(dp[i-1][j-1], dp[i-1][j], dp[i-1][j+1]) + A[i][j]
  return min(dp[-1]) # min of final row
```
Notes:
- Simple top-down DP, just look at the row above you

## Paint House III
[Link](https://leetcode.com/problems/paint-house-iii/)
```python
def minCost(houses, cost, m, n, target):
  dp = {}
  def dfs(i, p, t):
    
    if i == len(houses) or t < 0:
      return 0 if t == 0 else float('inf')
    
    if (i,p,t) not in dp:
      if houses[i] == 0:
        dp[(i,p,t)] = min(dfs(i+1,nc,t-(nc != p)) + cost[i][nc-1] for nc in range(1,n+1))
      else:
        dp[(i,p,t)] = dfs(i+1,houses[i],t-(houses[i] != p))
    
    return dp[(i,p,t)]
    
  
  res = dfs(0,-1,target)
  return res if res < float('inf') else -1
```
Notes:
 - Backtracking, dfs, and dp! Slide index of houses, only update remaining neighborhoods if different color
  - Don't repeat yourself and save the house, color, and remaining neighborhood pairing

## Work Break
[Link](https://leetcode.com/problems/word-break/submissions/)
```python
# O(nm)
def wordBreak(s, wordDict):
  n = len(s)
  dp = [False]*(n+1)
  dp[0] = True
  for i in range(1, n+1):
    for j in range(0, i):
      if dp[j] and s[j:i] in wordDict:
        dp[i] = True
        break
  return dp[n]
```
Notes:
- When we detect a valid word, let's indicate in `dp` that `dp[i] = True`.
  - That way, we can know we're starting from a good place

## House Robber
[Link](https://leetcode.com/problems/house-robber/)
```python
def rob(nums):
  if not nums: return 0
  n = len(nums)
  dp = [0] * (n+1)
  dp[0] = 0
  dp[1] = nums[0]
  for i in range(1, n):
    dp[i+1] = max(dp[i], dp[i-1] + nums[i])
  return dp[n]
```
Notes:
- Either keep the max sum from previous encountered (i.e. Don't Rob) `dp[i]` or Rob which is adding the max earnings from 2 houses away `dp[i-1] + nums[i]`

