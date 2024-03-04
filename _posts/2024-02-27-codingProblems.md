---
title: Coding Problems
tags: [Programming]
style: fill
color: secondary
description: FAQs for HW Engineers
---

Goal - [Write programs that not only work but are efficient](/blog/efficientProgramming)

---
# Math Problems
---
## Fibonacci

&nbsp;&nbsp;&nbsp;&nbsp; Fibonacci Sequence - 1,1,2,3,5,8,13,21,34,55,89,144,233,377,610

##### Python Program
```
# Python method for fibonacci sequence with best time complexity
# Time complexity will be O(n) because the loop executes n times.
# Addition takes constant O(1) time complexity

def fibonacci(n):
    x, y = 0, 1
    for i in range(1, n+1):
        x, y = y, x+y
    return y
```

##### Python Testbench
```
print("Fibonnacci Sequence - ")
for i in range(15):
    print(str(fibonnacci(i)) + ", ")
```
---
## Factorial

&nbsp;&nbsp;&nbsp;&nbsp;n! = n\*(n-1)\*(n-2)\*.....\*1

&nbsp;&nbsp;&nbsp;&nbsp;5! = 5\*4\*3\*2\*1 = 120

##### Python Program
```
# Python method for factorial with best time complexity
# Time complexity will be O(n) if multiplication takes O(1), otherwise O(nlogn) if multiplication takes logn.
# Multiplication scales exponentially with no. of bits

def factorial(n):
    	result = 1
    for i in range(1, n+1):	#Range starts with 1 because loop only gets executed when n>0....n+1 is used because range function will stop at n.
        result = result*i
    return result
```

##### Python Testbench
```
for i in range(16):
    print("Factorial of " + str(i) + " is: " + str(factorial(i)))
```
---
# Searching Algorithms
---

Worst-case time complexity will be for an element not in list

## 1. Linear Search - Unordered Data

Starts from the first element and iterates through the list until the element is found.

Time complexity is O(n) because in the worst-case. Don't use this when dataset is large.

Use linear search when the dataset is small and stored in contiguous memory.

##### Python Program
```
def linearSearch(listIn, value):
    for i in range(len(listIn)):
        if listIn[i] == value:
            return i
```

##### Python Testbench
```
inputArray = [5,4,3,2,1,9]
print("Found at position: " + str(linearSearch(inputArray, 1)))
```
---

## 2. Binary Search

Check the middle element in the array, check if that's the element being searched. If the value is less than the middle value, repeat the same process for the lower half of the array.

Time complexity is O(logn). 

More efficient than interpolation or exponential search. Suited for large datasets.

##### Python Program
```
def binarySearch(listIn, value):
    if(value < listIn[0] or value > listIn[len(listIn)-1]):
        return None
    listStart = 0
    listEnd = len(listIn)
    for x in range(len(listIn)//2 + 1):
        i = int((listEnd + listStart)/2)
        if(value == listIn[i]):
            return i
        elif (value < listIn[i]):
            listEnd = i
        else:
            listStart = i
    return None
```
---

## 3. Jump Search

Similar to binary search but the stride is determined by the jump size. 

It performs better than linear search but worse than binary search.

Stride can be determined by also taking square root of the list size.

##### Python Program
```
def jumpSearch(listIn, stride, value):
    if(value < listIn[0] or value > listIn[len(listIn)-1]):
        return None
    listStart = 0
    listEnd = len(listIn)
    linearSearch = 0
    for x in range(len(listIn)//stride + stride + 1):
        if (linearSearch == 0):
            if (listStart+stride > (len(listIn) - 1)):
                i = len(listIn) - 1
            else:
                i = listStart + stride
            if(value == listIn[i]):
                return i
            elif (value < listIn[i]):
                listEnd = i
                linearSearch = 1
            else:
                listStart = i
        else:
            for i in range(stride):
                if(value == listIn[listStart+i]):
                    return listStart+i
```

##### Python Testbench
```
inputArray = [1,2,3,4,5,6]
print("Found at position: " + str(jumpSearch(inputArray, 7, 5)))
```

---

## 4. Interpolation Search

Interpolation search can perform better than Binary search if the values are uniformly distributed.

Calculate the slope of the distribution.

---

## 5. Exponential Search

