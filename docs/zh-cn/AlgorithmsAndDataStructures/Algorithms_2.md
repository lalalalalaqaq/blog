# 算法小记2

### 回溯

- return
  - 有参时返回参数
  - 无参跳出当前方法

- remove：删除最后一位，意味这撤回操作  [1,2] >> [1]
- k == 0：暂停搜索条件
- 未完

```java
// 回溯框架
result = []
def backtrack(路径, 选择列表):
	if 满⾜结束条件:              //
		result.add(路径)         //增加路径
		return                  //跳出方法
	for 选择 in 选择列表:         //选择列表
		做选择                   //
		backtrack(路径, 选择列表) //
		撤销选择                 //remove



// lc 77 组合

class Solution {

    public List<List<Integer>> res =  new ArrayList<>();
    public List<List<Integer>> combine(int n, int k) {
        getcombine(n, k, 1, new ArrayList<>());
        return res;
    }

    public void getcombine(int n, int k, int start,List<Integer> path){

        if ( k == 0){
            res.add(new ArrayList(path)); //attention
            return ;
        }


        for (int i = start; i <= n - k + 1; i++){
            path.add(i);                 //attention
            getcombine(n,k-1,i+1,path);  //attention 
            path.remove(path.size() -1); //attention
        }
    }

}

```



### 滑动窗口

- 关于attentinon 1：
  - 在lc 76 and 567 中
    - window负责记录各个字符的数量
    - 由左右指针记录相对位置
  
- 关于窗口放缩问题
  - 先防后缩
    - 先右指针移动到符合标准的最大位置（第一个 while）
    - 然后左指针移动缩小到最优位置（第二个while）
  - 指针放缩的同时应该更新数据的内容，即attention操作，放缩操作对称
  
- 相关条件需要理解后编写

- 哈希表need是负责记录子串

- 滑动窗口四步思考

  **1、**当移动`right`扩大窗口，即加入字符时，应该更新哪些数据？

  **2、**什么条件下，窗口应该暂停扩大，开始移动`left`缩小窗口？

  **3、**当移动`left`缩小窗口，即移出字符时，应该更新哪些数据？

  **4、**我们要的结果应该在扩大窗口时还是缩小窗口时进行更新？

```java
/* 滑动窗口算法框架 */
void slidingWindow(string s, string t) {
    unordered_map<char, int> need, window;
    for (char c : t) need[c]++;

    int left = 0, right = 0;
    int valid = 0; 
    while (right < s.size()) {
        // c 是将移入窗口的字符
        char c = s[right];
        // 右移窗口
        right++;
        // 进行窗口内数据的一系列更新
        ...

        /*** debug 输出的位置 ***/
        printf("window: [%d, %d)\n", left, right);
        /********************/

        // 判断左侧窗口是否要收缩
        while (window needs shrink) {
            // d 是将移出窗口的字符
            char d = s[left];
            // 左移窗口
            left++;
            // 进行窗口内数据的一系列更新
            ...
        }
    }
}
```

```java
// lc 3 无重复长度字串的最大长度
public int lengthOfLongestSubstring(String s) {

        HashMap<Character,Integer>  windows = new HashMap<>();
        int left = 0;
        int right = 0;
        int res = 0;

        while( right < s.length()){
            char c = s.charAt(right);
            right++;
            // 窗口数据更新
            windows.put(c,windows.getOrDefault(c,0)+1);

            // 判断左窗口是否要收缩
            while(windows.get(c) > 1){
                char d = s.charAt(left);
                left++;
                //窗口数据更新
                windows.put(d,windows.getOrDefault(d,0)-1);
            }

            res = Math.max(res,right-left);
        }

        return res;
        
    }
```

```java
// lc 76 最小覆盖子串
// 最小覆盖子串的表达，最佳左右指针的位置
// 最小覆盖字串内容的控制由windows实现
// 有哈希表记录字符量、对应数目，左右指针控制位置
public String minWindow(String s, String t) {

        HashMap<Character,Integer>  window =  new HashMap();
        HashMap<Character,Integer>  need =  new HashMap();

        for (char c:t.toCharArray()){
            need.put(c,need.getOrDefault(c,0) + 1);
        }

        int left = 0;
        int right = 0;
        int valid = 0;
        int start = 0;
        int len = Integer.MAX_VALUE;

        while( right < s.length()){
            char c = s.charAt(right);
            right++;

            // attention 1
            if(need.containsKey(c)){
                window.put(c,window.getOrDefault(c,0)+1);
                if(window.get(c).equals(need.get(c))){
                    valid++;
                }
            }
            // attention 1

            while(valid == need.size()){
                
                if (right - left < len) {
                    start = left;
                    len = right - left;
                }

                char d = s.charAt(left);
                left++;
                if(need.containsKey(d)){
                    window.put(d,window.getOrDefault(d,0)-1);   
                    if(window.get(d).equals(need.get(d))){
                        valid--;
                    }
                }
            }

        }

        return len == Integer.MAX_VALUE ? "" : s.substring(start, start + len);

    }
```

```java
// lc 567 字符串的排列
// 符合小子串的排列意味着只需要出现相应的字符和对应的数量即可
public boolean checkInclusion(String s1, String s2) {

    HashMap <Character,Integer> needs = new HashMap();
    HashMap <Character,Integer> windows = new HashMap();
    int left =0; 
    int right = 0;
    int valid =0;

    for(char c: s1.toCharArray()){
        needs.put(c,needs.getOrDefault(c,0)+1);
    }

    while(right<s2.length()){
        char c = s2.charAt(right);
        right++;

       // attention 1
        if(needs.containsKey(c)){
            windows.put(c,windows.getOrDefault(c,0)+1);
            if(windows.get(c).equals(needs.get(c))){
                valid++;
            }
        }
        // attention 1

        while(right-left>=s1.length()){
            if(valid==needs.size()){
                return true;
            }
            char d = s2.charAt(left);
            left++;

            if(needs.containsKey(d)){
                if(windows.get(d).equals(needs.get(d))){
                    valid--;
                }
                windows.put(d,windows.getOrDefault(d,0)-1);
            }
        }

    }
    return false;


}
```



