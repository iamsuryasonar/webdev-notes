## Sliding Window
Core Pattern

```
start, end = 0, 0
res=0
while end < length(arr or string):
    do_something(end)
    while decreasing_window_size(start):
        dosomething(start)
        start +=1
    res = max(res, end-start+1) # or min
    end+=1
return res
```


### Longest Substring Without Repeating Characters

Thought process. 
goal : 
    - #1 find longest,
    - #2 without duplicate. 
for 
    - #1 need sliding window,
    - #2 need hashmap (dict).
so use start, end track window left and right. as long as no duplicate, keep moving the end to next, if there is duplicate, move start to right until no duplicate. update the result .

```
def lengthOfLongestSubstring(self, s: str) -> int:
    start, end = 0, 0
    arr = defaultdict(int)
    res=0
    while end < len(s):
        arr[s[end]]+=1
        while start < end and arr.get(s[end], 0) > 1:
            arr[s[start]]-=1
            start +=1
        res = max(res, end-start+1)
        end+=1
    return res
```


## 2 Pointer


- Problem 1
    xxxxxyyyyyzzzzzzzzzz
    |                  |
    left->          <- right

- Problem2
    ssstrrrrrinng1
    |
    pointer1->
    ssttrrringggg2
    |
pointer2->

- Problem3
    sssssstttrrrrring
    |    |
    slow |
        fast

#1 use left and right pointers start from 2 side of array or string , check when need to move left or right ;

#2 use 2 pointers to track 2 different array or string, decide when to move which pointer to next position;

#3 use slow and fast pointer ,each time move more steps on fast pointer than slow , see when they will meet . the classic sample is detect if there is cycle in linked list .

Examples:
- Container With Most Water
- 3 color flag

## Depth First Search

```
def dfs(input, visited): 
     if stop_condition or visited:
          return information to parent
     next_inputs = get_next_inputs()
     for next_input in next_inputs:
         visited |= current
         res = dfs(next_input, visited)
         # visited -= {current}  depends, some issue need to restore the search state
      return information needed by parent
```
so the idea is very simple. pass in whatever needed , check stop_condition (this should be the first thing for every recursive function), save into visited if needed and get next states…


## BFS
```
def bfs():
    q = [start_nodes]
    while q:
        nexts = []
        while q:
            n = q.pop(0)
            result= do_something(n)
            nexts.extend(next_level_nodes(n))
        q = nexts
     return result
```
