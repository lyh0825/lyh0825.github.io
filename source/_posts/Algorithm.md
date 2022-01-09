---
title: Algorithm
date: 2021-07-11 20:45:50
tags: Algorithm
---

## 递归

### 1.E跳台阶[:star:]

#### 思路

**【递归思想，类似于斐波那契数列】**

一只青蛙一次可以跳上1级台阶，也可以跳上2级。求该青蛙跳上一个n级的台阶总共有多少种跳法（先后次序不同算不同的结果）。

```python
# method 1 recursion 2**n
def drump(n):
    if n == 1:
        return 1
    elif n == 2:
        return 2
    else:
        return drump(n-1) + drump(n-2)

# method 2 loop n
def jump(number):
    if number < 1:
        return 0

    a, b = 0, 1
    for _ in range(number):
        a, b = b, a + b
    return b
```

## 排序

### 1.M排序

给定一个数组，请你编写一个函数，返回该数组排序后的形式。

```python
>>
return sorted(a)
>>
exchange
bubble->select
ordereder
insert->shell
divide and conquer
merge->quick
```

### 2.M最小的k个数

给定一个数组，找出其中最小的K个数。例如数组元素是4,5,1,6,2,7,3,8这8个数字，则最小的4个数字是1,2,3,4。

- 0 <= k <= input.length <= 10000
- 0 <= input[i] <= 10000

```python
class Solution:
    def GetLeastNumbers_Solution(self, tinput, k):
        # write code here
        temp = sorted(tinput)
        return temp[:k]
# 使用bubble控制遍历次数得出最小的n个数
a = [4, 5, 1, 6, 2, 7, 3, 8]
temp = []
n = 0
for i in range(len(a)-1, len(a)-5, -1):
    for j in range(i):
        if a[j+1] > a[j]:
            a[j], a[j+1] = a[j+1], a[j]
    temp.append(a.pop())
print(temp)
```

## 数组

### 1.E两数之和

#### 思路

**【enurmate(iteration)-->return index--value】**

给出一个整数数组，请在数组中找出两个加起来等于目标值的数，

你给出的函数twoSum 需要返回这两个数字的下标（index1，index2），需要满足 index1 小于index2.。注意：下标是从1开始的假设给出的数组中只存在唯一解

例如：

给出的数组为 {20, 70, 110, 150},目标值为90
输出 index1=1, index2=2

```python
# time complexity O(n^2)
class Solution:
    def twoSum(self , numbers , target ):
        # write code here
        for index1, value1 in enumerate(numbers):
            for index2, value2 in enumerate(numbers):
                if index1 == index2:
                    continue
                elif value1 + value2 == target:
                    return [index1+1, index2+1]
```

### 2.M最长无重复子数组[DP:star:]

#### 思路

**【1.双指针】**

方法一和方法二本质上属于同一个解法

**【2.temp记录:simulation->queue】**

注意使用temp(以list为例)时,所有的测试用例只有再使用slice[i+1:]更新cur_temp时才不会报错,如果直接使用.clear()或者cur_list = []会报错

**思路错误**:不应该直接更新cur=[],应该将相同的元素排出去,类似于队列(queue)

[1 2 3 4 5 1]--...cur...-->[1 2 3 4 5]

'1'-->pop// -->[1 |--> 2 3 4 5 1]

result:-->[2 3 4 5 1]

如果将cur变为[],则结果为:four:

**【3.DynamicProgramming】**

从思路上可以理解为dp【i】与dp【i-1】取max，dp：指当前考虑第i个元素的最大长度与考虑第i-1个元素(不考虑第i个元素)的最长无重复数组

这里设置一个方法def dp_len(temp， ele)用于检测ele是否在temp里，并且更新temp返回更新后的temp和len(temp)用于后续的比较

**题目**

给定一个数组arr，返回arr的最长无重复元素子数组的长度，无重复指的是所有数字都不相同。

子数组是连续的，比如[1,3,5,7,9]的子数组有[1,3]，[3,5,7]等等，但是[1,3,7]不是子数组

