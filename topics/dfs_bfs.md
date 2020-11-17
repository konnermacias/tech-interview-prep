# DFS / BFS Problems

## Number of Islands
[Link](https://leetcode.com/problems/number-of-islands/)
```python
def numIslands(grid):
  if not grid: return 0
  dirs = [(0,1),(0,-1),(1,0),(-1,0)]
  n, m = len(grid), len(grid[0])
  visited = set()

  def dfs(i,j):
    if (i,j) in visited: return 0
    visited.add((i,j))

    for d in dirs:
      ni, nj = i + d[0], j + d[1]
      if ((0 <= ni < n) and (0 <= nj < m) and (grid[ni][nj] == '1')):
        dfs(ni, nj)
    
    return 1

  numIslands = 0
  return sum(dfs(i,j) for i in range(n) for j in range(m) if grid[i][j] == '1')
```
Even faster:
```python
  def getCount(i,j):
    if ((0 <= i < n) and (0 <= j < m) and (grid[i][j] == '1')):
      grid[i][j] = '0'
      map(getCount, (i,i,i+1,i-1),(j+1,j-1,j,j))
      return 1
    return 0

   return sum(getCount(i,j) for i in range(n) for j in range(m))
```
Notes:
- Utilize a set to mark points as visited, or write a garbage character in its place (just mark it somehow!)
- Only return 1 after going through all possibilities
- Similar to graph problem. Loop through neighbors (go through all directions)


## Open the Lock
[Link](https://leetcode.com/problems/open-the-lock/submissions/)
```python
def openLock(self, deadends: List[str], target: str) -> int:
  s_deadends = set(deadends)
  queue = collections.deque([('0000',0)])
  visited = set('0000')
  
  while queue:
    combo, turns = queue.popleft()
    if combo == target:
      return turns
    elif combo in s_deadends:
      continue
    
    # iterate through combos
    for i, digit in enumerate(combo):
      # define your moves
      for move in [-1,1]:
        new_digit = (int(digit) + move) % 10
        new_combo = combo[:i] + str(new_digit) + combo[i+1:]
        if new_combo not in visited:
            visited.add(new_combo)
            queue.append((new_combo, turns+1))
  
  return -1
```
Notes:
- Keep track of visited. Trust in BFS for returning minimum solution
- Key piece is noticing what can we do at each possibility? We can add or subtract 1 from the value. Go through each possibility at that level.


## Longest Increasing Path in a Matrix
[Link](https://leetcode.com/problems/longest-increasing-path-in-a-matrix/)
```python
def longestIncreasingPath(self, matrix: List[List[int]]) -> int:
  if not matrix: return 0
  visited = {}
  m, n = len(matrix), len(matrix[0])
  dirs = ((0,1), (1,0), (0,-1), (-1,0))
  maxLen = 1
  
  def dfs(i, j):
    if (i,j) in visited:
      return visited[(i,j)]
    
    # we count
    mLen = 1
    for d in dirs:
      ni, nj = i+d[0], j+d[1]
      if 0 <= ni < m and 0 <= nj < n and matrix[ni][nj] > matrix[i][j]:
        l = 1 + dfs(ni, nj)
        mLen = max(mLen, l)

    visited[(i,j)] = mLen
    return mLen
  
  for i in range(m):
    for j in range(n):
      maxLen = max(maxLen, dfs(i,j))
  return maxLen
```
Notes:
- Trigger a BFS for each cell, maintain a visited so we don't repeat going down repeat paths

## Friend Circles
[Link](https://leetcode.com/submissions/detail/373032982/)
```python
def findCircleNum(M):
  n = len(M)
  visited = [False]*n
  numCircles = 0
  def dfs(i):
    for j in range(n):
      if M[i][j] == 1 and not visited[j]:
        visited[j] = True
        dfs(j)
  
  for i in range(n):
    if not visited[i]:
      dfs(i)
      numCircles += 1
  return numCircles
```
Notes:
- Keep visited array if you've seen the friend before. Use dfs if not visited.

## Walls and Gates
[Link](https://leetcode.com/problems/walls-and-gates/)
```python
def wallsAndGates(rooms):
  if not rooms:
    return

  m, n = len(rooms), len(rooms[0])
  queue = collections.deque([])
  for i, row in enumerate(rooms):
    for j, col in enumerate(row):
        if rooms[i][j] == 0:
            queue.append((i,j))

  while queue:
    cur_i, cur_j = queue.popleft()
    if cur_i > 0 and rooms[cur_i-1][cur_j] == 2147483647:
        rooms[cur_i-1][cur_j] = rooms[cur_i][cur_j] + 1
        queue.append((cur_i-1, cur_j))
    # repeat for other i,j combos...
```
Notes:
- Start from the gates and work your way out to the INF values
- Utilize the gate stat value of zero to begin incrementing values

## Confusing Number II
[Link](https://leetcode.com/problems/confusing-number-ii/)
```python
def confusingNumberII(N):
  validDigits = [0,1,6,8,9]
  mapping = { 0:0, 1:1, 6:9, 8:8, 9:6 }

  def dfs(num, rotation, mult):
    res = 0
    # good confusing number!
    if num != rotation:
      res += 1
    
    for d in validDigits:
      if d == 0 and num == 0:
        continue
      
      if num*10 + d <= N:
        res += dfs(num*10 + d, mapping[d]*mult + rotation, mult*10)
    return res
  
  return dfs(0,0,1)
```
Notes:
- Look at how we're building up the num and rotation
- We only dive into dfs if it's valid and does not violate the constraints

## Time Needed to Inform all Employees
[Link](https://leetcode.com/problems/time-needed-to-inform-all-employees/)
```python
def numOfMinutes(n, headID, manager, informTime):
  emplToSuboord = collections.defaultdict(list)
  for emp_id, manager in enumerate(manager):
    if manager != -1:
      emplToSuboord[manager].append(emp_id)
  

  def dfs(emp_id):
    time = 0
    for sub_id in emplToSuboord[emp_id]:
      time = max(time, dfs(sub_id))
    return time + informTime[emp_id]

  # bfs
  queue = collections.deque([(headID, 0)])
  max_time = 0
  while queue:
    emp_id, cur_time = queue.popleft()
    max_time = max(max_time, cur_time)

    for sub_id in emplToSuboord[emp_id]:
      queue.append((sub_id, cur_time + informTime[emp_id]))
  
  return max_time
```
Notes:
- Pass time + informTime of given employee up the recursive chain

## Word Ladder
[Link](https://leetcode.com/problems/word-ladder/)
```python
def ladderLength(beginWord, endWord, worList):
  wordList = set(wordList)
  queue = collections.deque([(beginWord, 0)])
  while queue:
    cur_word, cur_len = queue.popleft()
    if cur_word == endWord:
      return cur_len + 1
    for i in range(len(cur_word)):
      for ch in 'abcdefghijklmnopqrstuvwxyz':
        new_word = cur_word[:i] + ch + cur_word[i+1:]
        if new_word in wordList:
          wordList.remove(new_word)
          queue.append((new_word, cur_len+1))
  return 0
```
Notes:
- Simple BFS


## Work Break II
[Link](https://leetcode.com/problems/word-break-ii/)
```python
def wordBreak(s, wordDict):
  def dfs(s, dp):
    if dp.get(s, []):
      return dp[s]
    
    res = []
    if not s:
      res.append("")
      return res
    
    for word in wordDict:
      if s.startswith(word):
        sublst = dfs(s[len(word):], dp)
        for sub in sublst:
          res.append(word + (" " if sub else "") + sub)
    dp[s] = res
    return res
  return dfs(s, {})
```
Notes:
- Move down one valid word at a time through `s` using dfs.
- The base case is the end of `s`, return `""`
- Build up your res string going backwards
- Save results for combinations for a given substring