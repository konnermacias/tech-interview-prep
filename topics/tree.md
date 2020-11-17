# Favorite Problems

## Invert Binary Tree
[Link](https://leetcode.com/problems/invert-binary-tree/)

Recursive way:
```python
def util(root):
  if not root:
    return

  tmp = root.left
  root.left = util(root.right)
  root.right = util(tmp)

  # for parent nodes
  return root
```
Using Stack:
```python
def invert(root):
  dummy = root
  stk = [root]
  while stk:
    node = stk.pop()
    if not node:
      continue
    node.left, node.right = node.right, node.left
    
    stk += node.left, node.right

  return dummy
```
Notes:
- Showcase the recursive solution but also using a stack (DFS) or queue (BFS)
- For recursive, after your left and right util statements, return what's useful for a parent node to return (in this case `root`)


## Binary Tree Maximum Path Sum
[Link](https://leetcode.com/problems/binary-tree-maximum-path-sum/)

```python
mps = float('-inf')
def util(root):
  if not root:
    return 0

  left = self.util(root.left)
  right = self.util(root.right)

  self.mps = max(self.mps, root.val + left + right)

  return max(root.val + max(left, right), 0)
```
Notes:
- One your have the max path on your left subtree and right subtree, you add all them together to create an ultra sum
- For the parent, on the way back up, bring up your value and that max of either the left or right. Can only choose one since it's a single path.

## Diameter of Binary Tree
[Link](https://leetcode.com/problems/diameter-of-binary-tree/)
```python
md = 0
def maxDepth(root):
  if not root:
    return 0
  
  left = self.maxDepth(root.left)
  right = self.maxDepth(root.right)

  self.md = max(self.md, left+right)
  return max(left, right) + 1
```
- Add 1 for the parent node itself on your way back up plus the best route (either left or right not both)


## Binary Tree RHS View
[Link](https://leetcode.com/problems/binary-tree-right-side-view/)

```python
def rhsView(root):
  if not root:
    return []

  right_nodes = []
  level = [root]
  while level:
    right_nodes += [level[-1].val]
    level = [child for node in level for child in (node.left, node.right) if child]

  return right_nodes
```
Notes:
- Drop one level at a time. Choose the right node added

## Path Sum II
[Link](https://leetcode.com/problems/path-sum-ii/)
```python
def pathSum(root, summ):
  if not root:
    return []
  
  res = []
  queue = collections.deque([(root, [root.val])])
  while queue:
    node, lst = queue.popleft()

    if not node.left and not node.right and sum(lst) == summ:
      res.append(lst)
    
    if node.left:
      queue.append((node.left, lst + [node.left.val]))
    if node.right:
      queue.append((node.right, lst + [node.right.val]))

  return res
```
Notes:
- Utilize queue and keep track of a path sum using a lst which you add to a queue
- Make sure the node you're looking at is a leaf (make sure `not node.left and not node.right`)

## Path Sum
[Link](https://leetcode.com/submissions/detail/340826324/)
```python
def hasPathSum(root, summ):
  if not root:
    return False
  
  if not root.left and not root.right and root.val == sum:
    return True

  return self.hasPathSum(root.left, sum - root.val) or self.hasPathSum(root.right, sum - root.val)
```
Notes:
- Rather than carry a list, slowly bring down sum. Check if node.val == remaining.
- Check routes down either the left or right subtree

## Same Tree
[Link](https://leetcode.com/submissions/detail/340820034/)
```python
def isSameTree(p, q):
  queue = collections.deque([(p,q)])
  while queue:
    pn, qn = queue.popleft()
    if pn and qn:
      if pn.val != qn.val:
        return False
      else:
        queue.append((pn.left, qn.left))
        queue.append((pn.right, qn.right))
    elif pn is not qn:
      return False
  return True
```
Notes:
- Utilize queue to perform BFS. Check the node's values if they are both valid. Add their children after.
- Utilize Python's `is` to perform a XOR

## Validate Binary Search Tree
[Link](https://leetcode.com/submissions/detail/340806724/)
```python
def util(node, mini, maxi):
  if not node:
    return True
  
  if mini is not None and node.val <= mini:
    return False
  if maxi is not None and node.val >= maxi:
    return False

  return self.util(node.left, mini, node.val) and self.util(node.right, node.val, maxi)
```
Notes:
- Think in terms of "crashing" down left and "crashing" down right
- When crashing left, confirm the value doesn't exceed the value of its parent to the right
- Same thing when crashing right
- Recurse using 'and' to be thorough and look both ways


## Binary Tree Preorder Traversal
[Link](https://leetcode.com/submissions/detail/340431699/)

```python
def preorder(root):
  res, stk = [], []
  while True:
    while root:
      res.append(root.val)
      stk.append(root)
      root = root.left
    
    if not stk:
      return res
    
    node = stk.pop()
    root = node.right
```
Notes:
- Recursion trivial. Utilize a stack instead for a DFS.
- Crash left util you can't anymore. Add each node along the way into the stack
- After crashing, pop from the stack and go right. Repeat crashing left then popping and going right

## Inorder Tree Traversal
[Link](https://leetcode.com/problems/binary-tree-inorder-traversal/)
```python
def inorder(root):
  res, stk = [], []
  while True:
    while root:
      stk.append(root)
      root = root.left
    
    if not stk:
      return res
    
    node = stk.pop()
    res.append(node.val)
    root = node.right
```
Notes:
- Crash left, then pop and print node. Then go right.
- Main difference is we're not printing as we crash left.

## Delete Nodes and Return Forest
[Link](https://leetcode.com/problems/delete-nodes-and-return-forest/)
```python
def delNodes(root, to_delete):
  res = []
  def util(root, is_root):
    if not root: return None
    root_deleted = root in to_delete
    if is_root and not root_deleted:
      res.append(root)
    root.left = util(root.left, to_delete)
    root.right = util(root.right, to_delete)
    return None if root_deleted else root
  
  util(root, True)
  return sln_set
```
Notes:
- Clever way of adding the parent node even tho you might make its children `None` in the future

## Flip Equivalent Binary Trees
[Link](https://leetcode.com/problems/flip-equivalent-binary-trees/)
```python
def flipEquiv(self, root1, root2):
  if not r1 or not r2: return r1 == r2 == None
  return r1.val == r2.val and (self.flipEquiv(r1.left, r2.left) and  self.flipEquiv(r1.right, r2.right) \
                          or self.flipEquiv(r1.left, r2.right) and self.flipEquiv(r1.right, r2.left))
```
Notes:
- Check all comparisons whether we flip the tree or not


## Unique Binary Search Trees II
[Link](https://leetcode.com/problems/unique-binary-search-trees-ii/)
```python
def generateTrees(n):
  def genTreeList(start, end):
    lst = []
    # add None if violation
    if (start > end):
      lst.append(None)
    
    for i in range(start, end+1):
      leftLst = genTreeList(start, i-1)
      rightLst = genTreeList(i+1, end)

      for left in leftLst:
        for right in rightLst:
          root = TreeNode(i)
          root.left = left
          root.right = right
          lst.append(root)
    return lst
  
  return genTreeList(1,n)
```
Notes:
- Pick `i` to be root. Left subtree has elms `1..i-1` and right subtree has elms `i+1..n`
- Indicate condition when to add `None`, build from leaf nodes up
- The for loop allows you to exhaust all possibilities

## Inorder Successor in BST
[Link](https://leetcode.com/problems/inorder-successor-in-bst/)
```python
def successor(root, p):
  if not root: return None

  # crash right
  if root.val <= p.val:
    return successor(root.right, p)
  else:
    left = successor(root.left, p)
    return left if left is not None else root

def predecessor(root, p):
  if not root: return None

  if root.val >= p.val:
    return predecessor(root.left, p)
  else:
    right = predecessor(root.right, p)
    return right if right is not None else root
```
Notes:
- Learn to crash right and left, scrunch towards answer

## Binary Search Tree Iterator
[Link](https://leetcode.com/problems/binary-search-tree-iterator/)
```python
def __init__(self, root):
  self.stk = []
  self.pushAll(root)
def next(self):
  tmp = self.stk.pop()
  self.pushAll(tmp.right)
  return tmp.val
def hasNext(self):
  return self.stk
def pushAll(self, node):
  while node is not None:
    self.stk.append(node)
    node = node.left
```
Notes:
- Add nodes in preorder fashion, crash left, pop and to go right
- You'll end up printing the nodes in inorder fashion

## Balanced Binary Tree
[Link](https://leetcode.com/problems/balanced-binary-tree/)
```python
# recursive
def isBalanced(root):
  def util(root):
    if not root: return 0
    left = util(root.left)
    right = util(root.right)
    if left == -1 or right == -1 or abs(left-right) > 1:
      return -1
    return max(left, right) + 1
  return util(root) != -1

# iterative
def isBalanced(root):
  depth, stk = { None: 0}, [(root, False)]
  while stk:
    node, visited = stk.pop()
    if not node: continue

    if not visited:
      stk.append((node, True))
      stk.append((node.left, False))
      stk.append((node.right, False))
    else:
      left, right = depth[node.left], depth[node.right]
      if left == -1 or right == -1 or abs(left-right) > 1:
        depth[node] = -1
      else:
        depth[node] = max(left, right) + 1
  return depth[root] != -1
```
Notes:
- Recursive:
  - Get depth down left and right side. Anything
  - We can stop early when we violate the `abs(left-right) > 1`
- Iterative:
  - Only compare left and right if we have visited the node before
  - Use a dictionary to keep track of depths

## Construct Binary Tree from Preorder/Inorder Traversal
[Link](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)
```python
def buildTree(preorder, inorder):
  if inorder:
    ind = inorder.index(preorder.pop(0))
    root = TreeNode(inorder[ind])
    root.left = buildTree(preorder, inorder[0:ind])
    root.right = buildTree(preorder, inorder[ind+1:])
    return root
```
Notes:
- The first value of preorder traversal must be the root!
- Capture idx of root within inorder, split into 2 sub-problems


## Distribute Coins
[Link](https://leetcode.com/submissions/detail/371644853/)
```python
def distributeCoins(root):
  res = 0
  if root.left:
    res += distributeCoins(root.left)
    root.val += root.left.val - 1
    res += abs(root.left.val - 1)
  
  if root.right:
    res += distributeCoins(root.right)
    root.val += root.right.val - 1
    res += abs(root.right.val - 1)
  
  return res
```
Notes:
- When adding your left child to you, only take child.val - 1, leave 1 for them


## Sorted list to BST
[Link]()
```python
node = None
def sortedListToBST(self, head: ListNode) -> TreeNode:
  # make a BST
  if not head:
    return None
  size = 0
  runner = head
  self.node = head
  while runner is not None:
    runner = runner.next
    size += 1

  def inorderHelper(start, end):
    if (start > end):
      return None
    
    mid = start + (end - start) // 2
    left = inorderHelper(start, mid-1)
    
    newNode = TreeNode(self.node.val)
    newNode.left = left
    self.node = self.node.next
    
    right = inorderHelper(mid+1, end)
    newNode.right = right
    
    return newNode
  
  return inorderHelper(0, size-1)
```
Notes:
- Use same recursive technique as above, add to tree in inorder fashion
- left subtree is `0..mid-1`, right subtree is `mid+1..end`

## Serialize/Deserialize Binary Tree
[Link](https://leetcode.com/problems/serialize-and-deserialize-binary-tree/)
```python
def serialize(root):
  def helper(node):
    if not node: return "#"
    return ",".join([str(root.val), helper(node.left), helper(node.right)])
  return helper(node)

def deserialize(data):
  def helper(vals):
    val = next(vals)
    if val == "#":
      return None
    node = TreeNode(val)
    node.left = helper(vals)
    node.right = helper(vals)
    return node
  
  vals = iter(data.split(","))
  return helper(vals)
```
Notes:
- Use an `iter` to iterate over a list easier
- Simple and elegant

## Find Duplicate Subtrees
[Link](https://leetcode.com/problems/find-duplicate-subtrees/)
```python
def findDuplicateSubtrees(root):
  res = []
  dp = {}
  def postorder(root):
    if not root: return "#"
    serial = ",".join([str(root.val), postorder(root.left), postorder(root.right)])

    # check if seen before
    if dp.get(serial, 0) == 1:
      res.append(root)
    
    # mark that we've seen this
    dp[serial] = dp.get(serial, 0) + 1
    return serial
  postorder(root)
  return res
```
Notes:
- Postorder because we process after `root.right`
- We have a fresh serial string for each root node, we can check if we've seen that tree before

## Lowest Common Ancestor (LCA)
[Link](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/submissions/)
```python
def LCA(root, p, q):
  if root in (None, p, q): return root
  left = LCA(root.left, p, q)
  right = LCA(root.right, p, q)

  if not left: return right
  if not right: return left
  return root

# iterative
def LCA(root, p, q):
  stk = [root]
  parent = { root : None }

  # goes down from root, and stops when p and q are in parents
  while p not in parent or q not in parent:
    node = stk.pop()
    if node.left:
      parent[node.left] = node
      stk.append(node.left)
    if node.right:
      parent[node.right] = node
      stk.append(node.right)

  # now we just find the common ancestors
  ancestors = set()

  # walk up from p, add all its ancestors
  while p:
    ancestors.add(p)
    p = parent[p]

  # q must exist in p's ancestors, stop and return as soon as q find it
  while q not in ancestors:
    q = parent[q]

  return q
```
Notes:
- When we return `root` at the end, that will travel all the way up of the recursive chain
  - The first instance we have left and right defined, means we found `p` and found `q`


## Symmetric Tree
[Link](https://leetcode.com/problems/symmetric-tree/)
```python
def isSymmetric(root):
  if not root: return True
  def util(left, right):
    if not left and not right: return True
    if not left and right: return False
    if not right and left: return False
    return left.val == right.val and util(left.left, right.right) and util(left.right, right.left)
  return util(root.left, root.right)

# iterative
def isSymmetric(root):
  if not root:
    return True

  def isEquals(left, right):
    if not left or not right:
      return left == right == None
    else:
      return left.val == right.val

  stk = [(root.left, root.right)]
  while stk:
    left, right = stk.pop()
    if not left and not right:
      continue
          
    if not isEquals(left, right):
      return False
    
    stk.append((left.left, right.right))
    stk.append((left.right, right.left))
  return True
```