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
    
    
