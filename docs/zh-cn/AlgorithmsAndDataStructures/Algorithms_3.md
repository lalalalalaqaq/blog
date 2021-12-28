# 算法小记

### 位运算

位运算小操作

```java
@Test
public void bitoperation1(){
    char char_a = 'a';
    char char_A = 'A';

    // ASCII码 位操作                    // (char)char2int_n : char2int_n
    // 转小写
    int char2int_1 =  (char_A | ' ');  // a:97
    int char2int_2 =  (char_a | ' ');  // a:97
    //  转大写
    int char2int_3 =  (char_A & '_');  // A:65
    int char2int_4 =  (char_a & '_');  // A:65
    //  大小写转换
    int char2int_5 =  (char_A ^ ' ');  // a:97
    int char2int_6 =  (char_a ^ ' ');  // A:65

    // 是否异号
    int x = -1, y = 2 , m = 3 , n = 4;
    boolean f1 = ((x ^ y) < 0); // true 是异号
    boolean f2 = ((m ^ n) < 0); // false 不易号

    // 不用临时变量交换两个数
    int a = 1, b = 2;
    a ^= b;
    b ^= a;
    a ^= b;
    // 现在 a = 2, b = 1

    // 加一
    // int n = 1;
    // n = -~n;
    // 现在 n = 2

    // 减一
    // int n = 2;
    // n = ~-n;
    // 现在 n = 1
}

```



算法操作：**n&(n-1)**

意义：消除数字 n 的二进制表示中的最后一个1

核心逻辑如下

- n - 1 一定可以消除最后一个 1
  - 同时把其后的 0 都变成 1
  - 这样再和 n 做一次 & 运算
  - 就可以仅仅把最后一个 1 变成 0 了。

lc 461:

- 两个整数之间的 [汉明距离](https://baike.baidu.com/item/汉明距离) 指的是这两个数字对应二进制位不同的位置的数目。
- **ans**异或后，相同为零，不同为一
- 异或得到的**ans**在相同位为零，不同位为一
- 进行**n & (n-1)**(lc191)的操作即可获得汉明距离

```java
// lc 191 位1的个数    
	public int hammingWeight(int n) {
        int res = 0;
        while (n != 0){
            n = n & (n-1);
            res++;
        }
        return res;
        
    }

// lc 231 2的幂
    public boolean isPowerOfTwo(int n) {
        // 一个数如果是 2 的指数,那么它的二进制表示一定只含有一个1
        if (n <= 0) {
            return false;
        };
        return (n & (n - 1)) == 0;
    }

// lc 461 汉明距离
    public int hammingDistance(int x, int y) {
        int res = 0 ;
        int ans = x^y ;
        while( ans != 0){
            ans = ans&(ans - 1);
            res++;
        }

        return res;

    }

```



算法操作 **异或: ^**

**异或**四基本要点：

- 二进制中，**相同为零，不同为一**

- 交换律：a ^ b ^ c <=> a ^ c ^ b
- 任何数于0异或为任何数 0 ^ n => n
- 相同的数异或为0: n ^ n => 0

lc136中：

- for 循环的本质是连续异或
  - 举例：2 ^ 4 ^ 2 = 4
  - 2(010) ^ 4 (100) = 5(110)
  - 5(110) ^ 2 (010) = 4(100)
- 根据交换律，for循环的连乘可以等价为 2 ^ 2 ^ 4，
  - 约等于使用了**相同的数异或为0** 和 **任何数于0异或为任何数**
- 因为数组的只有一个数字一次出现，其余为出现两次，结果为出现一次的数字

```java
// lc 136 只出现一次的数字
    public int singleNumber(int[] nums) {

        int res = 0; 
        for (int i : nums){
            res ^= i;
        }
        return res;
    }
```

