# Problems
Problems and solutions

## 1. Two sums

    https://leetcode.com/problems/two-sum/?tab=Description

# Question: 
> Given an array of integers, return indices of the two numbers such that they add up to a specific target.
>You may assume that each input would have exactly one solution, and you may not use the same element twice.
>Example:
>Given nums = [2, 7, 11, 15], target = 9,
>Because nums[0] + nums[1] = 2 + 7 = 9,
>return [0, 1].

# Soluiton: 
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

# Code
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
# Question:

>You are given two non-empty linked lists representing two non-negative integers. The digits are stored in reverse order and each of >their nodes contain a single digit. Add the two numbers and return it as a linked list.
>
>You may assume the two numbers do not contain any leading zero, except the number 0 itself.》
>Input: (2 -> 4 -> 3) + (5 -> 6 -> 4)
>Output: 7 -> 0 -> 8
>
>Subscribe to see which companies asked this question.

# Solution
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
        
# Code: 
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

# Question
>Given a string, find the length of the longest substring without repeating characters.
>
>Examples:
>C
>Given "abcabcbb", the answer is "abc", which the length is 3.
>
>Given "bbbbb", the answer is "b", with the length of 1.
>
>Given "pwwkew", the answer is "wke", with the length of 3. Note that the answer must be a substring, "pwke" is a subsequence and not a substring.
# Solutions
1. Algorithm: two pointers(i, label), label labels the last un-repeated position. If current substring(label, s[i-1]) contains s[i], move label to the s[r+1] (r is the index of the repeat character s[i])
2. Time Complexity : O(n)
3. Solution: 
    1. naive solution: 
    two labels, first labels, second loop
    2. updated version:
    build a hashmap to store the current character and its last appeared position. Once s[i] repeats, update the position and second label's value. 
    
# Code:
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

# Tips:
    1. Why use hashmap?
    could save time(O(1) for getting last repeated index)
    2. why label = Math.max(label,map.get(ch)+1)?
    because the label should only move forward(last index of current character may appear before label, which is not what we want)
    
## 5. Longest palindromic subString

# Question
>Given a string s, find the longest palindromic substring in s. You may assume that the maximum length of s is 1000.
>
>Example:
>
>Input: "babad"
>
>Output: "bab"
>
>Note: "aba" is also a valid answer.
>Example:
>
>Input: "cbbd"
>
>Output: "bb"

# Solution:
1. For each character, check whether it is a center of palindromic subString.
    1. For character, there are two conditions: 
       1. length is odd: for character i, check i+counter-1, i-counter+1 (counter starts from 0)
       2. length is even: for character i, check i-counter, i+counter+1 (counter starts from 0)
    2. Time complexity: O(n2)
    
# Code
```
public String longestPalindrome(String s) {
            int max =1;
            int start = 0;
            int end = 1;
            for(int i  =0;i<s.length();i++){
                int counterOdd = 0;
                while((i-counterOdd-1)>=0 && (i+counterOdd+1)<=(s.length()-1) && (s.charAt(i-counterOdd-1) == s.charAt(i+counterOdd+1))){
                        counterOdd++;
                }
                if((2*counterOdd+1) > max){
                    max = 2*counterOdd+1;
                    start = i-counterOdd;
                    end = i+counterOdd+1;
                }
                int counterEven = 0;
                while(i-counterEven >=0 && i+counterEven+1<=s.length()-1 && (s.charAt(i-counterEven) == s.charAt(i+counterEven+1))){
                    counterEven++;
                }
                if((2*counterEven) > max){
                    max = 2*counterEven;
                    start = i-counterEven+1 ;
                    end = i+counterEven+1;
                }
                
            }
        return s.substring(start,end);
    }
```

#Tips:
1. Very care about the boundaries: see https://wordpress.com/post/xyuan14.wordpress.com/120
2. the while loop should check the updated statues of variables, but in this condition I mixed with the if condition (no proper way to to break out for nested loop, try to avoid this condition in the future. )

## 9. Palindrome number

# Question

>Determine whether an integer is a palindrome. Do this without extra space.

# Solution:
    1.Naive Solution: reverse integer, compare
    Time Complexity O(n)
    Space Complexity O(n)
    2. Compare the first half part and last half part: x>rev

# Code

Naive Solution:
```
public class Solution {
    public boolean isPalindrome(int x) {
        if(x < 0){return false;}
        else if(x <10) {return true;}
        else{
            int rev = 0;
            int y = x;
            while( x > 0 ){
                if(rev > Integer.MAX_VALUE/10){
                    return false;
                }
                else{
                    rev = rev*10 + x%10;
                    x = x/10;
                }
            }
            if (rev == y){
                return true;
            }
            else{
                return false;
            }
        }
    }
}

```