Find a range by exponentially multiplying the index. Once range is found use Binary search to find the position.

Exponential search has a time complexity of O(log n) for searching in a sorted array of size n, where n is the number of elements in the array. Additionally, it requires O(1) space complexity for storing variables used during the search process.

---

---
# Sorting Algorithms
---

## 1. Merge Sort

Recursively divide an input array into two halves. Then sort the divided arrays. 

It has a time complexity of O(nlogn) and is a popular choice for sorting large datasets.

---

## 2. Insertion Sort

Start from the second element in the array and compare whether it is smaller than any of the elements to its left and insert it there.

It has good space complexity of O(1). But it can have a worst case time complexity of O(n^2) if the array is sorted in reverse.

##### Python Program
```
def insertion_sort(arr):
    n = len(arr)
    for i in range(1, n):
        key = arr[i]
        j = i - 1
        while j >= 0 and arr[j] > key:
            arr[j + 1] = arr[j]
            j -= 1
        arr[j + 1] = key
```

##### Python Program
```
arr = [5, 2, 4, 6, 1, 3]
insertion_sort(arr)
print("Sorted array:", arr)
```
---

## 3. Bubble Sort

Compares adjacent elements in array and swaps them. The last element gets sorted in each iteration of i.

It has good space complexity of O(1). But it can have a worst case time complexity of O(n^2) if the array is sorted in reverse.

##### Python Program
```
def bubble_sort(arr):
    n = len(arr)
    for i in range(n - 1):
        for j in range(0, n - i - 1):
            if arr[j] > arr[j + 1]:
                arr[j], arr[j + 1] = arr[j + 1], arr[j]
```

##### Python Program
```
arr = [5, 2, 4, 6, 1, 3]
bubble_sort(arr)
print("Sorted array:", arr)
```

---

## 4. Quick Sort

It is very efficient.

Average time complexity is O(nlogn) and worst-case time complexity is O(n^2). Choice of improper pivot will lead to bad time complexity.

Space complexity is O(logn) - due to recursive calls on the stack.

##### Python Program
```
def quicksort(arr):
    if len(arr) <= 1:
        return arr
    
    pivot = arr[len(arr) // 2]			# Pivot is the middle element here
    left = [x for x in arr if x < pivot]
    middle = [x for x in arr if x == pivot]
    right = [x for x in arr if x > pivot]
    
    return quicksort(left) + middle + quicksort(right)
```

##### Python Testbench
```
arr = [3, 6, 8, 10, 1, 2, 1]
print(quicksort(arr))
```
---

## 5. Heap Sort

Create a max heap and then iterate through the heap.

Heap sort has a time complexity of O(n log n) in all cases (average, best, and worst cases). It has a space complexity of O(1), making it an in-place sorting algorithm. Heap sort is often used when a stable sorting algorithm with a guaranteed worst-case time complexity is required.

---

## 6. Counting Sort

Count the number of times a given value appears in an array. Calculate the cumulative sum for each element. The cumulative sum should indicate the first position of a given number in the sorted array.

Counting sort has a time complexity of O(n + k), where n is the number of elements in the input array and k is the range of the input (the difference between the maximum and minimum values). It has a space complexity of O(n + k), making it efficient for sorting integers within a relatively small range. However, it's not suitable for sorting elements with a large range or non-integer values.

##### Python Program
```
def counting_sort(arr):
    max_val = max(arr)
    counts = [0] * (max_val + 1)

    # Count occurrences of each element
    for num in arr:
        counts[num] += 1

    # Calculate cumulative sum
    for i in range(1, len(counts)):
        counts[i] += counts[i - 1]

    # Build sorted array
    sorted_arr = [0] * len(arr)
    for num in reversed(arr):
        index = counts[num] - 1
        sorted_arr[index] = num
        counts[num] -= 1

    return sorted_arr
```

##### Python Testbench
```
arr = [4, 2, 2, 8, 3, 3, 1]
sorted_arr = counting_sort(arr)
print("Sorted array:", sorted_arr)
```

---

---
# Using Pointers
---

---
# Bitwise
---

---
# Reference and Acknowledgement
---

[Big O Notation Cheat Sheet](https://www.studysmarter.co.uk/explanations/computer-science/algorithms-in-computer-science/big-o-notation/)
