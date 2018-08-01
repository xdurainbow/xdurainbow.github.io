---
layout: post
title: "LeetCode"
date: 2018-08-02 00:55:09 +0800
categories: leetcode
---
# 题目描述
## 1.Two Sum
Given an array of integers, return **indices** of the two numbers such that they add up to a specific target.

You may assume that each input would have **exactly** one solution, and you may not use the same element twice.

Example:

	Given nums = [2, 7, 11, 15], target = 9,
	Because nums[0] + nums[1] = 2 + 7 = 9,
	return [0, 1].

# 题目解析
给定一个整数数组和一个目标值，求解数组中的和为目标值的两个数的索引值。

## 实现1
首先想到最简单的办法就是遍历这个数组，去判断当前遍历到的两个数的和是否为目标值：

``` 
def twoSum(nums, target):
	"""
	:type nums: List[int]
	:type target: int
	:rtype: List[int]
	"""
	for i in range(len(nums)):
		for j in range(i+1, len(nums)):
			if nums[i] + nums[j] == target:
				return [i, j]
```
一遍直接AC。LeetCode上面有个功能，可以查看时间复杂度和空间复杂度以及这道题你击败了多少人。看了下结果：Runtime：`3152ms`，击败了`16.06%`的人。

## 改进思考
由于是最容易想到的最简单的也是最暴力的方法，效率显然是很low的，两个for循环，复杂度直接到了O(n<sup>2</sup>)。再分析下题目，我们有**exactly**一个解，那么如果这个数组是有序的，我们分别从数组的开始（i）和结尾（j）向中间遍历，如果两个数的和大于目标值，则i++，否则j--，直至找到那个唯一的解，这样只遍历了一遍数组，复杂度为O(n)，排序的算法复杂度为O(nlogn)，因此最终复杂度为O(nlogn)，远小于O(n<sup>2</sup>)。

## 实现2
首先将nums**_拷贝_**一份，然后对nums进行排序，按上述方法找到哪两个数的和为目标值，然后在之前备份的数组中查看这两个数值的索引。这里需要注意的是，由于数组中可能有重复值，例如给定数组为[3,3]，目标值为6，按上述方法返回的是[0,0]，而不是[0,1]。因此在找到第一个索引值后，把第一个数改变下，只要不和第二个数相等即可，`这里采取的方法是设置为第二个值+1，见倒数第三行`，这样就不会在求第二个索引值时受影响。

```
import copy

def twoSum(nums, target):
    """
    :type nums: List[int]
    :type target: int
    :rtype: List[int]
    """
    num = copy.deepcopy(nums)
    nums.sort()
    i = 0
    j = len(nums) - 1
    while(nums[i] + nums[j] != target):
        if nums[i] + nums[j] > target:
            j -= 1
        else:
            i += 1
    a = num.index(nums[i])
    num[a] = nums[j] + 1
    b = num.index(nums[j])
    return [a, b]
```

## Python中的拷贝
Python中创建一个对象，把他赋值给另一个对象时，并没有拷贝这个对象，而是拷贝对象的引用。

在下面的例子中以此为基础（结果是在2.7.10版本上得出的）

```
import copy
a = [1, 2, [1, 2]]
```
### 1、直接赋值
只是传递对象的引用，原list改变之后，拷贝的list也会跟着变

```
b = a
a[0] = 0
print a ====> [0, 2, [1, 2]]
print b ====> [0, 2, [1, 2]]
```

### 2、浅拷贝
没有拷贝子对象，原list改变之后，其中的子对象会改变。

这里对a[0]进行改变的时候，b[0]不会跟着变，但是改变a的子对象a[2]的内容时，b是会跟着变的。**注意：如果直接设置a[2] = 0时，b是不会变的，因为这里并不是在处理他的子对象**

```
b = copy.copy(a)
a[0] = 0
a[2][0] = 0
print a ====> [0, 2, [0, 2]]
print b ====> [1, 2, [0, 2]]
```

### 3、深拷贝
包含对象里面的子对象的拷贝，所以原list的任何改变不会影响到拷贝的list

```
b = copy.deepcopy(a)
a[0] = 0
a[2][0] = 0
print a ====> [0, 2, [0, 2]]
print b ====> [1, 2, [1, 2]]
```








