{{TOC}}
#Heap
##问题与原理
一道题：给一串未排序的数，如何实现操作popMax()，每次弹出其中的最大值？26, 32, 45, 10, 29, 8, 11, 9, 73, 15
直觉的想法：先把这串数的顺序排一遍，每次提取出最大的即可。O（NlogN）。
但是，每次插入一个新数，就要把它放进该放的位置，插入的复杂度是O(N)。
有没有更好的想法？
用二叉搜索树如何？好像可以，每次只需要搜索最右端的就是最大数，弹出效率高，插入也是O(logN)。但是，BST不是自平衡的，一些输入的数字可能让插入接近线性，复杂度也就接近了O(N)。
有没有别的办法保证能至少在O(logN)内弹出最大数，而插入新数复杂度也确定是O(logN）？
我们今天介绍一个数据结构来解决这个问题：Heap。它是一个二叉树，满足以下性质：
- It’s complete (Almost). Every level of the tree has the maximum  number of nodes except possibly the last level. It’s filled in reading  from left to right across each row.  
- The largest data is in the root.  
- For every node in the max#heap, its children contain smaller key 
- value.  

我们来看看Heap如何实现插入和删除O(logN)的性质：
					    73 
                    /         \ 
                 32           45 
               /     \        /    \ 
            26     11   10    29 
          /    \ 
        15     8 

（插入30）
弹出最大值

看起来很神奇？我们现在就来介绍Heap的结构、功能和性质。

##功能、方法和复杂度
Heap的基本功能：
Add：O(logN）
PopTop(Max or Min):O(logN)

##实现
1. 表示方法和基本操作
操作-用Array-用Linked Node的结构对比表，把index写在树的旁边。
我们可以用Array来实现堆。
奇怪，树不是Linked么？为何可以用Array？
正是利用了堆几乎是完全树的性质：除了最后一层，每一层的数目都是恒定的，所以我们可以算出每一个节点对应到array里面的index。
[实现](https://docs.google.com/document/d/1BUiLMZNLt7QrHeuqDnVAmwMKiDXc24TGLCwaZqQley4/edit#)
Array实现堆可以省掉指针的空间，而且不影响节点之间的连通。

![](https://www.cs.cmu.edu/~adamchik/15-121/lectures/Binary%20Heaps/pix/complete.bmp)

- its left child is located at 2*k index 
- its right child is located at 2*k+1. index 
- its parent is located at k/2 index
	
	0的位置空出来，是为了配出上面的关系，可以用二进制左右移动来定位，进一步提高了效率。

2. 建堆
建堆的两种方法，第二种需要证明正确性（叶子树已经是valid的了），但不需要证明复杂度（O(N))。
给我们一串数，不断加数进去就是建堆了。那么，建一个堆的时间复杂度是多少呢？是O(NlogN)吗？
我们还有更快的方法建立堆么？
逐层[扫描法](http://ahalei.blog.51cto.com/4767671/1427156)
这样做建堆时间只有O(N)！

##注意事项
##应用