Better Solution:
```
public class Solution {
    public boolean isPalindrome(int x) {
       if(x<0 || (x!=0 && x%10==0)){return false;}
       int rev = 0;
       while(x > rev){
           rev = rev*10 + x%10;
           x = x / 10;
       }
       return (rev == x || rev/10 == x);
    }
}
```
# Tips:
1. try to get the highest digit of a number but cannot -- acutally no need to get the digit one by one, but we can get the first half of the number

## 78 subsets  :shipit:


>Given a set of distinct integers, nums, return all possible subsets.
>
>Note: The solution set must not contain duplicate subsets.
>
>For example,
>If nums = [1,2,3], a solution is:
>
>[
>  [3],
>  [1],
>  [2],
>  [1,2,3],
>  [1,3],
>  [2,3],
>  [1,2],
>  []
>]

# Solution



## 153 Find minimum in rotated sorted Array  :shipit:

>Suppose an array sorted in ascending order is rotated at some pivot unknown to you beforehand.
>
>(i.e., 0 1 2 4 5 6 7 might become 4 5 6 7 0 1 2).
>
>Find the minimum element.
>
>You may assume no duplicate exists in the array.
>

# Solution:
    1. Naive: 
        1.1 for 4,5,6,7,0,1,2, set pivot as most right
        1.2 from left, in first half part, all elements should be larger than the most right
        1.3 so find the first element that is less than right by using binary search.
        1.4 the minimun should be between this pointer and the last pointer
        1.5 use binary search to find this element.
    2. Improved
    3. JiuZhang
    
# Code

```
public int findMin(int[] nums) {
        if(nums==null || nums.length==0){return -1;}
        if(nums.length==1) {return nums[0];}
        if(nums.length==2) {return Math.min(nums[0],nums[1]);}
        /*Naive Version*/
        /*
          for 4,5,6,7,0,1,2, set pivot as most right
          from left, in first half part, all elements should be larger than the most right
          so find the first element that is less than right by using binary search.
          the minimun should be between this pointer and the last pointer
        */
        
        int start = 0,end = nums.length-1,pivot = nums.length-1;
        while(start +1 < end){
            int mid = start + (end-start)/2;
            if(nums[mid]<nums[mid-1]){
                return nums[mid];
            }
            else if (nums[mid] < nums[pivot]){
                end = mid;
            }
            else{
                start = mid;
            }
        }
        
        return Math.min(nums[start],nums[end]);
    }

```

# Tips:

## 239 Sliding Window Maximum

>Given an array nums, there is a sliding window of size k which is moving from the very left of the array to the very right. You can only >see the k numbers in the window. Each time the sliding window moves right by one position.
>
>For example,
>Given nums = [1,3,-1,-3,5,3,6,7], and k = 3.
>
>Window position                Max
>---------------               -----
>[1  3  -1] -3  5  3  6  7       3
> 1 [3  -1  -3] 5  3  6  7       3
> 1  3 [-1  -3  5] 3  6  7       5
> 1  3  -1 [-3  5  3] 6  7       5
> 1  3  -1  -3 [5  3  6] 7       6
> 1  3  -1  -3  5 [3  6  7]      7
>Therefore, return the max sliding window as [3,3,5,5,6,7].

# Hints
    1. How about using a data structure such as deque (double-ended queue)?
    2. The queue size need not be the same as the window’s size.
    3. Remove redundant elements and the queue should store only elements that need to be considered.

# Solution: 
    1. Data structure: 
        1.1 keep a monotonic decreasing deque(double-ended queue) to store the window (index)
        1.2 an array to store the data
    2. Analysis: 
        2.1 keep a monotonic decreasing deque to store the window(index). all elements in the deque is the candidate for the largest element, and the head (left most) value is the current max value.
        2.2 dequeue all the elements which are less than nums[i]. if the element in the deque is less than the nums[i], that means this element has no chance to be the max value any more: this element will always be polled earlier than nums[i], if stays the same window with nums[i], nums[i] is larger.
        2.3 the head of the deque is the result
    3. Algorithm: loop the array
        3.1 for each slide, check whether the head is less than i-k+1. if so, remove the head.(this element is no longer in the window)
        3.2 dequeue all the elements which are less than nums[i]. enqueue the new element(nums[i]) to the list. 
        3.3 put the head of the deque to the result[i-k+1];

