# Konner's Way of Remembering Things


## <u>Backtracking</u>
- Build up a route towards a solution
- Define Stopping conditions
  - Or times when we "overshoot" the ending
- Slide indexes as you move closer to a stopping condition

<br>

- If no cases are hit, still <u>work to do</u>
  - (Exhaust all possibilities)
- Loop over all possibilities at given <u>level</u>

<br>

- Stop going down repeated paths (<b>dog searching</b>)
  - Leave "bread-crumbs" or "piss-stains" along your path
  - Eat the "fruit loops" on way back (Boo)

<br>

- Avoid duplicates => check (`i > idx and cands[i]==cands[i-1]`) (look behind you)
  - or `if used[i] or (i > 0 and nums[i]==nums[i-1] and not used[i-1]`)
  - `continue` over these cases

<br>

## <u>Binary Search</u>
- If anything is <i>inorder</i> for a given range, we can quickly pin point <b><u>the treasure!</u></b>
- Pin point the treasure/waldo/target by moving `lo` and `hi` to `mid` +- 1 
- Go until algorithm breaks (`lo > hi`)
  - If target exists, algorithm <i>will</i> find it!

<br>

- clearly define your `lo` & `hi` variables.
  - Ex: `lo, hi = max(nums), sum(nums)` or `lo, hi = 1, max(nums)`
- Typically do `lo <= hi`
- Squash down to minimax answer
- Ceiling: `math.ceil(A/B)` or `(A+B-1)//B`

<br>

## <u>DFS/BFS</u>
- We need a a "trigger" to begin a Dog Search

<br>

- Leave bread-crumbs / piss-stains before leaving
  - Potentially eat the fruit loop when you come back
  - After looping through routes, you arrive back on the level you started at ("you return home")
    - Do something after your "journey"
    - Ask yourself what you do after returning (`return 1` for example in num islands)

<br>

- Use `visited = set()` when possible
- Trust BFS will return shortest path
- Don't be afriad to edit the base matrix if possible
  - Mark visited with `'#'`
  - Increment counts at places
  - etc


<br>

## <u>Dynammic Programming</u>

- Typically a choice for either maximizing or minimizing! Make these bite-size choices using `min(x,y)` or `max(x,y)`
  - Usually binary decisions (steal or don't steal)
- `dp[i] = min(dp[i], dp[i-ways[j]] + cost)`, where the first parameter means **don't' use way** and right parameter means **use way**, except TANSTAAFL, there's a cost buddy boi.

<br>

- For 2D, we can add `dp[i][j] += max(dp[i-1][j-1], 0)` typically
- For substring, typically the algorithm goes:
```python
for i in range(n-1,-1,-1):
  for dis in range(1, n-i):
    j = i + dis
    dp[i][j] = s[i] == s[j] if dis == 1 else ...
```
- You can store booleans in the dp grid!
  - For instance if `s[i:j]` is a palindrome, mark True
  - Use `dis` variable to leverage your decision making

<br>

- We can loop unroll to compete 2 different decision makers against each other, ex:
```python
dp2[ch, b] = min(.., dp[a,b] + d(a, ch))
dp2[a, ch] = min(.., dp[a,b] + d(b, ch))
```
- Return `min(dp.values())` to choose the best between the decision makers

<br>

- Can also use dp as a dictionary to store words and their associated costs!
- Think of the math behind the problem, define your problem functions
- Ask if we can use 2 1-D arrays instead of a 2-D array
- Focus on the data structure the problem is asking about



## Graph
- Understand nodes & connections
  - A string of connections is a <u>route</u>
    - A route has a beginning and end
    - Dog Searches
- Create an undirected graph using a dictionary!
- Min path -> use BFS & queue


## Heap
- An efficient way of maintaining order for a stream of data
- Efficiently keep track of min and/or max of current stream
  - Can keep track of min and max at the same time!
  - `maxq, minq = [], []`
- Tuck tons of other helpful information behind the first entry ex: `(val, i, j, ...)`
- Use it to maintain order
  - Bottom half (max heap), top half (min heap)


## Linked List
- Better than array for adding shit in between
- Pointers baby!
- Dummy and a runner
  - Return dummy.next
- Inch worm
  - Move `runner.next` first
- Fast runners `while fastRunner and fastRunner.next:`
- Reverse:
```python
def reverse(head):
  prev = None
  while head:
    nxt = head.next
    head.next = prev
    prev = head
    head = nxt
  return prev
```


## Sliding Window
- All about the scrunch!
- Define conditions when to scrunch/un-scrunch
- When it's time to scrunch, define conditions when it's time to un-scrunch
- Use a frequency dictionary
- Sometimes, before sliding, it makes sense to update the frequencies of the input word first
- Leverage a counter variable to help with scrunching
- Keep track of exact location by:
```python
if (end - begin < minLen):
  head = begin
  minLen = end - begin
```

## <u>Stack</u>
- Adding dirty dishes "to-do"
- When you get the "soap", grab a dirty dish from the stack and clean the bitch!
  - The plate we pop off is "old"
- Plates can represent "levels"
- Sometimes, we jsut want to peek at the stack, use `stk[-1]`


## Tree
- Hierarchy
- Parents, Children
- Each child becomes the "root" of the generation beneath them
- If you've reached a non-existant child, turn around!!
- Make assumptions that it works!
  - `left = util(node.left)` and `right = util(node.right)`
  - After `left` and `right` have returned from their "journey", use what they've gathered
- As you leave your current level, give info back to your parents!
  - ex: `return max(left,right) + 1`
  - Sometimes you give yourself back to your parents
- You can end a generation by assigning a node to `None`
- Use a stack for DFS and a queue for BFS

<br>

- Use `nonlocal` for variables we need to increment
- `level = [child for node in level for child in (node.left, node.right) if child]`

BSTS
- Left subtree: elms `1...i-1`, right subtree: `i+1..n`
- When building all possible BSTs:
```python
for left in leftLst:
  for right in rightLst:
    root = TreeNode(i)
    root.left = left
    root.right = right
    lst.append(root)
return lst
```
- Add `None` if violation, all possible means `for i in range(start, end+1)`

<br>

- The successor of `root.left` is `root` in a BST
- The predecessor of `root.right` is `root` in a BST
- When building an iterator for a BST, the logic is very similar to the iterative method of printing out a tree in inorder fashion
- When checking for violations, `return -1` & pass it up the recursive chain when failure
  - Ultimately can check `util(root) !=  -1`
- If you need `node.left` & `node.right` values for an iterative solution, maintain `visited/depth` variables
  - Initialize with `depth = { None: 0 }`, check `depth[root] != -1` for final solution

<br>

- The first value in preorder traversal must be the root!
  - Can capture the idx of root in inorder traversal and split into 2 sub-problems


## Trie
- A trie is composed of TrieNodes!
- A trie has a root

Adding a word
```python
node = self.root
for letter in word:
  node = node.children[letter]
node.isWord = True # stamp
```
- Use DFS to find word in a trie
  - Chop off initial letter of search word as search through the trie
  - We can only search one letter at a time
  - Can explore all possbilities by looping:
```python
for n in node.children.values():
  dfs(n, word[1:])
```