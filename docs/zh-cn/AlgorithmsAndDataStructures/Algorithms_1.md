# 算法小记1



### 二分查找

- 前提：已经按顺序排列

流程：

- 定义好 left right
- while 的判断条件到底是什么 (left  <= right  or  left < right ?)
- mid = left + ( right - left ) / 2
- left 和 right 的变化
- return 的值到底是什么

```java
// lc 704

class Solution {
    public int search(int[] nums, int target) {
        if (nums == null || nums.length == 0) return -1;
        int l  = 0;
        int r = nums.length-1;
        while(l <= r){
            int mid = l+(r-l)/2;
            if(nums[mid] > target){
                r = mid-1;
            }else if(nums[mid] < target){
                l = mid+1;
            }else{
                return mid;
            }
        }
        return -1;
    }
    
}

// 拓展
// https://www.cnblogs.com/kyoner/p/11080078.html
// 左边界
int left_bound(int[] nums, int target) {
    if (nums.length == 0) return -1;
    int left = 0;
    int right = nums.length; // 注意

    while (left < right) { // 注意
        int mid = (left + right) / 2;
        if (nums[mid] == target) {
            right = mid;
        } else if (nums[mid] < target) {
            left = mid + 1;
        } else if (nums[mid] > target) {
            right = mid; // 注意
        }
    }
    return left;
}


// 右边界
int right_bound(int[] nums, int target) {
    if (nums.length == 0) return -1;
    int left = 0, right = nums.length;

    while (left < right) {
        int mid = (left + right) / 2;
        if (nums[mid] == target) {
            left = mid + 1; // 注意
        } else if (nums[mid] < target) {
            left = mid + 1;
        } else if (nums[mid] > target) {
            right = mid;
        }
    }
    return left - 1; // 注意
    
    

```





#### 广度优先

- 目前了解矩阵和树的BFS
- 将层层遍历的数据保留到**queue**中
- 用for 遍历 queue (遍历 queue.size() 次)
  - 判断任务所需内容，用一个很长的if去判断
    - if 包含边界条件的确认
    - 包含任务的判断内容
  - 将下一层需要遍历的数据置于queue中
    - 矩阵则是将坐标放进队列
    - 树则是将节点放入
  - 回头看while 终止条件**!queue.isEmpty()**

```java
// lc 733 
// 对于矩阵的BFS
public int[][] floodFill(int[][] image, int sr, int sc, int newColor) {

        int[] dx = {1, 0, 0, -1};
        int[] dy = {0, 1, -1, 0};
        
        int currColor = image[sr][sc];
        if (currColor == newColor){
            return image;
        }


        int n = image.length;
        int m = image[0].length;

        Queue<int[]> queue = new LinkedList<int[]>();
        queue.offer(new int[]{sr,sc});

        image[sr][sc] = newColor;

        while(!queue.isEmpty()){
            int[] cell = queue.poll();
            int x = cell[0];
            int y = cell[1];
            for (int i = 0; i < 4; i++){
                int mx = x + dx[i];
                int my = y + dy[i];

            if ( mx >= 0 &&  mx < n &&  my >= 0 && my < m && image[mx][my] ==currColor){
                    queue.offer(new int[]{mx,my});
                    image[mx][my] = newColor;
                }
            }
        }

        return image;
    }


//lc 695
// 矩阵BFS
public int maxAreaOfIsland(int[][] grid) {
        int ans = 0;
        for (int i = 0; i != grid.length; ++i) {
            for (int j = 0; j != grid[0].length; ++j) {
                int cur = 0;
                Deque<Integer> stacki = new LinkedList<Integer>();
                Deque<Integer> stackj = new LinkedList<Integer>();
                stacki.push(i);
                stackj.push(j);
                while (!stacki.isEmpty()) {
                    int cur_i = stacki.pop(), cur_j = stackj.pop();
                    if (cur_i < 0 || cur_j < 0 || cur_i == grid.length || cur_j == grid[0].length || grid[cur_i][cur_j] != 1) {
                        continue;
                    }
                    ++cur;
                    grid[cur_i][cur_j] = 0;
                    int[] di = {0, 0, 1, -1};
                    int[] dj = {1, -1, 0, 0};
                    for (int index = 0; index != 4; ++index) {
                        int next_i = cur_i + di[index], next_j = cur_j + dj[index];
                        stacki.push(next_i);
                        stackj.push(next_j);
                    }
                }
                ans = Math.max(ans, cur);
            }
        }
        return ans;
    }

// lc 116
// 树的bfs
  public Node connect(Node root) {

        if (root == null){
            return root;
        }

        Queue<Node> queue = new LinkedList<Node>();
        queue.add(root);


        while(!queue.isEmpty()){

            int size = queue.size();


            for (int i = 0 ; i < size ; i++){

                Node node  =  queue.poll();

                if (i < size - 1){
                    node.next = queue.peek();
                }

                if (node.left != null){
                    queue.add(node.left);
                }

                if ( node.right != null){
                    queue.add(node.right);
                }

            }

        }

        return root;
    
    }
// lc104 剑指55
// 树的bfs
    public int maxDepth(TreeNode root) {
        if (root == null){
            return 0;
        }

        Queue<TreeNode> queue = new LinkedList<TreeNode>();
        queue.add(root);
        int deep = 0;
        while (!queue.isEmpty()){
            
            int size = queue.size();
            
            for (int i = 0; i < size ; i++  ){
                
                TreeNode node = queue.poll();

                if (node.left != null){
                    queue.add(node.left);
                }

                if ( node.right != null){
                    queue.add(node.right);
                }

            }
            deep++;
        }
        
        return deep;

    }
```

