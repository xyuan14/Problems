# Problems
Problems and solutions

##1. Two sums

    https://leetcode.com/problems/two-sum/?tab=Description

#Question: 
> Given an array of integers, return indices of the two numbers such that they add up to a specific target.
>You may assume that each input would have exactly one solution, and you may not use the same element twice.
>Example:
>Given nums = [2, 7, 11, 15], target = 9,
>Because nums[0] + nums[1] = 2 + 7 = 9,
>return [0, 1].

#Soluiton: 
1. Naive Version 

  Two loops, the first one loop throw the list and the second loop check for each value whether it's proper. 
  Time complexity: O(n2)
2. HashMap 

  build a hashmap (K:value,V:index), for each element, check whether the index is target-i. If it is, get the corresponding value.
  Time complexity: (n)
3. Hints:
  1. Why hashmap?
     Algorithm: find the first element(get the value, index) -> find the second element(get the value,index), and check whether the element is the target. A loop takes O(n) to get the value and index, while a hashmap takes O(1) to get these two values.
  2. Why not store the value in advanced?
    because we need two elements and these two are all equals. So if: a+b = target, and b haven't been stored in the hashmap, a will be stored in the hashmap. When loop to b, b will find a, and get the target. So there is no need to store the data in advanced. 

#Code
  ```
public class Solution {
        public int[] twoSum(int[] nums, int target) {
            if(nums.length == 0){
                return null;
            }   
            else{
                int[] result = new int[2];
                HashMap<Integer,Integer> map = new HashMap<Integer,Integer>();
                for(int i = 0;i<nums.length;i++){
                    if(map.containsKey(target-nums[i])){
                        result[1] = i;
                        result[0] = map.get(target-nums[i]);
                        return result;
                    }
                    map.put(nums[i],i);
                }
                return result;
            }
        }
    }
    ```
    
## 2.Add Two Numbers
#Question:

>You are given two non-empty linked lists representing two non-negative integers. The digits are stored in reverse order and each of >their nodes contain a single digit. Add the two numbers and return it as a linked list.
>
>You may assume the two numbers do not contain any leading zero, except the number 0 itself.》
>Input: (2 -> 4 -> 3) + (5 -> 6 -> 4)
>Output: 7 -> 0 -> 8
>
>Subscribe to see which companies asked this question.

#Solution
1. Add two nodes' elements one by one. Be careful about the carry.
    Time Complexity: (n)(n is the length of the longer linked list.)
2. Tips:
    1. There are two possible ways to deal with the "adding procedure" after getting the result:
        1. current.val = value -> create new next node ->current.next = next -> current = current.next
        2. create the next node -> next.val = value -> current.next = next -> current = current.next
        In the first condition, the current points to an empty node in the end, and in the second condition the current points to the end of the linkedlist. So we should use the second way because it's too complex to set the final current pointer as null
        2. How to get the value of setting value and carry value
        Instead of creating new variables and set/ get mode, we can get value like this: (carry+val1+val2)%10, and get carry like this: (carry+val1+val2)/10 
        3. Be careful about the last digit: if carry is 1 in the end, we should create a new node
        
#Code: 
```
        public class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        if(l1 == null){return l2;}
        if(l2 == null){return l1;}
        int carry = 0;
        ListNode head = new ListNode(0);
        ListNode p1 = l1;
        ListNode p2 = l2;
        ListNode cursor = head;
        while(p1!=null || p2 != null){
            int val1,val2;
            if(p1 == null){val1 = 0;}
            else {val1 = p1.val;p1 = p1.next;}
            if(p2 == null){val2 = 0;}
            else {val2 = p2.val;p2 = p2.next;}
            ListNode node = new ListNode((carry+val1+val2)%10);
            carry = (carry+val1+val2)/10;
            cursor.next = node;
            cursor = cursor.next;
        }
        
        if(carry != 0){
            ListNode node = new ListNode(1);
            cursor.next = node;
        }
        head = head.next;
        return head;
    }
}
```     
## 3.Longest SubString Without Repeating Characters

#Question
>Given a string, find the length of the longest substring without repeating characters.
>
>Examples:
>C
>Given "abcabcbb", the answer is "abc", which the length is 3.
>
>Given "bbbbb", the answer is "b", with the length of 1.
>
>Given "pwwkew", the answer is "wke", with the length of 3. Note that the answer must be a substring, "pwke" is a subsequence and not a substring.
    #Solutions
    1. Algorithm: two pointers(i, label), label labels the last un-repeated position. If current substring(label, s[i-1]) contains s[i], move label to the s[r+1] (r is the index of the repeat character s[i])
    2. Time Complexity : O(n)
    3. Solution: 
        1. naive solution: 
        two labels, first labels, second loop
        2. updated version:
        build a hashmap to store the current character and its last appeared position. Once s[i] repeats, update the position and second label's value. 
#Code:
    Naive Version
```
    public class Solution {
    public int lengthOfLongestSubstring(String s) {
        if(s == null || s.equals("")){
            return 0;
        }
        else{
            int max = 1;
            int start = 0;
            int current = 0; 
            char[] chArr = s.toCharArray();
            for(int i = 0;i<s.length();i++){
                int repeatIndex = checkIndex(chArr,start,i-1,chArr[i]);
                if( repeatIndex == -1){
                    current++;
                }
                else{
                    start = repeatIndex+1;
                    if(current>max){
                        max = current;
                    }
                    current = i-start+1;
                }
            }
             if(current>max){
                max = current;
            }
            return max;
        }
    }
    
    public int checkIndex(char[] charr,int start, int end,char target){
        if(start <= end){
            for(int i = start;i<=end;i++){
            if(charr[i] == target){
                return i;
            }
        }
        return -1;
        }
        else{
            return -1;
        }
        
    }
}
```

HashMap:
```
public class Solution {
    public int lengthOfLongestSubstring(String s) {
        if(s == null || s.length() == 0){ return 0; }
            HashMap<Character, Integer> map = new HashMap<Character,Integer>();
            int label = 0;
            int max = 1;
            for(int i = 0;i<s.length();i++){
                char ch = s.charAt(i);
                if(map.containsKey(ch) && label < (map.get(ch)+1)){
                    label = Math.max(label,map.get(ch)+1);
                }
                map.put(ch,i);
                max = Math.max(max,i-label+1);
            }
            return max;
        }
}

```
Tips:
    1. Why use hashmap?
    could save time(O(1) for getting last repeated index)
    2. why label = Math.max(label,map.get(ch)+1)?
    because the label should only move forward(last index of current character may appear before label, which is not what we want)
    
