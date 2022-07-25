# 第二周 - LeetCode 每日一题

## 1、[27] Remove Element

### 题目描述

Given an array nums and a value `val`, remove all instances of that value **in-place** and return the new length.

Do not allocate extra space for another array, you must do this by modifying the input array in-place with `O(1)` extra memory.

The order of elements can be changed. It doesn't matter what you leave beyond the new length.

**Clarification:**

Center Code Mode.

The function's Params is an array about number, and the other is  that value you need remove. 

The funcion's return is result array's length.



**Example 1:**

**Input:** `nums = [3,2,2,3], val = 3`
**Output:** `2, nums = [2,2]`

> Explanation: Your function should return length = 2, with the first two elements of nums being 2.
> It doesn't matter what you leave beyond the returned length. For example if you return 2 with nums = [2,2,3,3] or nums = [2,2,0,0], your answer will be accepted.



**Example 2:**

**Input:** `nums = [0,1,2,2,3,0,4,2], val = 2`
**Output:** `5, nums = [0,1,4,0,3]`

> Explanation: Your function should return length = 5, with the first five elements of nums containing 0, 1, 3, 0, and 4. Note that the order of those five elements can be arbitrary. It doesn't matter what values are set beyond the returned length.

**Constraints:**

- 0 $\leq$ `nums.length` $\leq$ 100

- 0 $\leq$ `nums[i]` $\leq$ 50

- 0 $\leq$ `val` $\leq$ 100



### **题目解法**

1. 我的解法

```c++
int removeElement(vector<int>& nums, int val) {
    
    for( auto it = nums.begin(); it != nums.end(); ) {
        if( *it == val ) {
            it = nums.erase(it);
        }
        else {
            it++;
        }
    }
    return nums.size();
}
```

主要是使用vector的机制进行删除。

有两个点需要注意。

第一，vector::erase() 函数可以无异常的删除传入的迭代器所指向的数据，并返回新的这个位置的迭代器。

第二，移除数据后，之后的数据会在逻辑上向前移动一位，vector的长度会减少，所以不需要移动迭代器。



2. 题解一：双指针
3. 题解三：双指针优化

https://leetcode-cn.com/problems/remove-element/solution/yi-chu-yuan-su-by-leetcode-solution-svxi/





## 2、[938] Range Sum of BST

### **题目描述：**

Given the `root` node of a binary search tree, return the sum of values of all nodes with a value in the range  $ [ low, high ] $ .



**Example 01:**

![image-20210428163247176](https://hong-not-pic-1258424340.cos.ap-nanjing.myqcloud.com/notepic/20210428163247.png)

> **Input**: root = [10, 5, 15, 3, 7, null, 18], low = 7, high = 15
>
> **Output:** 32

**Example 02:**

![image-20210428163359242](https://hong-not-pic-1258424340.cos.ap-nanjing.myqcloud.com/notepic/20210428163359.png)、

> **Input:** root = [10, 5, 15, 3, 7, 13, 18, 1, null, 6], low = 6, high = 10
>
> **Output:** 23



**Constraints:**

- The number of nodes in the tree is in the range $[1, 2 * 10^4]$.
- $ 1 \leq Node.val \leq 10^5 $
- $ 1 \leq low \leq high \leq 10^5 $
- All Node.val are **unique**.



### 题目解答：

