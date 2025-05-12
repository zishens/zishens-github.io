---
title: "二分查找算法详解（C++ 实现）"
date: 2023-10-20
categories: [算法, C++]
tags: [二分查找, 数组, 模板]
math: true  # 启用数学公式支持
---

## 1. 算法简介
**二分查找（Binary Search）** 是一种在 **有序数组** 中快速查找目标值的高效算法，时间复杂度为 $$O(\log n)$$。

## 2. 基本实现（C++）
```cpp
#include <vector>
using namespace std;

int binarySearch(vector<int>& nums, int target) {
    int left = 0, right = nums.size() - 1;
    while (left <= right) {
        int mid = left + (right - left) / 2;  // 防止溢出
        if (nums[mid] == target) {
            return mid;  // 找到目标
        } else if (nums[mid] < target) {
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }
    return -1;  // 未找到
}
```

## 3. 关键点解析
### (1) 防止整数溢出
- **错误写法**：`mid = (left + right) / 2`  
  当 `left + right > INT_MAX` 时会溢出。
- **正确写法**：`mid = left + (right - left) / 2`

### (2) 循环终止条件
- `while (left <= right)`：确保区间有效（如 `[1, 1]` 仍需检查）。
- 终止时 `left > right`，此时目标不存在。

## 4. 常见变种问题
### (1) 查找第一个等于目标的位置
```cpp
int findFirst(vector<int>& nums, int target) {
    int left = 0, right = nums.size() - 1;
    while (left <= right) {
        int mid = left + (right - left) / 2;
        if (nums[mid] >= target) {
            right = mid - 1;
        } else {
            left = mid + 1;
        }
    }
    return (left < nums.size() && nums[left] == target) ? left : -1;
}
```

### (2) 查找最后一个等于目标的位置
```cpp
int findLast(vector<int>& nums, int target) {
    int left = 0, right = nums.size() - 1;
    while (left <= right) {
        int mid = left + (right - left) / 2;
        if (nums[mid] <= target) {
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }
    return (right >= 0 && nums[right] == target) ? right : -1;
}
```

## 5. 复杂度分析
| 操作       | 时间复杂度 | 空间复杂度 |
|------------|------------|------------|
| 标准二分查找 | $$O(\log n)$$ | $$O(1)$$ |
| 变种问题    | $$O(\log n)$$ | $$O(1)$$ |

## 6. 实战题目
- [704. 二分查找](https://leetcode.cn/problems/binary-search/)（模板题）
- [34. 在排序数组中查找元素的第一个和最后一个位置](https://leetcode.cn/problems/find-first-and-last-position-of-element-in-sorted-array/)（变种应用）
- [35. 搜索插入位置](https://leetcode.cn/problems/search-insert-position/)（边界处理）

## 7. 总结
- **核心思想**：通过区间折半快速缩小搜索范围。
- **适用条件**：
  1. 数组必须有序
  2. 无重复元素或需处理重复边界
- **调试技巧**：打印 `left/right/mid` 的值观察区间变化。

---

**欢迎在评论区讨论相关问题！**  
（如需测试代码，可尝试 [LeetCode 在线编译器](https://leetcode.cn/playground/)）