```python
# method 1 
# 使用队列queue模拟
a = [1, 2, 3, 4, 1, 5]
long = 0
queue = []
for ele in a:
    while ele in queue:
        queue.pop(0)
    queue.append(ele)
    long = max(long, len(queue))
print(long)

# methond 2
# list记录,再使用slice更新
arr = [2, 2, 3, 4, 8, 99, 3]
long, cur_seq = 0, []
for i in arr:
    if i in cur_seq:
        long = max(long, len(cur_seq))
        start = cur_seq.index(i)
        cur_seq = cur_seq[start + 1:]
    cur_seq.append(i)
print(max(long, len(cur_seq)))

# methond3
# 动态规划
class Solution:
    def maxLength(self, a):
        if len(a) == 1:
            return 1
        dp = [0]*(len(a)+1)
        dp[0] = 0
        cur_list = a[:1]
        for i in range(1, len(a)):
            progress = self.dp_len(cur_list, a[i])
            dp[i] = max(dp[i-1], progress[0])
            cur_list = progress[1]
        return max(dp)
    # update the situation
    def dp_len(self, temp, ele):
        while ele in temp:
            temp.pop(0)
        temp.append(ele)
        return len(temp), temp
```

### 3.M合并两个有序的数组

#### 思路

**[1.先将数据合成到一个数组中,之后对数组进行排序]**

**[2.使用merge sort的思想]**

例1:

A: [1,2,3,0,0,0]，m=3

B: [2,5,6]，n=3

合并过后A为:

A: [1,2,2,3,5,6]

```python
# methond1 [合并列表再排序,时间复杂度高]
class Solution:
    def merge(self , A, m, B, n):
        # write code here
        for i in range(len(B)):
            A[m] = B[i]
            m += 1
        index = len(A)-1
        sign = False
        while not sign:
            sign = True
            for i in range(index):
                if A[i] > A[i+1]:
                    sign = False
                    A[i], A[i+1] = A[i+1], A[i]
            index -= 1
        return A
    
# method2 [利用列表本身已经是排好序的,使用merge思想]
A = [4, 5, 6]
B = [1, 2, 3]
m = 3
n = 3
ls = []
if not A:
    print(B)
if not B:
    print(A)
i = j = 0
while i < m and j < n:
    if A[i] <= B[j]:
        ls.append(A[i])
        i += 1
    else:
        ls.append(B[j])
        j += 1
ls.extend(A[i:])
ls.extend(B[j:])
print(ls)

# method3 [利用merge排序但是思想上首先排最大值，因为对于AB来说都是已经排序号的链表]
A = [4, 5, 6， 0， 0， 0]
B = [1, 2, 3]
m = 3
n = 3
if not A:
    print(B)
if not B:
    print(A)

while m > 0 and n > 0:
    if A[m-1] > B[n-1]:
        A[m+n-1] = A[m-1]
        m -= 1
    else:
        lA[m+n-1] = B[n-1]
        n -= 1
# based on the poroblem, the B alwalys has the remaind ele < == >
#==> m = 0; n>
A[:n] = B[:n]
print(A)
```

## 链表

### 1.E反转链表

#### 思路

<img src="https://cdn.jsdelivr.net/gh/StarFire1226/photos@master/20210711215311.png"  />

**【使用指针进行操作：使用前序赋值法替换】**

输入一个链表，反转链表后，输出新链表的表头。

**示例1**

输入：

```
{1,2,3}
```

返回值：

```
{3,2,1}
```

```python
class ListNode:
    def __init__(self, x):
        self.val = x
        self.next = None
class Solution:
    # 返回ListNode
    def ReverseList(self, pHead):
        pre, cur, nex = None, pHead, None
        while c:
            nex = c.next
            cur.next = pre
            pre = cur
            cur = nex
        return pre
```

### 2.M链表中的节点每K个一组翻转

将给出的链表中的节点每\ k *k* 个一组翻转，返回翻转后的链表
如果链表中的节点数不是\ k *k* 的倍数，将最后剩下的节点保持原样你不能更改节点中的值，只能更改节点本身。

要求空间复杂度 O(1)

例如：

给定的链表是1→2→3→4→5

对于 *k*=2, 你应该返回 2→1→4→3→5

对于 *k*=3, 你应该返回 3→2→1→4→5

```python
# 链表反转
class Solution:
    def reverseKGroup(self , head , k ):
        # write code here
        fast = head        
        for _ in range(k):
            if not fast:
                return head
            fast = fast.next   
        new_head = self.reverse(head, fast)
        # 此时head变成了尾巴
		# tail = head
        head.next = self.reverseKGroup(fast, k)
        return new_head
    
    def reverse(self, head, fast):
        last = None
        p = head
        while p is not fast:
            temp = p.next
            p.next = last
            last = p
            p = temp   
        return last 

# list反转 
temp = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
while True:
    result = []
    n = int(input('reverse number:'))
    if n >= len(temp):
        print('the number is too bigger, pls litter')
        continue
    for i in range(0, len(temp), n):
        if i + n <= len(temp):
            result.extend((temp[i:i+n])[::-1])
        else:
            result.extend(temp[i:])
    print('->'.join('%s' %i for i in result))
```

