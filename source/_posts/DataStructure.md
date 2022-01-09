---
title: DataStructure
date: 2021-04-10 19:09:15
tags: Data Structure
---

# DataStructure

## Part3Queue-Stack

### Use Stack Implement Queue

### stack

- push-->append

- pop-->pop
- peek-->return the new item
- isEmpty-->return Bool
- size-->return len

**入栈**

![](https://cdn.jsdelivr.net/gh/StarFire1226/photos@master/push_stack.gif)

**出栈**

![](https://cdn.jsdelivr.net/gh/StarFire1226/photos@master/pop_stack.gif)

### queue

- enqueue-->append
- dequeue-->pop
- isEmpty-->return Bool
- size-->retuen size

**入队**

![](https://cdn.jsdelivr.net/gh/StarFire1226/photos@master/enqueue.gif)

**出队**

![](https://cdn.jsdelivr.net/gh/StarFire1226/photos@master/dequeue.gif)

### stack-->queue

![](https://cdn.jsdelivr.net/gh/StarFire1226/photos@master/stack_im_queue.gif)

### queue-->stack

![](https://cdn.jsdelivr.net/gh/StarFire1226/photos@master/queue_im_stack.gif)

## Part2Algorithm Analysis

### Four methods for generating list

```python
import timeit

def connect():
	l = []
	for i in range(1000):
		l = l + [i]

def append():
	l = []
	for i in range(1000):
		l.append(i)

def comprehension():
	l = [i for i in range(1000)]
	
def list_range():
	l = list(range(1000))

t1 = timeit.timeit('connect()', 'from __main__ import connect', number = 100000)
print('connect ', t1, 'millisecondes')
```

By running the code 100,000 times to find the time required for each method, we conclude that the best method is `list range() `

O(1) <-- append	O(n) <-- + if the length of list needed connecting is n

## Part1Introduction

### GCD_LCM

GCD: greatest common divisor

Euclid Algorithm:`欧几里得算法`

```python
def gcd(n, m):
	while n % m != 0:
		oldm = m
		oldn = n
		
		m = oldn
		n = oldm % oldn
	return n
```

LCM: least common multiple

```python
def lcm(x, y):
    if x > y:
        greater = x
    else:
        greater = y

    while True:
        if greater % x == 0 and greater % y == 0:
            lcmnum = greater
            break
        greater += 1
    return lcmnum
```

### Set/Collection

有序集合：可以包含任意数量的、各不相同的元素，有序集合的每个元素都关联着一个浮点数格式的分值（score），并且有序集合会按照分值，以从小到大的顺序来排列有序集合中的各个元素。

无序集合：集合（set）是一个无序的不重复元素序列。可以使用大括号 **{ }** 或者 **set()** 函数创建集合，注意：创建一个空集合必须用 **set()** 而不是 **{ }**，因为 **{ }** 是用来创建一个空字典。

集合不含重复元素，有序和无序的区别在于是否可以用score(index)进行排序

`list` `tuple` `string`是有序集合 

`dictionary` `set` 是无序集合

注：set是一个不含key的字典，含有key-value pairs的字典又称为Hash Map

### Sorting Algorithm

![](https://cdn.jsdelivr.net/gh/StarFire1226/photos@master/datastructure.png)

#### 1Bubble Sort

The idea of bubble sort is that in each cycle, the big element goes down and the small one goes up, so that sorting is carried out. The specific process is as follows:

![](https://cdn.jsdelivr.net/gh/StarFire1226/photos@master/bubblesort.gif)

```python
       
def bubble_sort(alist):
        for passnum in range(len(alist) - 1, 1, -1):
        	for i in range(passnum):
            	if alist[i] > alist[i+1]:
                	temp = alist[i]
                	alist[i] = alist[i+1]
                	alist[i+1] = temp
    	return alist

# enhancement bubble sort
def sbubble_sort(lst):

    unsorted_index = len(lst) - 1
    sorted_sign = False

    while not sorted_sign:
        sorted_sign = True
        for i in range(unsorted_index):
            if lst[i] > lst[i+1]:
                sorted_sign = False
                lst[i], lst[i+1] = lst[i+1], lst[i]
        unsorted_index = unsorted_index - 1
```

#### Optimize Bubble(鸡尾酒排序)

```python
def cocktail_sort(iterable):
    '''鸡尾酒排序（英语：Cocktail Sort/Shaker Sort）是冒泡排序的轻微变形，
    它还有很多奇怪的名字，双向冒泡排序 (Bidirectional Bubble Sort)、
    波浪排序 (Ripple Sort)、摇曳排序 (Shuffle Sort)、
    飞梭排序 (Shuttle Sort) 和欢乐时光排序 (Happy Hour Sort)
    参数：可迭代序列
    返回：升序排序后的可迭代序列
    原理：
     1. 对序列向尾部做升序冒泡排序，最大元素沉落尾部。
     2. 对序列向头部做降序冒泡排序，最小元素升到头部。
     3. 交替重复步骤1、2，逐渐缩小无序元素范围，直到没有无序元素。
    时间复杂度：O(n^2)
    稳定性： 稳定
    '''
    for i in range(len(iterable) - 1, 1, -1):
        bubbled = False

        for j in range(i):
            if iterable[j] > iterable[j + 1]:
                iterable[j], iterable[j + 1] = iterable[j + 1], iterable[j]
                bubbled = True

        for j in range(i, 1, -1):
            if iterable[j] < iterable[j - 1]:
                iterable[j], iterable[j - 1] = iterable[j - 1], iterable[j]
                bubbled = True

        if not bubbled:
            break

    return iterable
```



#### 2Select Sort

The idea of select sort is to find the smallest value in the first place in the process of each loop, loop to find other smaller values and place it in the next position, and sort by searching. The process is as follows:

![](https://cdn.jsdelivr.net/gh/StarFire1226/photos@master/SelectSort.gif)

```python
def select_sort(a):
    n = len(a) - 1
    for i in range(n):
        min_index = i
        for j in range(i, n+1):
            if a[j] < a[min_index]:
                min_index = j
        a[i], a[min_index] = a[min_index], a[i]
    return a
```

#### 3Insert Sort

The idea of insert sort is to compare with the previous elements. The big elements are placed at the back and the small ones are placed at the front. The specific process is as follows:

![](https://cdn.jsdelivr.net/gh/StarFire1226/photos@master/InsertSort.gif)

```python
def insert_sort(a):
    n = len(a)
    for i in range(1, n):
        cur_val = a[i]
        pos = i
        while pos > 0 and a[pos - 1] > cur_val:
            a[pos] = a[pos - 1]
            pos -= 1
        a[pos] = cur_val
    return a
```

#### 4ShellSort

Shell sort is another variation of insert sort. The final sort is achieved through interval insert sort and gradually reducing the interval. The process is as follows:

![](https://cdn.jsdelivr.net/gh/StarFire1226/photos@master/ShellSort.gif)

```python
def shell_sort(a):
    n = len(a)
    gap = n // 2
    while gap > 0:
        for i in range(gap):
            gap_insert(a, i, gap)   #有间隔的插入排序
        gap //= 2
    return a
def gap_insert(a, sta, gap):
    for i in range(sta + gap, len(a), gap):
        cur_val = a[i]
        pos = i
        while pos > sta and a[pos - gap] > cur_val:
            a[pos] = a[pos - gap]
            pos -= gap
        a[pos] = cur_val
    return a
            

if __name__ == "__main__":
    a = [90, 5, 83, 42, 12, 15]
    print(shell_sort(a))
```

#### 5MergeSort

Merge sort is based on the idea of divide and conquer. The data to be sorted is divided into two subsequences, the subsequences are sorted, and then the sorted subsequences are merged to achieve the final sorting. The process is as follows:

![](https://cdn.jsdelivr.net/gh/StarFire1226/photos@master/MergeSort.gif)

```python
def merge_sort(a):
    if len(a) <= 1:
        return a
    n = len(a) // 2
    left = merge_sort(a[:n])    #子序列归并排序
    right = merge_sort(a[n:])   
    return merge(left, right)   #合并排好序的子序列
def merge(left, right):
    l, r = 0, 0
    res = []
    while l < len(left) and r < len(right):
        if left[l] < right[r]:
            res.append(left[l])
            l += 1
        else:
            res.append(right[r])
            r += 1
    res.extend(left[l:])
    res.extend(right[r:])
    return res
            

if __name__ == "__main__":
    a = [90, 5, 83, 42, 12, 15]
    print(merge_sort(a))
```

#### 6HeapSort

The idea of heap sort is to create a big root heap, exchange the top position of the heap with the last one, and then create a big root heap, repeat the above operations to achieve sorting, the process is as follows:

![](https://cdn.jsdelivr.net/gh/StarFire1226/photos@master/HeapSort.gif)

```python
def heap_sort(a):
    n = len(a)
    for i in range(n // 2 - 1, -1, -1):
        siftdown(a, i, n - 1)          #建立大根堆
    for j in range(n - 1, 0, -1):
        a[0], a[j] = a[j], a[0]        #交换后，继续建立大根堆
        siftdown(a, 0, j-1)
    return a
def siftdown(a, sta, end):
    root = sta                  #根节点
    while True:
        child = 2 * root + 1    #左孩子节点
        if child > end:
            break
        if child + 1 <= end and a[child] < a[child + 1]:   #存在右孩子节点
            child += 1
        if a[root] < a[child]:                      #维护大根堆
            a[root], a[child] = a[child], a[root]
            root = child
        else:
            break
    return a
            

if __name__ == "__main__":
    a = [90, 5, 83, 42, 12, 15]
    print(heap_sort(a))
```

#### 7QuickSort

The idea of quick sorting is to select a baseline, put the one smaller than the baseline on one side, and put the one larger than the baseline on the other side, and achieve the final sorting by sorting the two parts. The process is as follows:

![](https://cdn.jsdelivr.net/gh/StarFire1226/photos@master/QuickSort.gif)

```python
def quick_sort(a):
    if len(a) <= 1:
        return a
    left = []
    right = []
    base = a.pop()
    for x in a:
        if x < base:
            left.append(x)
        else:
            right.append(x)
    return quick_sort(left) + [base] + quick_sort(right)
            

if __name__ == "__main__":
    a = [90, 5, 83, 42, 12, 15]
    print(quick_sort(a))
```

#### 8CountSort

The idea of count sort is to establish a counter, count the number of times each number appears, and then output the statistical results to achieve the final sort. The process is as follows:

![](https://cdn.jsdelivr.net/gh/StarFire1226/photos@master/CountSort.gif)

```python
def count_sort(a):
    n = len(a)
    max_val = max(a)
    count = [0] * (max_val + 1)
    for i in range(n):
        count[a[i]] += 1
    res = []
    for i in range(max_val + 1):
        for j in range(count[i]):
            res.append(i)
    return res
            
            

if __name__ == "__main__":
    a = [90, 5, 83, 42, 12, 15]
    print(count_sort(a))
```

#### 9BucketSort

The idea of bucket sorting is to put the elements in the corresponding range into the bucket, sort the elements in the bucket, and then take the elements out in order to complete the final sorting. The process is as follows:

![](https://cdn.jsdelivr.net/gh/StarFire1226/photos@master/BucketSort.gif)

```python
def bucket_sort(a,n=100,max_num=10000):
    buckets = [[] for _ in range(n)]   #创建桶
    for x in a:
        i = min(x // (max_num // n) , n - 1)    
        buckets[i].append(x)      #将对应的数据放进桶中
        for j in range(len(buckets[i]) - 1 , 0 ,-1):
            if buckets[i][j] < buckets[i][j - 1]:
                buckets[i][j] , buckets[i][j - 1] = buckets[i][j - 1] , buckets[i][j]
            else:
                break
    result = []
    for bin in buckets:
        result.extend(bin)
    return result


if __name__ == "__main__":
    a = [90, 5, 83, 42, 12, 15]
    print(bucket_sort(a))
```

#### 10RadixSort

The idea of radix sort is to divide integers into different numbers by bit, and compare each number. The specific process is as follows:

![](https://cdn.jsdelivr.net/gh/StarFire1226/photos@master/RadixSort.gif)

```python
def radix_sort(a):
    max_val = max(a)
    it = 0
    while 10 ** it <= max_val:
        buckets = [[] for _ in range(10)]
        for x in a:
            i = (x // (10 ** it)) % 10
            buckets[i].append(x)
        a = [j for i in buckets for j in i]
        it += 1
    return a


if __name__ == "__main__":
    a = [90, 5, 83, 42, 12, 15]
    print(radix_sort(a))
```

