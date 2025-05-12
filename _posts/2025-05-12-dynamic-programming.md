---
title: "动态规划入门：从斐波那契到背包问题（C++ 实现）"
date: 2023-11-01
categories: [算法, 动态规划]
tags: [DP, 斐波那契, 背包问题, 状态转移]
math: true
---

## 1. 什么是动态规划？
**动态规划（Dynamic Programming, DP）** 是一种通过将问题分解为重叠子问题来优化计算的算法思想，核心是 **记忆化存储** 和 **状态转移方程**。

## 2. 从斐波那契数列开始
### (1) 递归实现（低效）
```cpp
int fib(int n) {
    if (n <= 1) return n;
    return fib(n - 1) + fib(n - 2);  // 存在重复计算
}
```
- 时间复杂度：$$O(2^n)$$（指数级）

### (2) 动态规划优化
```cpp
int fibDP(int n) {
    if (n <= 1) return n;
    vector<int> dp(n + 1);
    dp[0] = 0; dp[1] = 1;
    for (int i = 2; i <= n; i++) {
        dp[i] = dp[i - 1] + dp[i - 2];  // 状态转移方程
    }
    return dp[n];
}
```
- 时间复杂度：$$O(n)$$，空间复杂度：$$O(n)$$

### (3) 空间优化（滚动数组）
```cpp
int fibOptimized(int n) {
    if (n <= 1) return n;
    int prev = 0, curr = 1;
    for (int i = 2; i <= n; i++) {
        int next = prev + curr;
        prev = curr;
        curr = next;
    }
    return curr;
}
```
- 空间复杂度优化至 $$O(1)$$

## 3. DP 解题四步骤
1. **定义状态**：明确 `dp[i]` 表示什么
2. **初始化**：设置初始值（如 `dp[0] = 0`）
3. **状态转移方程**：找出 `dp[i]` 与之前状态的关系
4. **确定结果**：返回 `dp[n]` 或其他终态

## 4. 经典问题：0-1 背包
### 问题描述
给定背包容量 `W` 和物品列表（重量 `weight[i]`，价值 `value[i]`），求能装入的最大价值。

### C++ 实现
```cpp
int knapsack(int W, vector<int>& weight, vector<int>& value) {
    int n = weight.size();
    vector<vector<int>> dp(n + 1, vector<int>(W + 1, 0));
    
    for (int i = 1; i <= n; i++) {
        for (int w = 1; w <= W; w++) {
            if (weight[i - 1] <= w) {
                dp[i][w] = max(
                    dp[i - 1][w],  // 不选当前物品
                    dp[i - 1][w - weight[i - 1]] + value[i - 1]  // 选当前物品
                );
            } else {
                dp[i][w] = dp[i - 1][w];
            }
        }
    }
    return dp[n][W];
}
```
- 时间复杂度：$$O(n \times W)$$  
- 空间优化版（一维数组）：
  ```cpp
  vector<int> dp(W + 1, 0);
  for (int i = 0; i < n; i++) {
      for (int w = W; w >= weight[i]; w--) {  // 逆向遍历
          dp[w] = max(dp[w], dp[w - weight[i]] + value[i]);
      }
  }
  ```

## 5. 动态规划 vs 分治法
| 特性         | 动态规划       | 分治法         |
|--------------|----------------|----------------|
| 子问题       | 重叠           | 独立           |
| 存储方式     | 记忆化         | 无记忆         |
| 典型问题     | 背包、最短路径 | 归并排序、FFT  |

## 6. 常见 DP 问题分类
1. **线性 DP**  
   - [70. 爬楼梯](https://leetcode.cn/problems/climbing-stairs/)
   - [53. 最大子数组和](https://leetcode.cn/problems/maximum-subarray/)
2. **区间 DP**  
   - [516. 最长回文子序列](https://leetcode.cn/problems/longest-palindromic-subsequence/)
3. **树形 DP**  
   - [337. 打家劫舍 III](https://leetcode.cn/problems/house-robber-iii/)

## 7. 调试技巧
- 打印 DP 表格观察状态变化：
  ```cpp
  for (auto& row : dp) {
      for (int val : row) cout << val << " ";
      cout << endl;
  }
  ```
- 使用 [Python Tutor](https://pythontutor.com/cpp.html) 可视化执行过程

## 8. 总结
- **核心思想**：空间换时间，避免重复计算
- **适用条件**：
  1. 最优子结构
  2. 重叠子问题
- **优化方向**：
  - 状态压缩（如滚动数组）
  - 剪枝（提前终止无效状态）

---

**欢迎在评论区分享你的 DP 解题心得！**  
（代码测试推荐 [LeetCode Playground](https://leetcode.cn/playground/)）
