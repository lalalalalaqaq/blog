## 前缀和

构造前缀和数组

- 原数组：[3,5,2,-2,-4,1]
- 前缀和数组：[0,3,8,10,8,12,13]
- 前缀和数组干什么？
  - 求索引 i -- j 的和
    - 1d：nums[0] - nums[2] = preSum[2+1] - preSum[0]
    - 2d:  mat(x1)(y1) -mat(x2)(y2) = preSum(x2+1)(y2+1) - preSum(x1)(y2+1) - preSum(x2+1)(y1) + preSum(x1)(y1);

```java
// 数组前缀和构建
// 1d
public NumArray(int[] nums) {
    preSum = new int[nums.length + 1];
    for(int i = 1; i < nums.length + 1; i++){
        preSum[i] = preSum[i-1] + nums[i-1];
    }
}

// 2d
public NumMatrix(int[][] matrix) {
    int m = matrix.length, n = matrix[0].length;
    if (m == 0 || n == 0) return;
    // 构造前缀和矩阵
    preSum = new int[m + 1][n + 1];
    for (int i = 1; i <= m; i++) {
        for (int j = 1; j <= n; j++) {
            // 计算每个矩阵 [0, 0, i, j] 的元素和
            preSum[i][j] = preSum[i-1][j] + preSum[i][j-1] 
                			+ matrix[i - 1][j - 1] - preSum[i-1][j-1];
        }
    }
}

```



lc303：[区域和检索 - 数组不可变](https://leetcode-cn.com/problems/range-sum-query-immutable/)

- 简单的前缀和

```java
class NumArray {

    private int[] preSum;

    // 前缀和
    public NumArray(int[] nums) {
        preSum = new int[nums.length + 1];
        for(int i = 1; i < nums.length + 1; i++){
            preSum[i] = preSum[i-1] + nums[i-1];
        }

    }
    
    public int sumRange(int left, int right) {
        return preSum[right + 1] - preSum[left];
    }
}

```



lc304：[二维区域和检索 - 矩阵不可变](https://leetcode-cn.com/problems/range-sum-query-2d-immutable/)

- 二维矩阵前缀和的记录
- 注意各个循环的起始点和终点
- 注意如何获得前缀和
- 注意求x1,y1 x2,y2 内的和的格式

```java
class NumMatrix {
    // preSum[i][j] 记录矩阵 [0, 0, i, j] 的元素和
    private int[][] preSum;

    public NumMatrix(int[][] matrix) {
        int m = matrix.length, n = matrix[0].length;
        if (m == 0 || n == 0) return;
        // 构造前缀和矩阵
        preSum = new int[m + 1][n + 1];
        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                // 计算每个矩阵 [0, 0, i, j] 的元素和
                preSum[i][j] = preSum[i-1][j] + preSum[i][j-1] + matrix[i - 1][j - 1] - preSum[i-1][j-1];
            }
        }
    }

    // 计算子矩阵 [x1, y1, x2, y2] 的元素和
    public int sumRegion(int x1, int y1, int x2, int y2) {
        // 目标矩阵之和由四个相邻矩阵运算获得
        return preSum[x2+1][y2+1] - preSum[x1][y2+1] - preSum[x2+1][y1] + preSum[x1][y1];
    }
}
```



lc560：[和为 K 的子数组](https://leetcode-cn.com/problems/subarray-sum-equals-k/)

- lc303的变种
- 注意和为k的表达

```java
class Solution {
    public int subarraySum(int[] nums, int k) {
        int n = nums.length;
        int[] preSum = new int[n+1];
        preSum[0] = 0;
      
        for(int i = 0; i < n; i++){
            preSum[i+1] = preSum[i]+nums[i];
        }

        int res = 0;

        for(int i = 0; i <= n; i ++){
            for(int j = 0; j < i; j++){
                if(preSum[i] - preSum[j] == k){
                    res++;
                }
            }
        }
        return res;
    }
}
```

