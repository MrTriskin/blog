---
title: LeetCode Daily Document
date: 2019-12-13 13:37
tags: LeetCode
---

# Document Policy

To keep practicing and try not to be left behind, I will continue to update my working through **Problems** on [LeetCode](https://leetcode.com/problemset/all/). I will provide my solutions, which usually seems naive, and the **Python3** version solution which gives me innavative insights about coding or from algorithm perspective.

打算在这里记录我的LeetCode佛系刷题生涯，我会在这里贴我自己的解答（python3）以及同语言解答中我认为比较推荐的答案。我本人的代码能力实在是不怎么样，会写出不忍直视的憨批算法，但是无所谓了，我感觉没人会看的，哈哈哈哈哈哈。这篇博客的主要以纪录为主，方便我以后回顾。以上。

# Two Sum [Easy]
```
Given an array of integers, return indices of the two numbers such that they add up to a specific target.

You may assume that each input would have exactly one solution, and you may not use the same element twice.

输入一个list和一个目标和数值target，要求输出list中和(sum)为目标数值的index.
```
**Example:**
```
Given nums = [2, 7, 11, 15], target = 9,

Because nums[0] + nums[1] = 2 + 7 = 9,
return [0, 1].
```
**My Solution:** 

Basically, my approach belongs to **Brute Force** with Time complexity $O(n^2)$ and Space complexity $O(1)$. The instance test runtime is **4932 ms** and memory usage is **13.7 MB**.   
```py
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        for i, n in enumerate(nums):
            for j in range(i+1,len(nums)):
                if n + nums[j] == target:
                    return([i, j])
```
**Recomended Solution:**

By utilizing **Dictionary**, this solution only walk through the whole list once to obtain the answer, which means Time complexity will be $O(n)$ with look up cost $O(1)$ and Space complexity is $O(n)$. The instance test runtime is **48 ms** and memory usage is **14 MB**.
```py
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        h = {}
        for i, num in enumerate(nums):
            n = target - num
            if n not in h:
                h[num] = i
            else:
                return [h[n], i]
```

# Add Two Numbers

```
You are given two non-empty linked lists representing two non-negative integers. The digits are stored in reverse order and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.
```
**Example:**
```
Input: (2 -> 4 -> 3) + (5 -> 6 -> 4)
Output: 7 -> 0 -> 8
Explanation: 342 + 465 = 807.
```
**My Solution:**

The instance solution test runtime is **60 ms** and memory usage is **12.8 MB**.
```py
class Solution:
    def addTwoNumbers(self, l1: ListNode, l2: ListNode) -> ListNode:
        accu = ListNode(0)
        t1 = l1
        t2 = l2
        c = 0
        t = t1.val + t2.val
        if t >= 10:
            accu.val = t % 10
            c += 1
        else:
            accu.val = t
        rn = accu
        t1 = t1.next
        t2 = t2.next
        while t1 != None or t2 != None:
            a = ListNode(0)
            if t1 == None:
                t1v = 0
            else:
                t1v = t1.val
                t1 = t1.next
            if t2 == None:
                t2v = 0
            else:
                t2v = t2.val
                t2 = t2.next
            if c == 1:
                t = t1v + t2v + 1
                c = 0
            else:
                t = t1v + t2v
            if t >= 10:
                a.val = t % 10
                c += 1
            else:
                a.val = t
            rn.next = a
            rn = a

        if c != 0:
            rn.next = ListNode(1)
        return (accu)
```
**Recomended Solution:**

This solution is **recursive** with test runtime **68 ms** and memory usage **12.7 MB**.
```py
class Solution:
    def addTwoNumbers(self, l1: ListNode, l2: ListNode) -> ListNode:
        _ = l1.val + l2.val
        digit, tenth = _ % 10, _ // 10
        answer = ListNode(digit)
        if any((l1.next, l2.next, tenth)):
            l1 = l1.next if l1.next else ListNode(0)
            l2 = l2.next if l2.next else ListNode(0)
            l1.val += tenth
            answer.next = self.addTwoNumbers(l1, l2)    
        return answer
```

# Longest Substring Without Repeating Characters
```
Given a string, find the length of the longest substring without repeating characters.

输入一个string，要求输出str中最长的不重复sub str的长度。
```
**Example:**
```
Input: "abcabcbb"
Output: 3 
Explanation: The answer is "abc", with the length of 3. 
```
**My Solution:**

The instance solution test runtime is **88 ms** and memory usage is **12.8 MB**.

```py
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        lstr = 0
        tmp = {}
        que = []
        for ind, c in enumerate(s):
            if tmp.get(c) == None:
                tmp[c] = ind
                que.append(c)
            else:
                while tmp.get(c) != None and len(que) != 0:
                    del tmp[que.pop(0)]
                tmp[c] = ind
                que.append(c)
            lstr = max(lstr, len(tmp.keys()))
        return(lstr)
```
**Recomended Solution:**

This solution tracked the **start pointer** and the **end pointer** of the longest sub string, with test runtime **52 ms** and memory usage **12.7 MB**. 

```py
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        
        if len(s) == 0:
            return 0
        map = {}
        max_length = start = 0
        
        for i in range(len(s)):
            if s[i] in map and start <= map[s[i]]:
                start = map[s[i]] + 1
                
            else:
                max_length = max(max_length, i - start + 1)
            map[s[i]] = i
        return(max_length)
```

# Longest Palindromic Substring 
```
Given a string s, find the longest palindromic substring in s. You may assume that the maximum length of s is 1000.

输入一个string，要求输出str中最长回文结构，若有多个会问return第一个（扩展到return全部也大同小异）。
```
**Example:**
```
Input: "babad"
Output: "bab"
Note: "aba" is also a valid answer.
```
**My Solution:**

The instance solution test runtime is **3368 ms** and memory usage is **12.7 MB**.

```py
class Solution:
    def longestPalindrome(self, s: str) -> str:
        dic = {}
        longest_palindromic = ""
        for ind, cha in enumerate(s):
            if dic.__contains__(cha):
                for cind in dic[cha]:
                    temp = s[cind:ind+1]
                    is_pali = True
                    if temp[::-1] != temp:
                        is_pali = False
                    if is_pali:
                        pali = temp
                    else:
                        pali = cha
                    if len(longest_palindromic) < len(pali):
                        longest_palindromic = pali
                dic[cha].append(ind)
            else:
                dic[cha] = [ind]
                pali = cha
                if len(longest_palindromic) < len(pali):
                        longest_palindromic = pali
        return(longest_palindromic)
```
**Recomended Solution:**

This approach uses **Dynamic Programming**, with test runtime **4908 ms** and memory usage **21.5 MB**. 

```py
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        
        if len(s) == 0:
            return 0
        map = {}
        max_length = start = 0
        
        for i in range(len(s)):
            if s[i] in map and start <= map[s[i]]:
                start = map[s[i]] + 1
                
            else:
                max_length = max(max_length, i - start + 1)
            map[s[i]] = i
        return(max_length)
```
Runtime **964 ms** and memory usage **12.6 MB**. 
How made winds :D.

优美简洁，又快又好的代码orz。

```py
class Solution:
    def longestPalindrome(self, s: str) -> str:
        p = ''
        for i in range(len(s)):
            p1 = self.get_palindrome(s, i, i+1)
            p2 = self.get_palindrome(s, i, i)
            p = max([p, p1, p2], key=lambda x: len(x))
        return p
    
    def get_palindrome(self, s: str, l: int, r: int) -> str:
        while l >= 0 and r < len(s) and s[l] == s[r]:
            l -= 1
            r += 1
        return s[l+1:r]
```
<!-- # Title
```
description
```
**Example:**
```

```
**My Solution:**

The instance solution test runtime is **60 ms** and memory usage is **12.8 MB**.

```py
```
**Recomended Solution:**

This solution is **recursive** with test runtime **68 ms** and memory usage **12.7 MB**.

```py
``` -->