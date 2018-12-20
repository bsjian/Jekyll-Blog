---
title: "A General Solution to Backtracking Problems"
layout: post
date: 2018-8-10 19:44
image: /assets/images/markdown.jpg
headerImage: false
tag:
- leetcode
- problems
- Java
category: blog
author: boshengjian
description: A general approach to solve backtracking problems
# jemoji: '<img class="emoji" title=":ramen:" alt=":ramen:" src="https://assets.github.com/images/icons/emoji/unicode/1f35c.png" height="20" width="20" align="absmiddle">'
---

## Summary:

Backtracking is a systematic method to iterate through all the
possible configurations of a search space.


[Wikipedia](https://en.wikipedia.org/wiki/Backtracking) defines Backtracking as - "a general algorithm for finding all (or some) solutions to some computational problems, notably constraint satisfaction problems, that incrementally builds candidates to the solutions, and abandons a candidate ("backtracks") as soon as it determines that the candidate cannot possibly be completed to a valid solution". 

The backtracking algorithm enumerates a set of partial candidates that, in principle, could be completed in various ways to give all the possible solutions to the given problem. 

---

## Contents
- [Formula](#formula)
- [Subsets](#subsets)
- [Subsets-Without-Duplicates](#subsets-without-duplicates)
- [Permutations](#permutations)
- [Permutations-Without-Duplicates](#permutations-without-duplicates)
- [Combination-Sum](#combination-sum)
- [Combination-Sum-Using-an-Element-at-Most-Once](#combination-sum-using-an-element-at-most-once)
- [Palindrome-Partitioning](#palindrome-partitioning)

---

## Formula


The **principal idea** is to construct solutions one component at a time
and evaluate such partially constructed candidates as follows.

- At each step in the backtracking algorithm, we start from a given
partial solution, say, a = (a1; a2; :::; ak), and try to extend it by adding
another element at the end.
- If a partially constructed solution can be extended further without
violating the problem’s constraints, it is done by taking the first
remaining legitimate option for the next component.
- If there is no legitimate option for the next component, no
alternatives for any remaining component need to be considered.
- In this case, the algorithm backtracks to replace the last component of
the partially constructed solution with its next option.

### State-Space Tree

Implementation by constructing a tree of choices being made is called the state-space tree. 

- Its root represents an initial state before the search for a solution begins.
- The nodes of the first level in the tree represent the choices made for the first component
of a solution,
- The nodes of the second level represent the choices for the second component, and so on.

### Several Concepts

**Promising:** A node in a state-space tree is said to be promising if it corresponds to a partially constructed solution that may still lead to a complete solution.
**Non-promising:** If the node will not lead to a solution.

So leaves represent either non-promising dead ends or complete solutions found by the algorithm.

If the current node turns out to be non-promising, the algorithm backtracks to the node’s parent to consider the next possible option for its last component;
if there is no such option, it backtracks one more level up the tree, and so on.

### Pseudo-code

![Pseudocode](/assets/blogs/coping/branch_algo.png)

(X,Y) associated with each node, where X is a partial solution, and Y is the remaining subproblem.

We could use **Depth First Search** to explore all the possible candidates. This solution of the questions below is based on [Issac Chua
](https://leetcode.com/issac3/)'s discussion in LeetCode community, so credit goes to him.

---

## Subsets

Problem Description: 

Given a set of **distinct** integers, nums, return all possible subsets (the power set).

**Note**: The solution set must not contain duplicate subsets.

**Example:**
{% highlight html %}
Input: nums = [1,2,3]
Output:
[
  [],
  [1],
  [1],
  [3],
  [1,2,3],
  [1,3],
  [2,3],
  [1,2],
]
{% endhighlight %}

**Solution:**
``` java
public List<List<Integer>> subsets(int[] nums) {
    List<List<Integer>> list = new ArrayList<>();
    Arrays.sort(nums);
    dfs(list, new ArrayList<>(), nums, 0);
    return list;
}

private void dfs(List<List<Integer>> list , List<Integer> tempList, int [] nums, int start){
    //Once in the function, the current tempList is one possible subset. So we need to 
    //add it in.
    list.add(new ArrayList<>(tempList));
    //We have already add all possible subsets before start.
    //Now, from start to the end of nums is where we haven't discovered yet. 
    //The remaining possible subsets can be divided into two categories: subsets w/ or w/o i-th element.
    //So, we: 1. take the current i, start from i + 1 in next recursion. 
    //        2. don't take current i (remove it), try to take i + 1 and start from i + 2 in recursion 
    //        (Now all possible subsets before i are included)
    //        3. don't take i + 1, try to take i + 2 and start from i + 3 in recursion
    //        (Now all possible subsets before i + 1 are included)
    for(int i = start; i < nums.length; i++){
        tempList.add(nums[i]);
        dfs(list, tempList, nums, i + 1);
        tempList.remove(tempList.size() - 1);
    }
    //Now the construction is complete.
}

```

---

## Subsets Without Duplicates
 
Problem Description: 

Similar to **Subsets**, given a collection of integers that might contain duplicates, nums, return all possible subsets (the power set).

**Note**: The solution set must not contain duplicate subsets.

**Example:**
{% highlight html %}
Input: [1,2,2]
Output:
[
  [2],
  [1],
  [1,2,2],
  [2,2],
  [1,2],
  []
]
{% endhighlight %}

**Solution:**
``` java
public List<List<Integer>> subsetsWithDup(int[] nums) {
    List<List<Integer>> list = new ArrayList<>();
    Arrays.sort(nums);
    dfs(list, new ArrayList<>(), nums, 0);
    return list;
}

private void dfs(List<List<Integer>> list, List<Integer> tempList, int [] nums, int start){
    list.add(new ArrayList<>(tempList));
    for(int i = start; i < nums.length; i++){
    // The only difference is to skip duplicates, note i must > start, otherwise i - 1 will out of boundary
        if(i > start && nums[i] == nums[i-1]) continue; // skip duplicates
        tempList.add(nums[i]);
        dfs(list, tempList, nums, i + 1);
        tempList.remove(tempList.size() - 1);
    }
}

```

---

## Permutations

Problem Description: 

Given a collection of distinct integers, return all possible permutations.

**Example:**
{% highlight html %}
Input: [1,2,3]
Output:
[
  [1,2,3],
  [1,3,2],
  [2,1,3],
  [2,3,1],
  [3,1,2],
  [3,2,1]
]
{% endhighlight %}

**Solution:**
``` java
public List<List<Integer>> permute(int[] nums) {
   List<List<Integer>> list = new ArrayList<>();
   //Since they're distinct elements. Sorting is not needed.
   dfs(list, new ArrayList<>(), nums);
   return list;
}

private void dfs(List<List<Integer>> list, List<Integer> tempList, int [] nums){
  // Different from subset problem, when entering the recursion, tempList cannot be added to results 
  // unless its length has reached nums.lnegth (namely it's a possible permutation)
   if(tempList.size() == nums.length){
      list.add(new ArrayList<>(tempList));
   } else{
    // Also, start is always 0. 
    // Because here, we're not developing from (1 - start) subsets, (those
    // sets can be considered as roots) to (start - nums.length) length subsets.
    // Permutation means we need elements from previous explored domain. 
    // We may drop a certain element when we develop the current tempList.
      for(int i = 0; i < nums.length; i++){ 
        // element already exists, skip
         if(tempList.contains(nums[i])) continue; 
         tempList.add(nums[i]);
         dfs(list, tempList, nums);
         tempList.remove(tempList.size() - 1);
      }
   }
} 

```

---

## Permutations Without Duplicates

Problem Description: 

Given a collection of numbers that **might contain duplicates**, return all possible unique permutations.

**Example:**
{% highlight html %}
Input: [1,1,2]
Output:
[
  [1,1,2],
  [1,2,1],
  [2,1,1]
]
{% endhighlight %}

**Solution:**
``` java
public List<List<Integer>> permuteUnique(int[] nums) {
    List<List<Integer>> list = new ArrayList<>();
    //Sort so that duplicates are consecutive.
    Arrays.sort(nums);
    dfs(list, new ArrayList<>(), nums, new boolean[nums.length]);
    return list;
}

// Our recursion function for this problem requires a boolean variable to check 
// if an element (in a position) is used or not, in case of duplicate elements.
private void dfs(List<List<Integer>> list, List<Integer> tempList, int [] nums, boolean [] used){
    if(tempList.size() == nums.length){
        list.add(new ArrayList<>(tempList));
    } else{
        for(int i = 0; i < nums.length; i++){
          //If i is used, continue. 
          //If i is not used, and nums[i] == nums[i - 1] and i - 1 is not used, continue. 
          //There's a duplicate block i.e. [2,2,2], and we always want to 
          //use the first element in the block first. So that we do not reuse them.
            if(used[i] || i > 0 && nums[i] == nums[i-1] && !used[i - 1]) continue;
            used[i] = true; 
            tempList.add(nums[i]);
            dfs(list, tempList, nums, used);
            used[i] = false; 
            tempList.remove(tempList.size() - 1);
        }
    }
}

```

---

## Combination Sum

Problem Description: 

Given a **set** of candidate numbers (candidates) (without duplicates) and a target number (target), find all unique combinations in candidates where the candidate numbers sums to target.

The same repeated number may be chosen from candidates unlimited number of times.

**Example 1:**
{% highlight html %}
Input: candidates = [2,3,6,7], target = 7,
A solution set is:
[
  [7],
  [2,2,3]
]
{% endhighlight %}

**Example 2:**
{% highlight html %}
Input: candidates = [2,3,5], target = 8,
A solution set is:
[
  [2,2,2,2],
  [2,3,3],
  [3,5]
]
{% endhighlight %}

**Solution:**
``` java
public List<List<Integer>> combinationSum(int[] nums, int target) {
    List<List<Integer>> list = new ArrayList<>();
    Arrays.sort(nums);
    dfs(list, new ArrayList<>(), nums, target, 0);
    return list;
}

// We reduce target each time, thus we have int remain as our argument
private void dfs(List<List<Integer>> list, List<Integer> tempList, int [] nums, int remain, int start){
    if(remain < 0) return;
    // if remain is just 0, then our path is correctly sum to the target.
    // thus add to the answer.
    else if(remain == 0) list.add(new ArrayList<>(tempList));
    else{ 
      // What differentiate this problem from others is that the resources is infinite
      // Which means we could reuse the elements.
      // So we loop start from i, and our recursive function also take i as argument
      // Not i + 1
        for(int i = start; i < nums.length; i++){
            tempList.add(nums[i]);
            dfs(list, tempList, nums, remain - nums[i], i); 
            tempList.remove(tempList.size() - 1);
        }
    }
}
```
Actually here every element can be a neighbor, so the same as **Permutation**, we can just delete the start argument as start is always from 0:

```java
private void dfs(List<List<Integer>> list, List<Integer> tempList, int [] nums, int remain){
    if(remain < 0) return;
    else if(remain == 0) list.add(new ArrayList<>(tempList));
    else{ 
        for(int i = 0; i < nums.length; i++){
            tempList.add(nums[i]);
            dfs(list, tempList, nums, remain - nums[i]); 
            tempList.remove(tempList.size() - 1);
        }
    }
}
```

---

## Combination Sum Using an Element at Most Once

Problem Description: 

Given a collection of candidate numbers (candidates) and a target number (target), find all unique combinations in candidates where the candidate numbers sums to target.

Each number in candidates may only be used once in the combination.

**Note:**

All numbers (including target) will be positive integers.
The solution set must not contain duplicate combinations.

**Example 1:**
{% highlight html %}
Input: candidates = [10,1,2,7,6,1,5], target = 8,
A solution set is:
[
  [1, 7],
  [1, 2, 5],
  [2, 6],
  [1, 1, 6]
]
{% endhighlight %}

**Example 2:**
{% highlight html %}
Input: candidates = [2,5,2,1,2], target = 5,
A solution set is:
[
  [1,2,2],
  [5]
]
{% endhighlight %}
**Solution:**
``` java
public List<List<Integer>> combinationSum2(int[] nums, int target) {
    List<List<Integer>> list = new ArrayList<>();
    Arrays.sort(nums);
    dfs(list, new ArrayList<>(), nums, target, 0);
    return list;
    
}

private void dfs(List<List<Integer>> list, List<Integer> tempList, int [] nums, int remain, int start){
    if(remain < 0) return;
    else if(remain == 0) list.add(new ArrayList<>(tempList));
    else{
      // What's different between this and the last one is that
      // the last one has unlimited resource. It's start is always 0
      // so that it can reuse i 
      // Now, same as Subsets, next recursive call is from i + 1
        for(int i = start; i < nums.length; i++){
            if(i > start && nums[i] == nums[i-1]) continue; // skip duplicates
            tempList.add(nums[i]);
            dfs(list, tempList, nums, remain - nums[i], i + 1);
            tempList.remove(tempList.size() - 1); 
        }
    }
} 
```

---
## Palindrome Partitioning

Problem Description: 

Given a string s, partition s such that every substring of the partition is a palindrome.

Return all possible palindrome partitioning of s.

**Example:**
{% highlight html %}
Input: "aab"
Output:
[
  ["aa","b"],
  ["a","a","b"]
]
{% endhighlight %}

**Solution:**
``` java
public List<List<String>> partition(String s) {
   List<List<String>> list = new ArrayList<>();
   dfs(list, new ArrayList<>(), s, 0);
   return list;
}

public void dfs(List<List<String>> list, List<String> tempList, String s, int start){
  // Here, tempList stores the partitioning results. If start is equal to length
  // Then, we include all chars in s, thus it's a possible answer.
   if(start == s.length())
      list.add(new ArrayList<>(tempList));
   else{
    // Similarity, start increase by one. 0 - start is already explored
      for(int i = start; i < s.length(); i++){
        // If found a non-Palindrome, we don't care so dfs will not be called
         if(isPalindrome(s, start, i)){
            tempList.add(s.substring(start, i + 1));
            dfs(list, tempList, s, i + 1);
            tempList.remove(tempList.size() - 1);
         }
      }
   }
}

public boolean isPalindrome(String s, int low, int high){
  // check if s.subString(low, high) is a Palindrome
} 
```



