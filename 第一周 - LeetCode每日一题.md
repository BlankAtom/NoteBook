

# 1. 220-Contains Duplicate Ⅲ

## 题目描述

Given an integer array `nums`and two integers `k` and `t` , return `true` if there are **two distinct indices** in the array such that `abs(nums[i] -nums[j]) <= t` and `abs(i - j) <= k`.

**Example 1:**
> **Input**: nums = [1,2,3,1], k = 3, t = 0
> **Output**: true

**Example 2:**

> **Input**: nums = [1,5,9,1,5,9], k = 2, t = 3
> **Output**: false

**Constraints:**

- 0 $\leq$ `nums.length` $\leq$ 2$\times$10^4^ 
- -2^31^ $\leq$ `nums[i]` $\leq$ 2^32^ -1
- 0  $\leq$ `k`  $\leq$ 10^4^
- 0  $\leq$ `t` $\leq$ 2^31^ -1



## 解题思路

**思路：**

> 序列中的每一个元素 x 的左侧，至多存在有k个元素，如果这k个元素中存在一个元素落在区间`[x - t, x + t]`中，我们就找到了一对符合条件的元素。

对于两个响铃的元素，他们各自的左侧的k个元素中有k-1个是重合的。所有使用滑动窗口思路接替，维护一个大小为k的滑动窗口，每次遍历到元素x时的时候，滑动窗口中包含x前面的最多k个元素，只需要检查窗口中是否存在元素落在区间 `[x-t, x+t]`中即可。



如果使用队列维护滑动窗口的元素，会出现元素无序情况。

那么只能对于每个元素都遍历一遍队列检查是否有元素符合条件。如果数组的长度为n，则使用队列的时间复杂度为`O(nk)`，会超出时间限制。

因此，需要满足如下条件的数据结构：

- 支持添加和删除
- 内部元素有序，支持二分查找。

这样我们就可以快速判断滑动窗口中是否存在元素满足条件。

具体地说，对于元素x，当我们希望判断滑动窗口中是否存在某个数y落在区间`[x-t, x+t]`中，只需要判断滑动窗口中所有大于等于`x-t`的元素中最小元素，它是否等于`x+t`即可。

可以使用有序集合来支持这些操作。



**实现：**

在有序集合中查找大于等于`x-t`的最小元素y，如果 y 存在，且`y <= x + t`，我们就找到一对符合条件的元素，完成检查后，我们将x插入到有序集合中，如果有序集合中的元素数量超过了x，我们将有序集合中最早被插入的元素删除即可。



**注意：**

本题的有序集合无需处理相同元素的情况，这是因为如果有序集合中存在相同元素，则直接返回true。

（具体情况具体分析）

为了防止`int`溢出，有两种处理方式，其一是使用`long`、`long long`。其二，是对查找区间进行限制，使其落在`int`范围内。



**代码：**

```C++
// 专业点说，因该避免过多占用空间的情况
// 也就是尽量不使用long来解决问题
bool containsNearbyAlmostDuplicate(vector<int>& nums, int k, int t) {
    // 获取容器的长度
    int n = nums.size();
    // 创建一个空的集合
    set<int> rec;
    for (int i = 0; i < n; i++) {
        // 获得集合当中，最小的大于等于元素x的数的迭代器
        // 第一次应该是一个空的才对……（实测返回的结果是0
        auto iter = rec.lower_bound(max(nums[i], INT_MIN + t) - t);
        // rec.end() 返回迭代器的值是set的size() （想不到吧）
        // 这里的首先判断iter迭代器和rec.end()的迭代器是否一致，不一致才做之后的比较
        // 第二个比较的是大于等于元素x的数字中最小的元素是否小于元素y+t，是的话返回true
        if (iter != rec.end() && *iter <= min(nums[i], INT_MAX - t) + t) {
            return true;
        }
        // 插入数字，新的元素
        rec.insert(nums[i]);
        // 当集合的大小超过了k，则去掉一个数组中最小序列的数
        if (i >= k) {
            rec.erase(nums[i - k]);
        }
    }
    
    // 遍历结束
    return false;
}

```

