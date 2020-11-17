# Sliding Window Problems

## Longest Substring w/o Repeating Characters
```python
def fn(s):
  freq = collections.defaultdict(int)
  begin = end = counter = maxLen = 0
  while (end < len(s)):
    
    # if we've already seen this char before...
    if (freq[s[end]] > 0):
      counter += 1
    
    freq[s[end]] += 1
    end += 1

    while (counter > 0):
      # if there's more than one of this character (not unique)
      if (freq[s[begin]] > 1):
        counter -= 1
      
      freq[s[begin]] -= 1
      begin += 1
    
    maxLen = max(maxLen, end-begin)

  return maxLen
```
Notes:
- Scrunch when we've encountered a character more than zero times (now has some repeating characters)
- Un-scrunch when we've encountered a character more than one times

## Minimum Window Substring
[Link](https://leetcode.com/problems/minimum-window-substring/)
```python
def minWindow(s, t):
  freq = collections.defaultdict(int)
  for ch in t:
    freq[ch] += 1

  head = begin = end = 0
  counter = len(t)
  minLen = float('inf')
  while (end < len(s)):
    # if this char belongs to 't'
    if freq[s[end]] > 0:
      counter -= 1
    
    freq[s[end]] -= 1
    end += 1

    while (counter == 0):
      if (end - begin < minLen):
        head = begin
        minLen = end - begin
    
      # if char frequency is matches t perfectly, make invalid
      if freq[s[begin]] == 0:
        counter += 1
    
      freq[s[begin]] += 1
      begin += 1
    
  return "" if minLen == float('inf') else s[head:head+minLen]
```
Notes:
- Scrunch when we've encountered all characters of t
- Unscrunch when character frequency of t is zero (means we're just perfect)
- Move head if there's a new minLen

## Minimum Window Subsequence
[Link](https://leetcode.com/submissions/detail/341499710/)
```python
def minWindow(s, t):
  head = sidx = end = tidx = 0
  minDis = float('inf')
  while sidx < len(s):
    if s[sidx] == t[tidx]:
      # advance to next char of t
      tidx += 1

      # when we've found last letter of T
      if tidx == len(T):
        end = sidx + 1
        
        # optimize going left
        tidx -= 1
        while tidx >= 0:
          while (s[sidx] != t[tidx]):
            # move back into s
            sidx -= 1
          
          # found equal, keep going back for both
          sidx -= 1
          tidx -= 1
        
        # correct for going too far back
        sidx += 1
        tidx += 1

        if (end - sidx < minDis):
          head = sidx
          minDis = end - sidx
    
    # move forward s index
    sidx += 1
  return "" if minDis == float('inf') else s[head:head+minDis]
```
Notes:
- Not every problem requires a counter to scrunch.
- This is really interesting as it carries two pointers. Only start doing something when we've found the last char of T
- This is the minimum window so we need to move the s pointer back until we've found the first character of T (scan right ->  optimize left <-)
- careful not to get lost in the pointer arithmetic!

## Longest Substring with at most K Distinct Characters
[Link](https://leetcode.com/problems/longest-substring-with-at-most-k-distinct-characters/)
```python
def lengthOfLongestSubstringKDistinct(s, k):
  begin = end = counter = longStr = 0
  freq = collections.defaultdict(int)
  while end < len(s):
    # leading up to the scrunch
    if freq[s[end]] == 0:
      counter += 1
    freq[s[end]] += 1
    end += 1
    
    if counter > k:
      run = True
      while run:
        freq[s[begin]] -= 1
        run = freq[s[begin]] > 0
        begin += 1
      
      counter -= 1
    longStr = max(longStr, end-begin)
  return longStr
```
Notes:
- Very similar, only stop scrunching when the frequency for a given character crushes to zero
  - Move begin to whenever that occurs

## find 2 Non-overlapping Sub-arrays w/target sum
[Link](https://leetcode.com/problems/find-two-non-overlapping-sub-arrays-each-with-target-sum/submissions/)
```python
def minSumOfLengths(arr, target):
  n = len(arr)
  best = [float('inf')]*n
  ans = bestSoFar = float('inf')
  end = start = cur_sum = 0
  while end < n:
    # add to sum
    cur_sum += arr[end]

    # scrunch window if we overdo it
    while cur_sum > target:
      cur_sum -= arr[start]
      start += 1

    # make decisions if we hit the target
    if cur_sum == target:
      # check if we've recorded an array who's hit the target before
      if start > 0 and best[start-1] != float('inf'):
        ans = min(ans, best[start-1] + (end-start+1))
      
      # update single best result so far
      bestSoFar = min(bestSoFar, end-start+1)

    # mark along the way
    best[end] = bestSoFar
    end += 1

  return ans if ans != float('inf') else -1
```
Notes:
- See comments