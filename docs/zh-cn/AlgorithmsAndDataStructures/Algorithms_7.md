# 队列和栈

## 括号问题（栈问题）

#### [20. 有效的括号](https://leetcode-cn.com/problems/valid-parentheses/)

- !left.isEmpty() : 对应着 str = "]" 的类似情况
- left.peek() == leftof(c) : 查看栈顶情况
- 只看栈顶因为问题产生的结果是

```java
public boolean isValid(String str) {
        Stack<Character> left = new Stack<>();
        for(char c : str.toCharArray()){
            if( c == '(' || c == '{'|| c == '['){
                left.push(c);
            }else{
                // !left.isEmpty() : 对应着 str = "]" 的类似情况
                if( !left.isEmpty() && left.peek() == leftof(c) ){
                    left.pop();
                }else{
                    return false;
                }
            }
        }

        return left.isEmpty();
    }

    public char leftof(char c){
        if(c == ')') return '(';
        if(c == '}') return '{';
        return '[';
    }
```

#### [921. 使括号有效的最少添加](https://leetcode-cn.com/problems/minimum-add-to-make-parentheses-valid/)

- "()": 直接遇到一对 need ++ need-- 清掉一对
- need == -1 表示需要插入左括号，前面的东西已经无法挽救了
- res先记录需要插入右括号的，然后加上插入左括号的
  - 即 res + need

```java
public int minAddToMakeValid(String s) {
        int res = 0;
        int need = 0;
        for(int i = 0; i <= s.length() - 1; i++){
            if(s.charAt(i) == '('){
                need++;
            }
            if(s.charAt(i) == ')'){
                need--;

                if (need == -1){
                    need = 0;
                    res++;
                }
            }
        }

        return res + need;

    }
```



## 单调栈（栈问题）

### 单调栈问题解题思路

- 创建单调栈

- for : 将数组从后往前遍历
  - while : 若 栈非空  and 栈顶值 <= 当前数 : 移除栈顶数，知道此数能进栈
  - 若 栈为空 : 直接将数存进栈内
- 上述 for(while)结构能保证一个单调递增栈
  - 同时，也能找到"数组中对应索引存储着下⼀个更⼤元素"

#### [503. 下一个更大元素 II](https://leetcode-cn.com/problems/next-greater-element-ii/)

- 单调栈的模板
- 因为此题考虑是循环数组，有两种方案解决循环数组
  - 创建一个2倍长的数组拷贝
  - 索引乘二取余（此方法）

```java
public int[] nextGreaterElements(int[] nums) {
        int n = nums.length;
        int[] res =  new int[n];
        Stack<Integer> s = new Stack<Integer>();
        for (int i = 2 * n - 1; i >= 0; i--){
            while(!s.empty() && s.peek() <= nums[i % n]){
                s.pop();
            }
            res[i%n] = s.empty() ? -1 : s.peek();
            s.push(nums[i%n]);
        }
        return res;
    }
```

#### [739. 每日温度](https://leetcode-cn.com/problems/daily-temperatures/)

- 存储的是索引
- 返回的结果应该是索引直接的距离

```java
public int[] dailyTemperatures(int[] temperatures) {
        int n = temperatures.length;
        int[] res =  new int[n];
        Stack<Integer> s = new Stack<Integer>();
        for (int i = n - 1; i >= 0; i--){
            while(!s.empty() && temperatures[s.peek()] <= temperatures[i]){
                s.pop();
            }
            res[i] = s.empty() ? 0 : (s.peek() - i);
            s.push(i);
        }
        return res;
    }
```

