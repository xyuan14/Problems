## LinkedList 

https://leetcode.com/problems/add-two-numbers/

# Basic operations: 

1. loop two lists: 
```
while p1 or p2: 
  if p1:
    # operate p1
    p1 = p1.next
  if p2: 
    # operate p2
    p2 = p2.next
```
Tips: 
1.  p1 or p2. if p2 not None, operate, otherwise ignore.

2. create dummy node: 
```
  dummy = ListNode()
  p = dummy
```
