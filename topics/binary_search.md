# Binary Search Problems

## Search Insert
[Link](https://leetcode.com/problems/search-insert-position/submissions/)
```python
def searchInsert(nums, target):
   lo, hi = 0, len(nums)
   if target > nums[-1]:
     return len(nums)
   if low < nums[0]:
     return 0

    searchIndex = 0
    while lo <= hi:
      mid = lo + (hi - lo) // 2
      if (nums[mid] == target): return mid
      elif (nums[mid] > target): hi = mid-1
      else:
        lo = mid+1
        searchIndex = lo
  return searchIndex
```
Notes:
- Remember `mid` equation

## Split Array Largest Sum
[Link](https://leetcode.com/problems/split-array-largest-sum/submissions/)
```python
def canPartition(target, nums, m):
  numPartitions = 1
  curTotal = 0
  for num in nums:
    curTotal += num
    if curTotal > target:
      numPartitions += 1
      curTotal = num

      # check if violate
      if numPartitions > m:
        return False
  return True

def splitArray(nums, m):
  maxNum, sumNums = max(nums), sum(nums)
  if m == 1: return sumNums

  lo, hi = maxNum, sumNums
  while lo <= hi:
    mid = (lo + hi) // 2
    if canPartition(mid, nums, m):
      hi = mid - 1
    else:
      lo = mid + 1
  return lo
```
Notes:
- We want to smush our answer to the number where we just barely make `m` partitions

## Koko Eating Bananas
[Link](https://leetcode.com/problems/koko-eating-bananas/submissions/)
```python
def canEatAllWithinTime(mid, piles, H):
  curTotal = sum((pile + mid - 1)//mid for pile in piles)
  if curTotal > H:
    return False
  return True

def minEatingSpeed(piles, H):
  lo, hi = 1, max(piles)
  while lo <= hi:
    mid = (lo + hi) // 2
    if canEatAllWithinTime(mid, piles, H):
      hi = mid - 1
    else:
      lo = mid + 1
  return lo
```
Notes:
- Ask yourself how many hours would each pile take me to eat? This indicates the use of the ceiling function.
- Push towards solution which just allows us to pass the number of hours required.

## Minimize Max Distance to Gas Station
[Link](https://leetcode.com/problems/minimize-max-distance-to-gas-station/)
```python
def canPlaceStationsForDistance(mid, stations, K):
  sumCount = sum(math.ceil((stB - stA)/mid)-1 for stA,stB in zip(stations, stations[1:]))
  if sumCount > K:
    return False
  return True

def minmaxGasDist(stations, K):
  lo, hi = 1e-6, stations[-1] - stations[0]
  while lo + 1e-6 < hi:
    mid = (lo + hi) / 2
    if canPlaceStationsForDistance(mid, stations, K):
      hi = mid
    else:
      lo = mid
  
  return hi
```
Notes:
- What is `mid` in this context? It's the guess of distance between stations.
  - Use this guess and determine if this statisfies us to add K gas stations
  - Go between each pair of stations (`zip`), use quick maths to determine this
- Based on if you can make it for your guess, either smush your answer smaller, or guess bigger

## Mountain Array
[Link](https://leetcode.com/problems/find-in-mountain-array/)
```python
def findInMountainArray(target, mountain_arr):
  n = mountain_arr.lenght()
  lo, hi = 0, n-1
  peak = 0

  # find the peak
  while (lo < hi):
    mid = (lo + hi)//2
    if mountain_arr.get(mid) < mountain_arr.get(mid+1):
      lo = peak = mid + 1
    else:
      hi = mid
  
  # find target left of the peak
  lo, hi = 0, peak
  while (lo <= hi):
    mid = (lo + hi)//2
    mid_val = mountain_arr.get(mid)
    if mid_val < target:
      lo = mid + 1
    elif mid_val > target:
      hi = mid - 1
    else:
      return mid
  
  # find target right of the peak
  lo, hi = peak, n-1
  while (lo <= hi):
    mid = (lo + hi)//2
    mid_val = mountain_arr.get(mid)
    if mid_val < target:
      hi = mid - 1
    elif mid_val > target:
      lo = mid + 1
    else:
      return mid
  
  return -1
```

## Search in Rotated Sorted Array
[Link](https://leetcode.com/problems/search-in-rotated-sorted-array/submissions/)
```python
def search(nums, target):
  n = len(nums)
  lo, hi = 0, n-1
  
  # find index of smallest value
  while (lo < hi):
    mid = (lo + hi)//2
    if nums[mid] > nums[hi]:
      lo = mid + 1
    else:
      hi = mid
  
  rot = lo
  lo, hi = 0, n-1
  while (lo <= hi):
    mid = (lo + hi)//2
    realmid = (mid + rot) % n
    if nums[realmid] == target:
      return realmid
    elif nums[realmid] > target:
      hi = mid-1
    else:
      lo = mid+1
  return -1
```
Notes:
- Modulo is powerful, we can use the endpoints as a reference (usually target is the reference)