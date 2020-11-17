## Decode String
[Link](https://leetcode.com/submissions/detail/344520535/)
```python
def decodeString(s):
  stk = []
  curNum, curString = 0, ''
  for ch in s:
    if ch == '[':
      stk.append((curString, curNum))
      curString, curNum = '', 0
    elif ch == ']':
      prevString, prevNum = stk.pop()
      curString = prevString + prevNum*curString
    elif ch.isdigit():
      curNum = curNum*10 + int(ch)
    else:
      curString += ch
  
  return curString
```
Notes:
- Use DFS to dive into the bracket parentheses. When you receive a closing bracket, it's time to backtrack. And do something with the latest values you have in the stack.

## Validate Stack Sequences
[Link](https://leetcode.com/problems/validate-stack-sequences/)
```python
def validateStackSequences(pushed, popped):
  stk = []
  pop_idx = 0

  for num in pushed:
    stk.append(num)
    while stk and stk[-1] == popped[pop_idx]:
      stk.pop()
      pop_idx += 1

  return len(stk) == 0
```
Notes:
- Work through the problem by hand first