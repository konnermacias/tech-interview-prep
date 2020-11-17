# Linked List

## Add Two Numbers
[Link](https://leetcode.com/problems/add-two-numbers/)
```python
def addTwoNums(l1, l2):
  dummy = runner = ListNode()
  carry = False
  while (l1 or l2 or carry):
    newValue = 0
    if carry:
      newValue += 1
    
    if l1:
      newValue += l1.val
    if l2:
      newValue += l2.val
    
    carry = (newValue >= 10)
    newValue %= 10

    runner.next = ListNode(newValue)
    runner = runner.next

  return dummy.next
```
Notes:
- Notice we can have a case where we've finished going through l1 and l2 but still have a carry bit.
- Only add mod 10 value


## Palindrome Linked List
[Link](https://leetcode.com/problems/palindrome-linked-list/)
```python
def isPalindrome(self, head: ListNode) -> bool:
  runner = fastRunner = head
  while fastRunner and fastRunner.next:
    fastRunner = fastRunner.next.next
    runner = runner.next
  
  if fastRunner:
    runner = runner.next
  
  runner = self.reverse(runner)
  freshRunner = head
  
  while runner:
    if freshRunner.val != runner.val:
      return False
      
    freshRunner = freshRunner.next
    runner = runner.next
  
  return True

def reverse(self, head):
  prev = None
  while head:
    nxt = head.next
    head.next = prev
    prev = head
    head = nxt
  
  return prev
```
Notes:
- Good idea using a fast runner. We can reverse the list to feel like we're doing a middle out
- Be focused when doing the logic of reversing the list in inch-worm style

## Reverse Nodes in K Group
[Link](https://leetcode.com/problems/reverse-nodes-in-k-group/)
```python
 def reverseKGroup(self, head: ListNode, k: int) -> ListNode:
  runner = head
  count = 0
  while runner and count != k:
    runner = runner.next
    count += 1
  
  # we hit k before the true end of the linked list
  if count == k:
    reversed_runner = self.reverseKGroup(runner, k)
    
    # let's reverse the true list
    while count > 0:
      # let's affect the true head
      nxt = head.next
      head.next = reversed_runner
      reversed_runner = head
      head = nxt
      count -= 1
    
    head = reversed_runner
  return head
```
Notes:
- Please review

## Sort list
[Link](https://leetcode.com/problems/sort-list/submissions/)
```python
def sortList(self, head: ListNode) -> ListNode:
  if not head or not head.next:
    return head
  
  prev = None
  slow_runner = fast_runner = head
  while fast_runner and fast_runner.next:
    prev = slow_runner
    slow_runner = slow_runner.next
    fast_runner = fast_runner.next.next
  
  # found the middle
  prev.next = None
  
  # sort each half
  l1 = self.sortList(head)
  l2 = self.sortList(slow_runner)
  
  return self.merge(l1, l2)
    
def merge(self, l1, l2):
  dummy = ListNode()
  runner = dummy
  
  # create sorted version between the two lists
  while l1 and l2:
    if l1.val < l2.val:
      runner.next = l1
      l1 = l1.next
    else:
      runner.next = l2
      l2 = l2.next
    
    runner = runner.next
      
  if l1:
    runner.next = l1
  if l2:
    runner.next = l2
  
  return dummy.next
```