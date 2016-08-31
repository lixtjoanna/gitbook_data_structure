# 线性数据结构

## ArrayList技巧

## LinkedList技巧

1. 开头加dummy

   用于head的corner case无法resolve的情况，如203. Remove Linked List Elements

   ```java
   public ListNode removeElements(ListNode head, int val) {
          if (head == null)
              return null;
          ListNode dummy = new ListNode(0);
          dummy.next = head;
          ListNode cur = head, prev = dummy;
          
          while(cur != null) {
              if (cur.val == val) {
                  prev.next = cur.next;
              }
              else {
                  prev = cur;
              }
              cur = cur.next;
          }
          
          return dummy.next;
   }
   ```

   ​

2. A

3. A

4. A

5. A

6. A


# 树

##Recursion / Recursion Tree

### 原裝

1. 104_Maximum Depth of Binary Tree (Easy)

   正常递归即可，记得每上一层要+1（画树能看得更清楚）。

   ```java
   public int maxDepth(TreeNode root) {
       if (root == null) 
           return 0;
       return Math.max(maxDepth(root.left), maxDepth(root.right)) + 1;
   }
   ```

2. 235_Lowest Common Ancestor of a Binary Search Tree

   畫遞歸樹經典例子。先搜索左右，若左右都找到了相關元素，就返回root；若只有左找到就返回左，右一樣；

   ```java
   // 在root为根的二叉树中找A,B的LCA:
   // 如果找到了就返回这个LCA
   // 如果只碰到A，就返回A
   // 如果只碰到B，就返回B
   // 如果都没有，就返回null
   public TreeNode lowestCommonAncestor(TreeNode root, TreeNode node1, TreeNode node2) {
       if (root == null || root == node1 || root == node2) {
           return root;
       }
       
       // Divide
       TreeNode left = lowestCommonAncestor(root.left, node1, node2);
       TreeNode right = lowestCommonAncestor(root.right, node1, node2);
       
       // Conquer
       if (left != null && right != null) {
           return root;
       } 
       if (left != null) {
           return left;
       }
       if (right != null) {
           return right;
       }
       return null;
   }
   ```
3. 112_PathSum

   递归，不需要存value，减到0返回true，最后还没有返回false。

   ```java
   public boolean hasPathSum(TreeNode root, int sum) {
       if (root == null) 
           return false;
       if (root.left == null && root.right == null) { // if root is leaf
           return root.val == sum;
       }
       return hasPathSum(root.left, sum - root.val) || hasPathSum(root.right, sum - root.val);
   }
   ```

### Wrapper

往往用於需要放進非原方法信息的場合；有時是和原方法性質不同的信息，有時是增加新的信息。

1. 110_Balanced Binary Tree

   判定平衡樹，關鍵在於傳遞孩子height的同時記錄平衡信息，在此用﹣1。也可以用遞歸樹表示。

   原函數返回的是boolean，但我們要用的函數除了要記錄左右子樹各自的平衡信息，還要記錄兩子樹相對的高度差，所以必須是int，就要用到wrapper。

   ```java
   public boolean isBalanced(TreeNode root) {
       return bHeight(root) != -1;
   }

   private int bHeight(TreeNode root) {
       if (root == null) {
           return 0;
       }
       int bleft = bHeight(root.left);
       int bright = bHeight(root.right);
       if (bleft == -1 || bright == -1 || Math.abs(bleft - bright) > 1) {
           return -1;
       }
       return Math.max(bleft, bright) + 1;
   }
   ```

2. 98_Validate Binary Search Tree

   维护上下界，传递界用max和min通过参数传递。因为不仅要传boolean所以用wrapper。

   注意这里传递时double上下界的使用，是为了规避int上下界和传递出来的min和max撞在一起无法区分的情况。

   ```java
   public boolean isValidBST(TreeNode root) {
       if (root == null)
           return true;
       return helperValidater(root.left, Double.NEGATIVE_INFINITY, (double)root.val) && helperValidater(root.right, (double)root.val, Double.POSITIVE_INFINITY);
   }

   private boolean helperValidater(TreeNode p, double min, double max) {
       if (p == null)
           return true;
       if (p.val <= min || p.val >= max) {
           return false;
       }
       return helperValidater(p.left, min, p.val) && helperValidater(p.right, p.val, max);
   }
   ```

3. ​

## Binary Search (Find Successor)

