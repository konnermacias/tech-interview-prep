# Etc Problems

## Two Sum
[Link](https://leetcode.com/problems/two-sum/)
```python
def twoSum(nums, target):
  res = []
  diffs = {}
  for i, num in enumerate(nums):
    if (target-num) in diffs:
      res += diffs[target-num], i
      return res
    
    diffs[num] = i

  return res
```
Note:
- Look backwards as you scan. If we've already encountered the one we're looking for, then use it immediately and stop.

## String Transforms into Another String
[Link](https://leetcode.com/problems/string-transforms-into-another-string/)
```python
def canConvert(str1, str2):
  if str1 == str2: return True
    dp = {}
    for i, j in zip(str1, str2):
      if dp.setdefault(i,j) != j:
        return False
    # confirms there's at least one unusued character
    return len(set(str2)) < 26
```
Notes:
- Confirm you don't get a cycle. Leave `j` in place in case you come back to that character later


## Guess the Word
[Link](https://leetcode.com/problems/guess-the-word/)
```python
def findSecretWord(wordlist, master):
  def match(w1, w2):
    return (sum ch1 == ch2 for ch1, ch2 in zip(w1,w2))
  
  num_matches = 0
  while num_matches < 6:
    count = [collections.Counter(w[i] for w in wordlist) for i in range(6)]
    guess = max(wordlist, key=lambda w: sum(count[i][c] for i,c in enumerate(w)))
    num_matches = master.guess(guess)
    wordlist = [w for w in wordlist if match(guess,w) == num_matches]
```
Notes:
- Gather frequency counts for each character, to help with making a better guess
- Create a word based on choosing the char with highest frequency at each position
- update word list matches whatever master returns, repeat until we eventually get it right.


## Minimum Area Rectangle
[Link](https://leetcode.com/problems/minimum-area-rectangle/)
```python
def minAreaRect(points):
  seen = set()
  res = float('inf)
  for x1, y1 in points:
    for x2, y2 in seen:
      if (x1,y2) in seen and (x2,y1) in seen:
        area = abs(x2-x1) * abs(y2-y1)
        if area and area < res:
          res = area
    seen.add((x1,y1))
  return res if res < float('inf') else 0
```
Notes:
- Once we have the bottom left and top right of the rectanlgle, check if we have seen the side points, then store min area

## Reorganize String
[Link](https://leetcode.com/submissions/detail/349997454/)
```python
def reorganizeString(S):
  a = sorted(sorted(S), key=S.count)
  h = len(S) // 2
  a[1::2], a[::2] = a[:h], a[h:]
  return ''.join(a) * (a[-1] != a[-2])
```
Notes:
- Cool python

## Expressive Words
[Link](https://leetcode.com/problems/expressive-words/)
```python
def expressiveWords(S, words):
  def isStretchy(S, word):
    n, m, j = len(S), len(word), 0
    for i in range(n):
      if j < m and S[i] == word[j]:
        j += 1
      if i > 1 and S[i-2] == S[i-1] and S[i-1] == S[i]:
        continue
      elif i > 0 and i < n-1 and S[i-1] == S[i] and S[i] == S[i+1]:
        continue
      else:
        return False
      
      return j == m
    return sum(isStretch(S, word) for word in words)
```
Notes:
- Use 2 pointers, move word pointer ahead if we get a character match
- As long as we're passing what's going on then we can continue otherwise return False

## Count of Smaller Numbers After Self
[Link](https://leetcode.com/problems/count-of-smaller-numbers-after-self/)
```python
def countSmaller(nums):
  def mergeSort(zNums):
    half = len(zNums) // 2
    if half:
      left, right = mergeSort(zNums[:half]), mergeSort(zNums[half:])
      for i in range(len(zNums))[::-1]:
        if not right or left and left[-1][1] > right[-1][1]:
          res[left[-1][0]] += len(right)
          zNums[i] = left.pop()
        else:
          zNums[i] = right.pop()
    return zNums
  
  res = [0]*len(nums)
  mergeSort(list(enumerate(nums)))
  return res
```
Notes:
- Utilize merge sort to track when a number switches right to the left side