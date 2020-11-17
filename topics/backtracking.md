# Backtracking Problems

## Letter combinations of Phone Number
[Link](https://leetcode.com/problems/letter-combinations-of-a-phone-number/)

```python
def util(combos, cur_combo, idx, digits):
  # stopping condition
  if len(cur_combo) == len(digits):
    combos.append(cur_combo)
  else:
    # gather letters to iterate through
    letters = num_to_letters[int(digits[idx])]

    for i in range(len(letters)):
      # use idx + 1 for combinations, not permutations
      util(combos, cur_combo + letters[i], idx + 1, digits)


def letterCombos(digits):
  combos = []
  util(combos, "", 0, digits)
  return combos
```

## Combo Sum I
[Link](https://leetcode.com/problems/combination-sum/)
```python
def util(sln_set, cur_sln, idx, cands, remaining):
  if (remaining < 0): return
  elif (remaining == 0):
    sln_set.append(cur_sln)
  else:
    for i in range(idx, len(cands)):
      # cands[i] can be used infinite times, don't increment i here
      util(sln_set, cur_sln + [cands[i]], i, cands, remaining-cands[i])


def comboSum(candidates, target):
  sln_set = []
  candidates.sort()
  util(sln_set, [], 0, candidates, target)
  return sln_set
```

Notes:
- When building up sum, use a parameter like `remaining` which holds your current progress in route to a target value

## Combo Sum II
[Link](https://leetcode.com/problems/combination-sum-ii/)

New Rule: `cands[i]` can only be used once, sln cannot have duplicates.

```python
def util(sln_set, cur_sln, idx, cands, remaining):
  if (remaining < 0): return
  elif (remaining == 0):
    if cur_sln not in sln_set:
        sln_set.append(cur_sln)
  else:
    for i in range(idx, len(cands)):
      # key piece! 
      if i > idx and cands[i] == cands[i-1]:
        continue
      util(sln_set, cur_sln + [cands[i]], i + 1, cands, remaining-cands[i])
```
Notes:
- sort the cands, this allows you to easily check duplicates `cand[i] == cand[i-1]`
- utilize a `i > idx` to indicate that you're past the original value you were looking at

## Permutations II
[Link](https://leetcode.com/problems/permutations-ii/)

Rule: unique permutations

```python
def util(sln_set, cur_sln, used, nums):
  if len(cur_sln) == len(nums):
    sln_set.append(cur_sln)
  else:
    for i in range(len(nums)):
      if used[i] or (i > 0 and nums[i]==nums[i-1] and not used[i-1]):
        continue
      
      # mark visited
      used[i] = True
      util(sln_set, cur_sln + [nums[i]], used, nums)
      used[i] = False

def permUnique(nums):
  sln_set = []
  used = [False] * len(nums)
  nums.sort()
  util(sln_set, [], used, nums)
  return sln_set
```
Notes:
- Iterate i from 0:len(nums)-1, permutations need all possibilities.
- Avoid duplicate answers by checking if we've used a given idx
- Avoid duplicate answers by checking if nums[i]==nums[i-1] and we haven't touched nums[i-1]
  - This would be repeating ourselves, fails constraint, skip route!
- We mark false after the recursive call. Anything after the recursive call should be backtracking

## Subsets II
```python
def util(sln_set, cur_sln, idx, used, nums):
  sln_set.append(cur_sln)
  for i in range(len(nums)):
    if used[i] or (i > 0 and nums[i] == nums[i-1] and not used nums[i-1]):
      continue
    used[i] = True
    util(...)
    used[i] = False
```
Notes:
- Sometimes there's no base case, just run through all possibilities