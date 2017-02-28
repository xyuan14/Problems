# Problems
Problems and solutions

1. Two sums
https://leetcode.com/problems/two-sum/?tab=Description

Question: 
Given an array of integers, return indices of the two numbers such that they add up to a specific target.
You may assume that each input would have exactly one solution, and you may not use the same element twice.
Example:
Given nums = [2, 7, 11, 15], target = 9,
Because nums[0] + nums[1] = 2 + 7 = 9,

return [0, 1].

  
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
