# Greedy Problems

## Split Array into Consecutive Subsequences
[Link](https://leetcode.com/problems/split-array-into-consecutive-subsequences/)
```python
  remaining = collections.Counter(nums)
  end = collections.Counter()
  for num in nums:
    if not remaining[num]:
      continue
      
    remaining[num] -= 1
    if end[num-1] > 0:
      end[num-1] -= 1
      end[num] += 1
      
    elif remaining[num+1] and remaining[num+2]:
      remaining[num+1] -= 1
      remaining[num+2] -= 1
      end[num+2] += 1
    
    else:
        return False
  return True
```
Notes:
- Realize that each number can either be the beginning or the end of the subsequence at a given iteration.