
```markdown
---
title: "快速排序算法详解（C++ 实现与优化）"
date: 2023-10-25
categories: [算法, C++]
tags: [排序算法, 分治, 递归]
math: true
---

## 1. 算法概述
**快速排序（Quick Sort）** 是一种基于分治思想的高效排序算法，平均时间复杂度为 $$O(n \log n)$$，最坏情况下（如数组已有序）退化到 $$O(n^2)$$。

## 2. 基础实现（C++）
### (1) Lomuto 分区方案
```cpp
#include <vector>
using namespace std;

// 分区函数
int partition(vector<int>& nums, int left, int right) {
    int pivot = nums[right];  // 选择最右元素作为基准
    int i = left - 1;         // i 标记小于 pivot 的区间末尾
    for (int j = left; j < right; j++) {
        if (nums[j] < pivot) {
            i++;
            swap(nums[i], nums[j]);
        }
    }
    swap(nums[i + 1], nums[right]);  // 将 pivot 放到正确位置
    return i + 1;
}

// 递归排序
void quickSort(vector<int>& nums, int left, int right) {
    if (left < right) {
        int p = partition(nums, left, right);
        quickSort(nums, left, p - 1);
        quickSort(nums, p + 1, right);
    }
}
```

### (2) Hoare 分区方案（更高效）
```cpp
int partitionHoare(vector<int>& nums, int left, int right) {
    int pivot = nums[left + (right - left) / 2];  // 选择中间值作为基准
    int i = left - 1, j = right + 1;
    while (true) {
        do { i++; } while (nums[i] < pivot);
        do { j--; } while (nums[j] > pivot);
        if (i >= j) return j;
        swap(nums[i], nums[j]);
    }
}
```

## 3. 关键优化策略
### (1) 随机化基准选择
```cpp
int partitionRandom(vector<int>& nums, int left, int right) {
    int random = left + rand() % (right - left + 1);
    swap(nums[random], nums[right]);  // 随机交换到最右
    return partition(nums, left, right);
}
```

### (2) 三数取中法（Median-of-Three）
```cpp
int medianOfThree(vector<int>& nums, int left, int right) {
    int mid = left + (right - left) / 2;
    if (nums[left] > nums[mid]) swap(nums[left], nums[mid]);
    if (nums[left] > nums[right]) swap(nums[left], nums[right]);
    if (nums[mid] > nums[right]) swap(nums[mid], nums[right]);
    swap(nums[mid], nums[right - 1]);  // 将中位数放到倒数第二位置
    return nums[right - 1];
}
```

## 4. 复杂度分析
| 场景       | 时间复杂度 | 空间复杂度（递归栈） |
|------------|------------|----------------------|
| 平均情况   | $$O(n \log n)$$ | $$O(\log n)$$ |
| 最坏情况   | $$O(n^2)$$      | $$O(n)$$       |
| 优化后     | $$O(n \log n)$$ | $$O(\log n)$$ |

## 5. 可视化示例
原始数组：`[3, 6, 8, 10, 1, 2, 1]`

1. 选择基准 `pivot = 2`（实际由分区策略决定）
2. 分区后：
   ```
   [1, 1, 2, 10, 6, 8, 3]  // 2 已就位
   ```
3. 递归排序左右子数组。

## 6. 与归并排序对比
| 特性         | 快速排序       | 归并排序       |
|--------------|----------------|----------------|
| 稳定性       | 不稳定         | 稳定           |
| 额外空间     | $$O(1)$$       | $$O(n)$$       |
| 最坏时间复杂度 | $$O(n^2)$$    | $$O(n \log n)$$|

## 7. 实战题目
- [912. 排序数组](https://leetcode.cn/problems/sort-an-array/)（标准实现）
- [215. 数组中的第K个最大元素](https://leetcode.cn/problems/kth-largest-element-in-an-array/)（快速选择变种）
- [75. 颜色分类](https://leetcode.cn/problems/sort-colors/)（三路分区应用）

## 8. 总结
- **核心思想**：分治 + 分区
- **优化关键**：
  - 随机化基准避免最坏情况
  - 小数组切换插入排序（如 `right - left < 10`）
- **适用场景**：大规模数据、对内存敏感的场景

---

**欢迎在评论区讨论实现细节或性能优化！**  
（代码测试建议使用 [LeetCode Playground](https://leetcode.cn/playground/)）
```