### 3.E判断链表中是否有环

### 思路

**[快慢指针]**:**如果链表中存在有环,则slow指针必定会与fast指针相遇**

**需要注意的是,控{}和{single}制空链表即如果head和head.next都是None则return None**

![](https://cdn.jsdelivr.net/gh/StarFire1226/photos@master/fast_slow_pointer.gif)

判断给定的链表中是否有环。如果有环则返回true，否则返回false。

你能给出空间复杂度O(1)的解法么？

输入分为2部分，第一部分为链表，第二部分代表是否有环，然后回组成head头结点传入到函数里面。-1代表无环，其他的数字代表有环，这些参数解释仅仅是为了方便读者自测调试

```python
class Solution:
    def hasCycle(self , head ):
        if head is None or head.next is None:
            return False
        slow = head
        fast = head.next
        while fast != slow:
            if fast is None or fast.next is None:
                return False
            fast = fast.next.next
            slow = slow.next
        return True
```



## 字符串

### 1.E匹配括号

**[思路]**

要判断括号的有效性，**左括号必须和右括号相对应**。如果是有效括号，并且他们中间还有括号，那么他们必须也是有效的，所以最简单的一种方式就是使用栈来解决。



我们遍历字符串中的所有字符

**1，如果遇到了左括号，就把对应的右括号压栈（比如遇到了字符'('，就把字符')'压栈）。**
**2，如果遇到了右括号**

- 1）查看栈是否为空，如果为空，说明不能构成有效的括号，直接返回false。
- 2）如果栈不为空，栈顶元素出栈，然后判断出栈的这个元素是否等于这个右括号，如果不等于，说明不匹配，直接返回false。如果匹配，就继续判断字符串的下一个字符。

**3，最后如果栈为空，说明是完全匹配，是有效的括号，否则如果栈不为空，说明不完全匹配，不是有效的括号。**

**题目**

给出一个仅包含字符'(',')','{','}','['和']',的字符串，判断给出的字符串是否是合法的括号序列
括号必须以正确的顺序关闭，"()"和"()[]{}"都是合法的括号序列，但"(]"和"([)]"不合法。

```python
class Solution:
    def isValid(self , s ):
        mapping = {")":"(", "]":"[", "}":"{"}
        stack = []
        for i, char in enumerate(s):
            if char not in mapping:#left
                stack.append(char)
            else:
                if not stack or stack[-1] != mapping[char]:
                    return False
                stack.pop()
                
        return len(stack) == 0
```

## 树

### 1.M遍历二叉树

实现pre-order/in-order/post-order遍历二叉树

```python
class Solution:
    def threeOrders(self , root ):
        # write code here
        self.res = [[],[],[]]
        self.dfs(root)
        return self.res

    def dfs(self,root):
        if not root:
            return
        self.res[0].append(root.val)
        self.dfs(root.left)
        self.res[1].append(root.val)
        self.dfs(root.right)
        self.res[2].append(root.val)
        return
```

### 2.M求二叉树的层序遍历【BFS】

给定一个二叉树，返回该二叉树层序遍历的结果，（从左到右，一层一层地遍历）
例如：
给定的二叉树是{3,9,20,#,#,15,7},

该二叉树层序遍历的结果是
[
[3],
[9,20],
[15,7]
]

```python
class Solution:
    def levelOrder(self , root ):
        # write code here
        #队列解决
        qune, res = [], []
        if not root:
            return res
        qune.append(root)
        while len(qune)!=0:
            n = len(qune)
            temp = []
            for i in range(n):
                node = qune.pop(0)
                temp.append(node.val)
                if node.left:
                    qune.append(node.left)
                if node.right:
                    qune.append(node.right)
            res.append(temp)
        return res
```

### 3.M树的遍历【前序遍历】

#### 思路【使用递归，本质上是树的深度遍历DSF】

输入几组数据，pid为-1的代表根节点。如果数据为非根节点，那么就要搜索此节点直至找到根节点。

|  id   | pid  |
| :---: | :--: |
|   A   |  -1  |
|  A-1  |  A   |
|  A-2  |  A   |
|  A-3  |  A   |
| A-2-1 | A-2  |
| A-2-2 | A-2  |
| A-2-3 | A-2  |

`result: /A /A/A-1 /A/A-2 /A/A-3 /A/A-2/A-2-1 /A/A-2/A-2-2 /A/A-2/A-2-3`

```python
d = {
    "A":"-1",
    "A-1":"A",
    "A-2":"A",
    "A-3":"A",
    "A-2-1":"A-2",
    "A-2-2":"A-2",
    "A-2-3":"A-2"
}

res = []

def func(node):
    # using recursion
    array.append(node[0])
    if node[1] == "-1":
        return
    func((node[1], d[node[1]]))

for ele_tuple in d.items():
    # 每次设置一个temp=[]
    array = []
    func(ele_tuple)
    string = "/".join(array[::-1])
    res.append("/"+string)

for ele in res:
    print(ele)
```

### 4. **按之字形顺序打印二叉树**

**[思路]**

利用栈后入先出的特性，一个栈保存当前层节点，一个栈保存下一层节点。入栈规则如下：

- 当前层为**奇数层**时：下一层为从左到右输出，故从右到左入栈即可。
- 当前层为**偶数层**时：下一层为从右到左输出，故从左到右入栈即可。

----

给定一个二叉树，返回该二叉树的之字形层序遍历，（第一层从左向右，下一层从右向左，一直这样交替）
例如：
给定的二叉树是{1,2,3,#,#,4,5}

该二叉树之字形层序遍历的结果是

[

[1],

[3,2],

[4,5]

]

```python
class Solution:
    def Print(self, pRoot):
        # write code here
        if pRoot == None:
            return []
        # 定义两个栈，一个保存从左到右的打印的层的节点，一个保存从右到左打印的层的节点
        stack1 = []
        stack2 = []
        stack1.append(pRoot)
        res = []
        while stack1 or stack2:
            if stack1:
                tempValue = []
                while stack1:
                    tempNode = stack1.pop()
                    tempValue.append(tempNode.val)
                    if tempNode.left:
                        stack2.append(tempNode.left)
                    if tempNode.right:
                        stack2.append(tempNode.right)
                res.append(tempValue)
            if stack2:
                tempValue = []
                while stack2:
                    tempNode = stack2.pop()
                    tempValue.append(tempNode.val)
                    if tempNode.right:
                        stack1.append(tempNode.right)
                    if tempNode.left:
                        stack1.append(tempNode.left)
                res.append(tempValue)
        return res
```



## 动态规划

### 1.E子数组的最大累加和[:star::star::star:]

#### 思路

![](https://cdn.jsdelivr.net/gh/StarFire1226/photos@master/dp_instance.gif)

给定一个数组arr，返回子数组的最大累加和

例如，arr = [1, -2, 3, 5, -2, 6, -1]，所有子数组中，[3, 5, -2, 6]可以累加出最大的和12，所以返回12.

题目保证没有全为负数的数据

[要求]时间复杂度为O(n)，空间复杂度为O(1)

```python
# method1:暴力解法
class Solution:
    def maxsumofSubarray(self , arr ):
        # write code here
        sum = arr[0]
        presum = 0
        for ele in arr:
            if presum < 0:
                presum = ele
            else:
                presum += ele
            sum = max(presum, sum)
        return sum

# method2:动态规划
class Solution:
    def maxsumofSubarray(self , arr ):
        # write code here
        # dp[i]代表到第i位的时侯,以arr[i]结尾的连续子数组最大累加和
        dp = [0] * len(arr)  # 开辟dp
        res = arr[0]  # 保存最终的结果
        dp[0] = arr[0]  # 初始化
        for i in range(1,len(arr)):
            # 维护dp[i], 如果<0，则舍弃cur
            dp[i] = max(dp[i-1],0) + arr[i]  
            res = max(res,dp[i])  # 每更新一个dp值就更新一下res
        return res

# method3:动态规划 不仅可以return max number 还可以返回子串
a = [1, -2, 3, 5, -2, 6, -1]

dp = [0]*len(a)
dp[0] = a[0]


def fun(index):
    # 相比如method1、2此处更新状态时，也记录了index变化
    if dp[index] <= 0:
        pointer.clear()
        return 0
    pointer.append(index)
    return dp[index]


pointer = []
for i in range(1, len(a)):
    dp[i] = fun(i-1) + a[i]

print(dp)
print(list((a[i] for i in pointer)))
```

### 2.M最短路径和[:star::star::star:]

#### 思路

![](https://cdn.jsdelivr.net/gh/StarFire1226/photos@master/20210716180943.png)

:red_circle:dp[ij] = a[ij]

:green_book:dp[ij] = a[ij] + min(dp[i-1, j], dp[i-1, j+1])

:ribbon:dp[ij] = a[ij] + min(dp[i-1, j], dp[i-1, j-1])

:yellow_heart:dp[ij] = a[ij] + min(dp[i-1, j-1], dp[j-1, j], dp[i-1, j+1])

**【注意边界条件和状态转移】**

从上到下找到最短路径（数字之和最小）,只能往下层走，同时只能走向相邻的节点，例如图中第一排的2只能走向第二排的3、7，第一排的5可以走向第二排的1、7、3

|  1   |  8   |  5   |  2   |
| :--: | :--: | :--: | :--: |
|  4   |  1   |  7   |  3   |
|  3   |  6   |  2   |  9   |

```python
a = [[1, 8, 5, 2],
     [4, 1, 7, 3],
     [3, 6, 2, 9]]
x = len(a)
y = len(a[0])
dp = [[0 for i in range(y)] for j in range(x)]
# 遍历顺序是每行内的每列。所以遍历a中第一行只执行到if，dp中第一行就确定了，然后再确定dp第二行。
# 要注意两个边界条件
for i in range(x):
    for j in range(y):
        if i == 0:
            dp[i][j] = a[i][j]
        elif j == 0:
            dp[i][j] = a[i][j] + min(dp[i-1][j], dp[i-1][j+1])
        elif j == y-1:
            dp[i][j] = a[i][j] + min(dp[i-1][j-1], dp[i-1][j])
        else:
            dp[i][j] = a[i][j] + min(dp[i-1][j-1], dp[i-1][j], dp[i-1][j+1])
            
print(min(dp[-1]))
```

### 3.M最长公共子串

#### 思路

![](https://cdn.jsdelivr.net/gh/StarFire1226/photos@master/8D59E0AEF6D5338A2881FB09B7427DA9.png)

给定两个字符串str1和str2,输出两个字符串的最长公共子串

题目保证str1和str2的最长公共子串存在且唯一。

`len(str1) > len(str2)`

```python
str1, str2 = "1AB2345CD", "12345EF"
l1, l2 = len(str1), len(str2)
dp = [[''] * (l1+1) for _ in range(l2+1)]
dp[0][0] = ''
ans = ''

for i in range(1, l1):
    for j in range(1, l2):
        if str1[i] == str2[j]:
            dp[i][j] = dp[i-1][j-1] + str1[i]
            if len(dp[i][j]) > len(ans):
                ans = dp[i][j]
print(ans)
```

### 4.H最长公共子序列

#### **思路**

LCS可以描述两段文字之间的“相似度”，即它们的雷同程度，从而能够用来辨别抄袭。另一方面，对一段文字进行修改之后，计算改动前后文字的最长公共子序列，将除此子序列外的部分提取出来，这种方法判断修改的部分，往往十分准确。简而言之，百度知道、百度百科都用得上。

若xm=yn(最后一个字符相同)，则：
Xm与Yn的最长公共子序列Zk的最后一个字符必定为xm
zk=xm=yn
LCS(Xm,Yn) = LCS(Xm-1,Yn-1)+xm

若xm≠yn，则：
LCS(Xm,Yn)= max{LCS(Xm-1,Yn),LCS(Xm,Yn-1)}

```python
# 使用递归
def lcs(str1, str2, m, n):
    if m == 0 or n == 0:
        return 0
    if str1[m-1] == str2[n-1]:
        case1 = 1 + lcs(str1, str2, m-1, n-1)
        return case1
    else:
        case2 = max(lcs(str1, str2, m-1, n), lcs(str1, str2, m, n-1))
        return case2
str1 = 'bd'
str2 = 'abcd'
m = len(str1)
n = len(str2)
res = lcs(str1, str2, m, n)
print(res)

# 使用暴力遍历求解
str1, str2 = "1AB2345C6D8", "12345E6F8"
l1, l2 = len(str1), len(str2)
temp = []

for i in range(l1):
    for j in range(l2):
        if str1[i] == str2[j]:
            temp.append(str1[i])

print(temp)
```

### 5.M最长回文子串

**思路**

对于一个字符串，请设计一个高效算法，计算其中最长回文子串的长度。

给定字符串**A**以及它的长度**n**，请返回最长回文子串的长度。

```txt
输入:"abc1234321ab",12
return:7
```

```python
class Solution:
    def getLongestPalindrome(self, A, n):
        # write code here
        dp = [[0]*(n) for _ in range(n)]
         
        maxL = 0
             
        for right in range(0, n):
            for left in range(right,-1,-1):
                if A[left] != A[right]:
                    continue
                if right - left <= 1:
                    dp[right][left] = 1
                else:
                    dp[right][left] = dp[right-1][left+1]
                if dp[right][left]==1 and right-left+1 > maxL:
                    maxL = right-left+1
        return maxL
```

### 6.E求路径

**思路**

![](https://cdn.jsdelivr.net/gh/StarFire1226/photos@master/20210719225203.png)

一个机器人在m×n（col*row）大小的地图的左上角（起点）。机器人每次向下或向右移动。机器人要到达地图的右下角（终点）。可以有多少种不同的路径从起点走到终点？

| 起点 |      |          |
| :--: | ---- | :------: |
|      |      |          |
|      |      |          |
|      |      | **终点** |

对于其它位置来说，到达这个位置有两种情况：

一种是从上面的格子走过来的

另一种是从左边的格子走过来的

所以，我们定义一个𝑚×𝑛大小的二维数组𝑑𝑝

𝑑𝑝[𝑖][𝑗]表示从起点到达第𝑖行第𝑗列的方案数。

先把第一行第一列赋值为1

然后从第二行第二列的元素开始循环

𝑑𝑝[𝑖][𝑗]=𝑑𝑝[𝑖−1][𝑗]+𝑑𝑝[𝑖][𝑗−1]

右下角的dp值就是我们要求的答案

```python
# m, n = row * col
class Solution:
    def uniquePaths(self , m , n ):
        # 创建DP时注意m，n的位置
        dp = [[0]*n for _ in range(m)]
        # 先traverse col 再 traverse row
        for i in range(m):
            for j in range(n):
                if i == 0 or j == 0:
                    dp[i][j] = 1
                else:
                    dp[i][j] = dp[i-1][j] + dp[i][j-1]
        return dp[-1][-1]
```

### 7.E买卖股票的最好时机

假设你有一个数组，其中第*i* 个元素是股票在第*i* 天的价格。
你有一次买入和卖出的机会。（只有买入了股票以后才能卖出）。请你设计一个算法来计算可以获得的最大收益。

```Python
# 使用DynamicProgramming
class Solution:
    def maxProfit(self , prices):
        # write code here
        length = len(prices)
        dp = [0] * length
        for i in range(1, length):
            if dp[i-1] <= 0:
                dp[i] = prices[i] - prices[i - 1]
            else:
                dp[i] = dp[i - 1] + prices[i] - prices[i - 1]
        return max(dp)

# 使用暴力双循环
class Solution:
    def maxProfit(self , prices):
        # write code here
        ans = 0
        for i in range(len(prices)):
            for j in range(len(prices)):
                if prices[j] - prices[i] > 0 and i < j:
                    temp = prices[j] - prices[i]
                    ans = max(ans, temp)
        return ans

# 使用切片
class Solution:
    def maxProfit(self , prices):
        # write code here
        ans = 0
        for i in range(len(prices)-1):
            temp = max(prices[i+1:])
            ans = max(temp - prices[i], ans)
        return ans
```

### 8.M矩阵的最小路径和

**思路**

![](https://cdn.jsdelivr.net/gh/StarFire1226/photos@master/75F64625D66A42FC306D2D970E044011.png)

**题目**

给定一个 n * m 的矩阵 a，从左上角开始每次只能向右或者向下走，最后到达右下角的位置，路径上所有的数字累加起来就是路径和，输出所有的路径中最小的路径和。

|  1   |  3   |  5   |  9   |
| :--: | :--: | :--: | :--: |
|  8   |  1   |  3   |  4   |
|  5   |  0   |  6   |  1   |
|  8   |  8   |  4   |  0   |

```python
    def minPathSum(self , matrix ):
        n, m = len(matrix), len(matrix[0])
        
        dp = [[0]*m for _ in range(n)]
        
        for i in range(n):
            for j in range(m):
                if i == 0 and j == 0:
                    dp[i][j] = matrix[0][0]
                elif i == 0 and j != 0:
                    dp[i][j] = dp[i][j-1] + matrix[i][j]
                elif i != 0 and j == 0:
                    dp[i][j] = dp[i-1][j] + matrix[i][j]
                else:
                    dp[i][j] = min(dp[i-1][j], dp[i][j-1]) + matrix[i][j]
        return dp[-1][-1]
```

### 9.M子数组最大乘积

**思路**

使用双dp_max和dp_min来控制如果出现-5*-8的更新

**题目**

给定一个double类型的数组arr，其中的元素可正可负可0，返回子数组累乘的最大乘积。

```txt
输入:[-2.5,4,0,3,0.5,8,-1]
输出:12.00000
```

```PYTHON
arr = [0.8, -0.3, 0.4, -2.0, 3.0]
if len(arr) <= 0:
    print(0)
n = len(arr)
dpmax = [0]*n
dpmin = [0]*n
dpmax[0] = arr[0]
dpmin[0] = arr[0]
for i in range(1, n):
    if arr[i] < 0:
        dpmax[i] = max(arr[i], arr[i]*dpmin[i-1])
        dpmin[i] = min(arr[i], arr[i]*dpmax[i-1])
    else:
        dpmax[i] = max(arr[i], arr[i]*dpmax[i-1])
        dpmin[i] = min(arr[i], arr[i]*dpmin[i-1])
print(dpmax, dpmin, sep='\n')
maxsum = max(max(dpmax), max(dpmin))
print(maxsum)
>>
[0.8, -0.24, 0.4, 0.24, 3.0]
[0.8, -0.3, -0.12, -2.0, -6.0]
3.0
```

### 10.M最长递增子序列的长度

**【思路】**

[**1.动态规划**]([最长上升子序列 - 最长递增子序列 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/longest-increasing-subsequence/solution/zui-chang-shang-sheng-zi-xu-lie-by-leetcode-soluti/))

**2.贪心算法+二分查找**

原始数组为A, 建立一个辅助数组B, 变量end用来记录B数组末尾元素的下标

遍历A中的所有的元素 x = A[i]

- 如果x > B的末尾元素，则将x追加到B的末尾，end+=1
- 如果x < B的末尾元素，则利用二分查找，寻找B中第一个大于x的元素，并用x进行替换 e.g. x= 4 B=[1,3,5,6] ==> B=[1,3,4,6]

遍历结束之后，B的长度则为最长递增子序列的长度

**Example**

![](https://cdn.jsdelivr.net/gh/StarFire1226/photos@master/20210721134208.png)

**题目**

给定数组arr，设长度为n，输出arr的最长递增子序列。（如果有多个答案，请输出其中 按数值(注：区别于按单个字符的ASCII码值)进行比较的 字典序最小的那个）

```txt
输入：[1,2,8,6,4]
输出：[1,2,4]
statement：
其最长递增子序列有3个，（1，2，8）、（1，2，6）、（1，2，4）其中第三个 按数值进行比较的字典序 最小，故答案为（1，2，4）   
```

```python
# 使用贪婪算法加二分算法
def get_lis_length(arr):
    temp = [arr[0]]
    end = 0

    for i in range(1, len(arr)):
        if arr[i] > temp[end]:
            end += 1
            temp.append(arr[i])
        else:
            pos = binary_search(temp, 0, len(temp), arr[i])
            temp[pos] = arr[i]
    return end + 1

def binary_search(arr, start, end, value):
    l = start
    r = end-1
    while l <= r:
        m = (l + r) //2
        if arr[m] == value:
            return m
        elif arr[m] < value:
            l = m + 1
        else:
            r = m - 1
    return l

# 使用dynamic programming
class Solution:
    def lengthOfLIS(self, nums: List[int]) -> int:
        if not nums:
            return 0
        # 记录长度dp
        dp = []
        for i in range(len(nums)):
            dp.append(1)
            for j in range(i):
                if nums[i] > nums[j]:
                    dp[i] = max(dp[i], dp[j] + 1)
        return max(dp)

arr = [2, 1, 5, 3, 6, 4, 8, 9, 7]
print(get_lis_length(arr))
```

### 10.M求最长递增子序列

**贪心算法+二分优化**

```python
class Solution:
    def LIS(self , arr ):
        # write code here
        import bisect
        a=[]
        dp=[1]*len(arr)
        for i in range(len(arr)):
            j=bisect.bisect_left(a, arr[i])
            if j==len(a):
                a.append(arr[i])
            else:
                a[j]=arr[i]
            dp[i]=j+1
        L=len(a)
        for i in range(len(dp)-1,-1,-1):
            if dp[i]==L:
                a[L-1]=arr[i]
                L-=1
        return a
```

## 二分

## DFS[深度优先搜索]

### 1.H字符串排列

#### 思路

**recursion**

`【定义一个set保持在当前位置已经访问过的字符】`

输入一个字符串,按字典序打印出该字符串中字符的所有排列。例如输入字符串abc,则按字典序打印出由字符a,b,c所能排列出来的所有字符串abc,acb,bac,bca,cab和cba。

输入一个字符串,长度不超过9(可能有字符重复),字符只包括大小写字母。

example

```txt
输入: "ab"
输出:['ab', 'ba']
```

```python
class Solution:
    def Permutation(self, s):
        def bfs(visited, unvisited):
            if len(unvisited) == 0:
                res.append(visited)
                return 
            d = set()
            for i, x in enumerate(unvisited):
                if x not in d:
                    d.add(x)
                    bfs(visited+x, unvisited[:i]+unvisited[i+1:])
        res = []
        bfs('', s)
        return res
```

## 哈希

## 分治

### 1.寻找第k大[待解决]

#### 思路

快排+二分，与快排不同的是，利用二分法每次都减少了一半的不必要排序。
当high=low小于k的时候，在后半部分搜索，
当high=low大于k的时候，在前半部分搜索。

```python
import sys


def findKth(a, start, end, K):
    low, high = start, end
    key = a[start]
    while start < end:
        while start < end and a[end] <= key:
            end -= 1
        a[start] = a[end]
        while start < end and a[start] >= key:
            start += 1
        a[end] = a[start]
    a[start] = key
    if start < K - 1:
        return findKth(a, start + 1, high, K)
    elif start > K - 1:
        return findKth(a, low, start - 1, K)
    else:
        return a[start]


try:
    while (1):
        line = sys.stdin.readline()
        if line == '': break
        lines = line.strip().replace('[', '').replace(']', '').split(',')
        lines = list(map(int, lines))
        n, K = lines[-2], lines[-1]
        val = findKth(lines, 0, n - 1, K)
        print(val)
except:
    pass
```



## 模拟

### 1.M设计LRU缓存结构[待解决]

#### 思路

设计LRU缓存结构，该结构在构造时确定大小，假设大小为K，并有如下两个功能

- set(key, value)：将记录(key, value)插入该结构
- get(key)：返回key对应的value值

[要求]

1. set和get方法的时间复杂度为O(1)
2. 某个key的set或get操作一旦发生，认为这个key的记录成了最常使用的。
3. 当缓存的大小超过K时，移除最不经常使用的记录，即set或get最久远的。

若opt=1，接下来两个整数x, y，表示set(x, y)
若opt=2，接下来一个整数x，表示get(x)，若x未出现过或已被移除，则返回-1
对于每个操作2，输出一个答案

```txt
输入：
[[1,1,1],[1,2,2],[1,3,2],[2,1],[1,4,4],[2,2]],3
返回值：
[1,-1]
复制
说明：
第一次操作后：最常使用的记录为("1", 1)
第二次操作后：最常使用的记录为("2", 2)，("1", 1)变为最不常用的
第三次操作后：最常使用的记录为("3", 2)，("1", 1)还是最不常用的
第四次操作后：最常用的记录为("1", 1)，("2", 2)变为最不常用的
第五次操作后：大小超过了3，所以移除此时最不常使用的记录("2", 2)，加入记录("4", 4)，并且为最常使用的记录，然后("3", 2)变为最不常使用的记录
```

```python
class Solution:
    
    def LRU(self , operators , k ):
        # write code here
        keys = []
        d = {}
        res = []
        for op in operators:
            if op[0] == 1:
                if len(keys) == k:
                    key = keys.pop(0)
                    d.pop(key)
                keys.append(op[1])
                d[op[1]]=op[2]
                
            if op[0] == 2:
                ans = d.get(op[1],-1)
                res.append(ans)
                if ans != -1:
                    keys.remove(op[1])
                    keys.append(op[1])
        return res
```