1. 285_Inorder Successor in BST (TreeNode root, TreeNode p)

     successor是右子树的左极或右父亲。

     while loop搜索到该元素，维护“右父亲”节点。若没有右子树就返回右父亲。

   ```java
     public TreeNode inorderSuccessor(TreeNode root, TreeNode p) {
         if (root == null || p == null) return null;
         
         //二分查找标记更新successor（右父亲）
         TreeNode successor = null;
         
         while(root != null && root.val != p.val) {
             if (root.val > p.val) {
                 successor = root;
                 root = root.left;
             }
             else {
                 root = root.right;
             }
         }
         
         if (root == null) return null;//没找到p
         
         if (p.right == null) return successor;//p没有右子树
         
         p = p.right;//p的右子树的左极
         while (p.left != null) {
             p = p.left;
         }
         return p;
     }
   ```

2. 270_Closest Binary Search Tree Value

   找最近的元素而不是exact search，在切割的时候保留root并比较哪个更近即可。

   ```java
   //就是不切root的binary search。如果是叶子就返回root.val，如果不是叶子，正常BS，但保留root并对比其结果。注意只有一个孩子的情况下按方向返回root或继续递归。
   public int closestValue(TreeNode root, double target) {
       if (root.left == null && root.right == null) {
           return root.val;
       }
       double d1 = Math.abs(root.val - target);
       double d2 = d1;
       int c = root.val;
       if (target > root.val && root.right != null) {
           c = closestValue(root.right, target);
           d2 = Math.abs(c - target);
       }
       if (target < root.val && root.left != null) {
           c = closestValue(root.left, target);
           d2 = Math.abs(c - target);
       }
       return (d1 > d2) ? c : root.val;
   }
   ```

3. ​

## DFS/BFS

### DFS: Recursive

DFS思想的关键即利用BST的顺序性及同构性。思考：子树结果能为总的结果做些什么？

#### 挖掘子树之间及父子之间的关系

104_Maximum Depth of Binary Tree (Easy) / 111__Minimum Depth of Binary Tree

Minimum可以用BFS也可以用DFS，注意分情况，root为null和其他node为null不一样。

```java
    public int minDepth(TreeNode root) {
        if (root == null) {
            return 0;
        }
        return getMin(root);
    }

    public int getMin(TreeNode root){
        if (root == null) {
            return Integer.MAX_VALUE;
        }

        if (root.left == null && root.right == null) {
            return 1;
        }

        return Math.min(getMin(root.left), getMin(root.right)) + 1;
    }
```
#### “隔断世界之间的联系”？

 116_Populating Next Right Pointers in Each Node (Medium)

- You may only use constant extra space.
- You may assume that it is a perfect binary tree (ie, all leaves are at the same level, and every parent has two children).

   就是以如下形式連接tree nodes：

   ```
    		1 -> NULL
          /  \
         2 -> 3 -> NULL
        / \  / \
       4->5->6->7 -> NULL
   ```

   看起来像BFS，每刷過一層就連一次right pointer。但這樣要用queue，不符合constant extra space要求。

   用DFS能不能做？每下一層，先連上左和右樹，再分別遞歸左、右樹的鏈接。但這樣的話，中間5->6就連不上。用root.right.next = root.next.left來鏈接。

```java
   public void connect(TreeLinkNode root) {
       if (root == null) return;
       if (root.left != null && root.right != null) {
           root.left.next = root.right;
          
       }
       if(root.next!= null && root.right!=null) {
           root.right.next = root.next.left;
       }

       connect(root.left);
       connect(root.right);
   }
```

#### “添加的同时保存轨迹”

257_BinaryTreePaths

用于需要更新信息但又要按照stack frame保存原信息的场合。关键是：复制。

```java
/**
* @param root the root of the binary tree
* @return all root-to-leaf paths
*/
public List<String> binaryTreePaths(TreeNode root) {
   List<String> result = new ArrayList<String>();
   if (root == null) {
       return result;
   }
   helper(root, String.valueOf(root.val), result);
   return result;
}

 private void helper(TreeNode root, String path, List<String> result) {

       if (root == null) {
           return;
       }
       
       if (root.left == null && root.right == null) {
           result.add(path);
           return;
       }
       
       if (root.left != null) {
           helper(root.left, path + "->" + String.valueOf(root.left.val), result);
       }
       
       if (root.right != null) {
           helper(root.right, path + "->" + String.valueOf(root.right.val), result);
       }

   }
```



### DFS: Iterative (Stack)

#### 三顺序DFS iteration的Stack实现

173_Binary Search Tree Iterator

`next()` and `hasNext()` should run in average O(1) time and uses O(*h*) memory, where *h* is the height of the tree.

记录当前元素的位置。

所以是stack，因为递归无法维护“当前位置”。

用stack，curr记录当前位置，curr往左压栈，然后pop出左极，再将curr放在当前值的右子树处准备下一次next call。HasNext检测curr和stack是否有元素即可。