# Code: 
```
public class Solution {
    public int[] maxSlidingWindow(int[] nums, int k) {
        if(nums == null || nums.length == 0) {return new int[0];}
        int[] results = new int[nums.length-k+1];
        LinkedList<Integer> deque = new LinkedList<Integer>();
        
        for(int i = 0 ;i<nums.length;i++){
            // check whether the head is not in the slide
            if(!deque.isEmpty() && deque.peekFirst() == i-k){
                deque.pollFirst();
            }
            // poll all elements less than nums[i]
            while(!deque.isEmpty() && nums[deque.peekLast()] < nums[i]){
                deque.pollLast();
            }
            // enque the i
            deque.offer(i);
            //put the head to the results.
            if(i >= k-1){
                results[i-k+1] = nums[deque.peekFirst()];
            }
        }
        return results;
    }
}

```

# Tips:
    1. deque to deal with the head
    2. use monotonic stack to store all candidates. 

## 271 Encode and Decode Strings
>Design an algorithm to encode a list of strings to a string. The encoded string is then sent over the network and is decoded back to the original list of strings.
>
>Machine 1 (sender) has the function:
>
>string encode(vector<string> strs) {
>  // ... your code
>  return encoded_string;
>}
>Machine 2 (receiver) has the function:
>vector<string> decode(string s) {
>  //... your code
>  return strs;
>}
>So Machine 1 does:
>
>string encoded_string = encode(strs);
>and Machine 2 does:
>
>vector<string> strs2 = decode(encoded_string);
>strs2 in Machine 2 should be the same as strs in Machine 1.
>
>Implement the encode and decode methods.
>
>Note:
>The string may contain any possible characters out of 256 valid ascii characters. Your algorithm should be generalized enough to work on any possible characters.
>Do not use class member/global/static variables to store states. Your encode and decode algorithms should be stateless.
>Do not rely on any library method such as eval or serialize methods. You should implement your own encode/decode algorithm.

# Solutions:
    1. use length:
        1.1 encode: 
            1.1.1 for each string, convert to str.length() + "#" + str. That means for each string, we give it a prefix (length+#)
            1.1.2 append all each string together.
        1.2 decode: loop the string with i
            1.2.1 : use indexOf to get the most recent #'s index
            1.2.2 : calculate the length, ( i to the index before #, convert to integer)
            1.2.3 : get the string with length, after #
            1.2.4 : move i to the next
            
    2. use escaping: 
        2.1 encode: 
            2.1.1 convert all "#" to "##"
            2.1.2 use " # " to label the ending of a string
        2.2 decode: 
            2.2.1: split all string with " # "
            2.2.2: replace all ## with #
            
# Code: 
```
    //length: 
    
    public class Codec {

    // Encodes a list of strings to a single string.
    public String encode(List<String> strs) {
        StringBuilder sb = new StringBuilder();
        for(String str : strs){
            sb.append(str.length() + "#" + str);
        }
        return sb.toString();
    }

    // Decodes a single string to a list of strings.
    public List<String> decode(String s) {
        int i = 0;
        List<String> result = new ArrayList<>();
        while(i<s.length()){
            int shap = s.indexOf('#',i);
            int len = Integer.parseInt(s.substring(i,shap));
            result.add(s.substring(shap+1,shap+len+1));
            i = shap+len+1;
        }
        return result;
    }
}

```

```
// escaping
```

# Tips
1 Be careful about indexOF 
2 More flexible.
## 303 Range Sum Query

>Given an integer array nums, find the sum of the elements between indices i and j (i ≤ j), inclusive.
>
>Given nums = [-2, 0, 3, -5, 2, -1]
>
>sumRange(0, 2) -> 1
>sumRange(2, 5) -> -1
>sumRange(0, 5) -> -3
>
>
>You may assume that the array does not change.
>There are many calls to sumRange function.

# Solution:
    1. Naive: for each query, sum from nums[i] to nums[j]
        1.1 init complexity: O(1)
        1.2 query complexity: O(n)
    2. improved: initiate for pre sum, for query, result = nums[j] - nums[i-1]
        2.1 init complexity: O(n)
        2.2 query complexity: O(1)
# Code

```

public class NumArray {
    
    private int[] sums;
    
    public NumArray(int[] nums) {
        sums = new int[nums.length];
        if(nums.length!=1){
            for(int f = 1;f < sums.length;f++){
                nums[f]+=nums[f-1];
            }
        }
            sums = nums;
    }
    
    public int sumRange(int i, int j) {
        if(i ==0){
            return sums[j];
        }
        else{
            return sums[j] - sums[i-1];
        }
    }
}

/**
 * Your NumArray object will be instantiated and called as such:
 * NumArray obj = new NumArray(nums);
 * int param_1 = obj.sumRange(i,j);
 */
```


# Tips:
1. for these problems (range problem), it's very common to use pre sum.
2. For the trade off - higher complexity for once is much better than for many times.

