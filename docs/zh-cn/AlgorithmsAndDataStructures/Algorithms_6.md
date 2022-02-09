# 差分数组

> 差分数组和前缀和差不多，一个是存和，一个是求差

### 构造Difference 类

- 初始化diff(差分数组)
- public Difference：根据原始结果构造差分数组
- public void increment：区间加减
- public int[] result：返回结果

```java
class Difference{
    private int[] diff;

    public Difference(int[] nums){
        assert nums.length > 0;
        diff = new int[nums.length];
        //构造差分数组
        diff[0] = nums[0];
        for(int i = 1; i < nums.length - 1; i++){
            diff[i] = nums[i] - nums[i-1];
        }
    }

    public void increment(int i, int j,int val){
        diff[i]+= val;
        if( j + 1 < diff.length){  // 若摸到边界,则是全部添加val
            diff[ j + 1] -= val;
        }
    }

    public int[] result(){
        int[] res = new int[diff.length];
        res[0] = diff[0];
        for(int i = 1; i < diff.length; i++){
            res[i] = res[i-1] + diff[i];

        }
        return res;
    }

}
```



#### [1109. 航班预订统计](https://leetcode-cn.com/problems/corporate-flight-bookings/)

- int[] nums：根据需求建数组
- df：初始化
- for循环：是为了符合要求
- df.result()：返回结果

```java
class Solution {
    public int[] corpFlightBookings(int[][] bookings, int n) {
        int[] nums = new int[n];
        Difference df = new Difference(nums);
        
        for(int[] booking : bookings){
            int i = booking[0];
            int j = booking[1];
            int val = booking[2];
            df.increment(i-1,j-1,val);
        }

        return df.result();

    }
}

class Difference{
    private int[] diff;

    public Difference(int[] nums){
        assert nums.length > 0;
        diff = new int[nums.length];
        //构造差分数组
        diff[0] = nums[0];

        for(int i = 1; i < nums.length - 1; i++){
            diff[i] = nums[i] - nums[i-1];
        }
    }

    public void increment(int i, int j,int val){
        diff[i]+= val;
        if( j + 1 < diff.length){  // 若摸到边界,则是全部添加val
            diff[ j + 1] -= val;
        }
    }

    public int[] result(){
        int[] res = new int[diff.length];
        res[0] = diff[0];
        for(int i = 1; i < diff.length; i++){
            res[i] = res[i-1] + diff[i];

        }
        return res;
    }

}
```



#### [1094. 拼车](https://leetcode-cn.com/problems/car-pooling/)

```java
class Solution {
    public boolean carPooling(int[][] trips, int capacity) {
        int[] nums = new int[1001];
        Difference df = new Difference(nums);

        for(int[] trip : trips){
            int val = trip[0];
            int i = trip[1];
            int j = trip[2] - 1; // attention
            df.increment(i,j,val);
        }

        int[] res = df.result();

        for (int i = 0; i < res.length; i++) {
            if (capacity < res[i]) {
                return false;
            }
        }
        return true;
        

    }
}


class Difference{
    private int[] diff;

    public Difference(int[] nums){
        assert nums.length > 0;
        diff = new int[nums.length];
        //构造差分数组
        diff[0] = nums[0];

        for(int i = 1; i < nums.length - 1; i++){
            diff[i] = nums[i] - nums[i-1];
        }
    }

    public void increment(int i, int j,int val){
        diff[i]+= val;
        if( j + 1 < diff.length){  // 若摸到边界,则是全部添加val
            diff[ j + 1] -= val;
        }
    }

    public int[] result(){
        int[] res = new int[diff.length];
        res[0] = diff[0];
        for(int i = 1; i < diff.length; i++){
            res[i] = res[i-1] + diff[i];

        }
        return res;
    }

}
```