```java
public class BSTIterator {
        TreeNode curr;
        Stack<TreeNode> stack = new Stack<TreeNode>();

        public BSTIterator(TreeNode root) {
            curr = root;
        }

        /** @return whether we have a next smallest number */
        public boolean hasNext() {
            return (curr != null) || (!stack.isEmpty());
        }

        /** @return the next smallest number */
        public int next() {
            while (curr != null) {
                stack.push(curr);
                curr = curr.left;
            }

            TreeNode node = stack.pop();
            curr = node.right;

            return node.val;
        }
   }
```

94_Binary Tree Inorder Traversal (Must do iteratively) 

上题变种

144_Binary Tree Preorder Traversal



### BFS (Queue)

1. 102_Binary Tree Level Order Traversal

   BFS经典题目，要按照level打印，需要记录每个level的元素个数。  

   Queue每一轮里先数个数，然后for loop弹出目前层，每弹一个node加入一次孩子，再继续loop。注意linkedlist实现的queue的用法。

   ```java
   public List<List<Integer>> levelOrder(TreeNode root) {
       List<List<Integer>> res = new ArrayList<List<Integer>>();
       
       if (root == null) {
           return res;
       } 
       
       Queue<TreeNode> queue = new LinkedList<TreeNode>();
       queue.offer(root);
       
       while (!queue.isEmpty()) {
           List<Integer> pres = new ArrayList<Integer>();
           int size = queue.size();
         
           for (int i = 0; i < size; i++) {
               TreeNode node = queue.poll();
               pres.add(node.val);
               if (node.left != null) {
                   queue.offer(node.left);
               }
               if (node.right != null) {
                   queue.offer(node.right);
               }
           }
         
           res.add(pres);
       }
       
       return res;
   }
   ```

2. ​

#图

##压缩路径
##3Color
##Topology Sorting
##Graph Traverse
###DFS    
    if(越界了) return;

    int[][] direction = {{1,0},{0,1}..}
    for(int[][] dir : direction) {
        findAllIsland(x+dir[0][1])
    }


###BFS
1  1  0  1
     1  1  0  0
     0  0  1  1
    
    input: int[][]
    output:  2
        
    int count = 0;
    遍历所有的点
        1. 1
            新大陆的入口
                count++;
                findAllIsland(入口) //把入口自己和他所有连接标记成 2 
        
        2. 不是1
             continue


    findAllisland(入口)  遍历  //dfs  bfs

    4个方向 findAllisland()

        public static List<List<TreeNode>> printLayer(TreeNode root) {
            if (root == null) {
                return null; 
            }
            Queue<TreeNode> queue = new Queue<TreeNode>();
            TreeNode cur = root;
            queue.add(cur);
            int cursize = 0;
            
            List<List<TreeNode>> res = new ArrayList<ArrayList<TreeNode>>();
            while (!queue.isEmpty()) {
                List<TreeNode> list = new ArrayList<TreeNode>();
                cursize = queue.size();
                for (int i = 0; i < cursize; ++i) {
                    cur = queue.dequeue();
                    list.add(cur);
                    
                    if (cur.left != null) {
                        queue.add(cur.left);
                    }
                    if (cur.right != null) {
                        queue.add(cur.right);
                    }
                }
                res.add(list);
            }
            
            return res;
        }


#Misc
##窗口法
1. 219Contains Duplicate II
   用一个hashset，移动窗口往前，去掉的时候注意remove()只能去掉内容(hash没有顺序），所以要注意计算正确的index。
   	public boolean containsNearbyDuplicate(int[] nums, int k) {
   	    if (nums == null || nums.length < 2)
   	        return false;
   	    HashSet<Integer> set = new HashSet<Integer>();
   	    
   	    for (int i = 0; i < nums.length; ++i) {
   	        if (set.contains(nums[i]))
   	            return true;
   	        set.add(nums[i]);
   	        if (set.size() > k) {
   	            set.remove(nums[i - k]);
   	        }
   	    }
   	    
   	    return false;
   	} 
2. a


## Numbers

1. FindLastDigit
2. a



## 概率

## 数学规律

1. 1出现次数（《程序员代码面试指南》）：给定整数n，从1到n中1出现的次数？

   按照位数来数。

2. Rand5 to Rand7

   公式：5*rand5() + rand5()
    —> k*randk() + randk() 分成等概率的k^2份。
    怎么证明：randk等于除k取模。
    [Rand](https://www.quora.com/Design-a-rand7-function-using-a-rand5-function-Help)

3. 螺帽和螺母配套，不许自己排序，则互相排序，各拿出一个螺帽，找到它匹配的螺母，再用那个螺母反过来排序。

   Divide and Conquer. 比较像QuickSort.

4. ​