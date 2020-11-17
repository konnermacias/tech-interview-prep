# Favorite Problems

## Merge K sorted lists
[Link](https://leetcode.com/problems/merge-k-sorted-lists/)


Python:
```python
import heapq

def mergeKLists(lists):
  node = runner = ListNode()
  h = [(n.val, n) for n in lists if n]
  heapq.heapify(h)
  while h:
    v, n = h[0]
    if not n.next:
      heapq.heappop(h)
    else:
      heapq.heapreplace(h, (n.next.val, n.next))
    
    runner.next = n
    runner = runner.next
  
  return node.next
```
Notes
- Create min heap, add initial elms to get started
- Only pop from here when no more elements to step through
- heapreplace does a poppush(), we pop bc we've let `runner` have the node

C++ Tips:
- Use `priority_queue<ListNode*>`


## Longest Continuous Subarray w/abs <= limit
```python
def longestSubarray(nums, limit):
  maxq, minq = [], []
  res = i = 0
  for j, elm in enumerate(nums):
    heapq.heappush(maxq,[-elm, j])
    heapq.heappush(minq,[elm, j])
  
    # while max - min fail constraint
    while (-maxq[0][0] - minq[0][0] > limit):
      i = min(maxq[0][1], minq[1]) + 1
      while maxq[0][1] < i : heapq.heappop(maxq)
      while minq[0][1] < i : heapq.heappop(minq)
    
    res = max(res, j-i+1)
  return res
```
Notes:
- Bring up `i` in variable jumps as `j` marches on

## Find Median from Data Stream
[Link](https://leetcode.com/problems/find-median-from-data-stream/)
```python
def __init__(self):
  self.heaps = [], []

def addNum(self, num):
  smallHalf, bigHalf = self.heaps
  heappush(smallHalf, -heappushpop(bigHalf, num))
  if len(bigHalf) < len(smallHalf):
    heappush(bigHalf, -heappop(smallHalf))

def findMedian(self):
  smallHalf, bigHalf = self.heaps
  if len(bigHalf) > len(smallHalf):
    return float(bigHalf[0])
  return (bigHalf[0]-smallHalf[0])/2.0
```
Notes:
- We want fast access time using not a lot of storage
- Keep track of a smaller half and a big half (50% <= )
- Use a min priority queue (`bigHalf`) and max priority queue (`smallHalf`) to handle the juggling.