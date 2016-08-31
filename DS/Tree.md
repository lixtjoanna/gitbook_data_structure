
#Tree
##问题
上一节中，我们认识了线性数据结构List。我们把它想象为一些箱子，有一种方式可以将这些箱子线性地排列起来，让我们能够添加、删除、访问、搜索和遍历。
现在，我们来试着解答一个新的问题。
中华民族是一个有着悠久传统的民族，许多同学家里都有家谱。我家是明代大槐树移民，就是朱元璋时从山西迁移了一批人到华北平原，来补充战乱时损失的人口。我们家的家谱就从第一代移民开始，每个祖先有几个儿子，他们的儿子分别又有几个儿子，都记载在家谱上。因为中国古代是一个男权社会，我们的家谱正文只记载父亲和儿子，而母亲和女儿则是作为附注和附录记载的，很多女性祖先都没有名字，只叫“张氏”“王氏”。如果我们现在修家谱，肯定不能采取这样封建的做法了。
那么，假如我们手里有一本家谱，我现在想要找到第15代传人小明的爷爷是谁，我应该怎么找呢？
一个最直观的方法是，我们先往回翻看小明之前的所有记载，看看是谁生了小明，假如这个人叫大明，那么我们再往回翻，看谁生了大明，这个人就是小明的爷爷。【图】

但是，这种做法很低效。因为我们要检查小明所有的父辈，比如他的伯伯、叔叔。还要检查大明所有的父辈，才能找出小明的爷爷来。如果我们每天都要找很多人的祖先，这种做法就非常凌乱了。

怎样才能更快地找到小明爷爷呢？

这种办法似乎更好些：我们阅读一遍家谱，把家谱所记载的父子传承关系，画成一张图。这张图上，每个人都用一个圈来表示，而人们之间的关系则用上下线来表示。【图】

你会发现，这张图有以下的特点：
1. 最顶端只有一个人，家谱没有记载他的父亲是谁，他是家谱的开始，是所有人的始祖。
2. 始祖以下的每一个人都只有一个父亲，一个人可以有多个儿子，也可以没有儿子。

然后，我们在这张图上找到小明，顺着他上面的连线找到他的父亲，再顺着他父亲的连线找到他的爷爷。因为每个人只有一个父亲，这个过程很迅速，不需要检索其他父辈。如果我们已经有了这张图，那么找到爷爷所花费的时间，仅他与爷爷之间的辈分差异有关——也就是两代人。这就大大缩短了查询的时间，便利了多次查询。

上面画的这张图，就叫做“树”。是一种重要的数据结构。运用“树”，我们只需要以下代码就可以找到小明的爷爷：

	TreeNode grandfather = xiaoming.parent().parent();

##定义
![](https://bittigerimages.s3.amazonaws.com/gitbookImages/DataStructures/edge-1.jpg)
树的定义，和上图的特点，是一致的。
树，就是一个节点（node）的集合，这些节点用父子关系来储存元素，父子关系满足以下条件：
1. 如果树非空，则有一个根节点，它没有父节点。
2. 根节点以外的其他节点，都有且仅有一个父节点。每个节点可能有一个或多个子节点，也可能没有。

树有一种更加正式的定义方式，涉及到”图“和”环“这两个概念，我们这个笔记不会涉及“图”和“环”，所以暂时不介绍了。有兴趣的朋友可以参考相关的教科书或维基百科。

树的各个部分都有独特的名称，总结如下：
[Tree Concepts](http://typeocaml.com/2014/11/26/height-depth-and-level-of-a-tree/)
- Root: The node at the top of the tree.  
- Parent: When any node (except the root) has exactly one edge running upward to another node. The node above is called parent of the node.  
- Child: Any node may have one or more lines running downward to other nodes. These nodes below the given node called its children.  
- Edge: connection between one node to another.
- Leaf: A node that has no children is called a leaf. There can be only one root in a tree but there can be many leaves.
- Levels: the level of a particular node refers to how many generations the node is from the root. The root is level 0 and its children are level 1 and so on.  
- Height: The height of a tree is the number of edges on the longest downward path between the root and a leaf.
- Path: a sequence of nodes and edges connecting a node with a descendant.

根据每个节点所能拥有的最大的子节点数量，树可以分为二叉树和多叉树两种。二叉树就是任一节点最多只能有两个子节点的树，而多叉树则是所有其他情况的树。

##二叉搜索树
###问题
了解了树的定义，让我们来用树解决一个新的问题。
看到以下的树，我们怎样才能把数字按照从小到大的顺序打印出来？
![](https://bittigerimages.s3.amazonaws.com/gitbookImages/DataStructures/binary_search_tree.jpg)
认真观察这个树，你会发现，只要从左到右把节点中的数打印出来就可以了。

###定义
这样的能够自动给元素排序的树，就叫做二叉搜索树。它满足以下条件：
1. 每一个节点的左子树中的元素，比这个节点的元素小，或与它相等。
2. 每一个节点的右子树中的元素，比这个节点的元素大，或与它相等。
这种树最大的作用，就是可以保持元素的顺序，相当于是一个排过序的list。但是，它进行插入或删除操作，在大部分情况下，又比排序的list更快捷。因此，当我们需要维护元素顺序或对元素排序时，二叉搜索树结构就非常适用。二叉搜索树是面试中非常常见的一种数据结构，值得深入的探讨。

###功能、方法及复杂度
由于独特的性质，BST可以进行一系列复杂度较低的基本操作。
1. Searching
	O(log N)，和binarySearch原理类似（都有order所以都可以搜一次截取一半）。
	如果树是一个链条，就成了O(N)。为什么？
	####平衡树
	树在极端情况下，和LinkedList并没有区别，要插入一个新元素到正确的位置，如果事先不知道位置的话，插入效率就是O(N)【图】。
	也可以说，LinkedList是树的一种极端情况，即每个非根、非叶子节点都有且仅有一个子节点的情况，此时节点形成一条链条，和LinkedList一模一样。
	这种样子的树也满足BST的条件，但和我们想要的BST大相径庭。
	由此我们发现，树操作的复杂度，和这棵树处于什么状态，有很大的关联。树结构越接近于一个链条，各操作就越像线性结构，就越失去了树结构独特的优势。那么，如何定义这种情况呢？我们定义一个“平衡树”的概念。当一棵树父节点的左子树和右子树的高度之差不大于1时，我们将其称为“平衡树”。“平衡树”的最大限度地保存了树结构的特点，规避了线性结构的问题。
	我们需要在使用树的过程中维护其平衡。维护树平衡的方法有许多，AVL是一种较为常用的自平衡二叉查找树。当AVL树检测到不平衡的情况出现，它会旋转树节点，使得树重新平衡。
2. Insertion
	平均情况下O(log N)，用类似binarySearch的方式找到和新元素最接近的叶子节点。
	有三种可能：重复（不做任何事结束），左树找到SPOT，右树找到SPOT。因为一次cut掉一半，故是O(log N)。
	二叉搜索树进行插入或删除操作，在大部分情况下，比排序的list更快捷。
2. Deletion
	平均情况O(log N)再多一些。先搜索到元素花O(log N)，如果没找到就end，如果找到，分四种情况，同时需要记录元素是其parent的左孩子还是右孩子。
	- 元素是leaf
		parent对应指针指到null。
	- 元素仅有左边child
		parent对应指针指到左child，元素的指针全部删除。
	- 元素仅有右边child
	  parent对应指针指到右child，元素的指针全部删除。
	- 元素有左右两个children
		这种情况最为复杂，需要找successor，也就是右子树中最小的元素。从要删除的元素往下一路向左，找到最左元素，定为successor，然后做两件事：
		- successor如果有右树，将其连到其parent的左树上。
		- successor新的右树，连到被删除元素的右树上。

	花费的时间，除了搜索以外，还有找successor以及固定的relink时间。
3. Traversal
	树尤其是二叉树、BST的遍历，是一种极其重要和常见的操作。我们上面所出的题目，之所以能够实现顺序打印子节点，靠的正是一种二叉树遍历的方法：深度优先遍历中的中根遍历。
	树遍历有两种方法：深度优先遍历和广度优先遍历，其中深度优先遍历又分为先根、后根和中根遍历。
	下面介绍遍历方法。
	- Depth First
	“先顺完一个根须，再顺其他根须”，和breadth first一层一层推进的顺序相对。注意顺完一个往回倒的时候是从下往上倒的。
	根据parent和child的visit顺序，分为Pre/Post/In-order，就是指先visit根节点还是其children。
	Pre就是先根节点，后children；In就是左边children-根节点-右边children。
	所有节点都被visit了一遍，所以是O(n)。
	- Breadth First
	优先遍历完同层。
	用一个queue结构，先dequeue根节点q，按照层序enqueue其children，visit(q)然后dequeue顶部新根节点，继续enqueue。重复到dequeue空为止。注意中间queue的形态。
	所有的Node都会被enqueue和dequeue各一遍，所以复杂度是O(2n)即O(n)。

#####Pre-Order
	Algorithm preOrder(Node root) {
		visit(root);
		for every child (c) of root {
			preOrder(c);
		}
	}

#####Post-Order
	Algorithm postOrder(Node root) {
		for every child (c) of root {
			postOrder(c);
		}
		visit(root);
	}

#####In-Order
	Algorithm inOrder(Node root) {
		for every left child (c) of root {
			inOrder(c);
		}
		visit(root);
		for every right child (c) of root {
			inOrder(c);
		}
	}

#####Breadth-First
【图】
	Algorithm breadthFirst(Node root) {
		Initialize queue Q to contain root;
		while Q is not empty {
			q = Q.dequeue();
			visit(q);
			for every child of q (c) {
				Q.enqueue(c);
			}
		}
	}
	我们将在下一节中具体介绍这些遍历方法的实现方式。

	================================================

	这些基本操作，对应的java方法如下：【对应的java库是哪个？ 】
	- boolean find(int key)
	- void insert(int key, double value)
	- void delete(int key)
	- Iterator iterator()

	下面，我们来看一看这些基本操作的实现。

###实现
1. TreeNode嵌套类
		// private static nested Node class
		private static class Node {
		    int key;
		    double value;
		    Node left, right;

		    Node(int key, double value) {
		        this.key = key;
		        this.value = value;
		        left = right = null;
		    }
		}
2. boolean find(int key)
		public boolean find(int key) {
		    if (root == null)
		        return false; // tree is empty
		    Node curr = root;

		    // while not found
		    while (curr.key != key) {
		        if (curr.key < key) // go right
		            curr = curr.right;
		        else
		            // go left
		            curr = curr.left;
		        if (curr == null) // not found
		            return false;
		    }
		    return true; // found
		}
3. void insert(int key, double value)
注意需要记录一个parent，这样找到spot以后，可以保存其父节点的指针。
		public void insert(int key, double value) {
		    Node newNode = new Node(key, value);
		    if (root == null) // empty tree
		        root = newNode;
		    else {
		        Node parent = root; // keep track of parent
		        Node curr = root;

		        while (true) {
		            // no duplicates allowed
		            if (key == curr.key)
		                return;
		            parent = curr;

		            if (key < curr.key) { // go left
		                curr = curr.left;
		                if (curr == null) { // found a spot
		                    parent.left = newNode;
		                    return;
		                }
		            } else { // go right
		                curr = curr.right;
		                if (curr == null) { // found a spot
		                    parent.right = newNode;
		                    return;
		                }
		            } // end of if-else to go right or left
		        } // end of while
		    } // end of if-else to check empty BST or not
		} // end of insert method

4. void delete(int key)
		public void delete(int key) {
				// the tree is empty
				if (root == null)
					return;

				Node curr = root;
				Node parent = root;
				boolean isLeftChild = true; // flag to check left child

				while (curr.key != key) {
					parent = curr;
					if (key < curr.key) { // go left
						isLeftChild = true;
						curr = curr.left;
					} else {
						isLeftChild = false;
						curr = curr.right;
					}

					if (curr == null) // case 1: not found
						return;
				}

				if (curr.left == null && curr.right == null) { // case 2: leaf
					if (curr == root)
						root = null;
					else if (isLeftChild)
						parent.left = null;
					else
						parent.right = null;
				} else if (curr.right == null) { // case 3: no right child
					if (curr == root)
						root = curr.left;
					else if (isLeftChild)
						parent.left = curr.left;
					else
						parent.right = curr.left;
				} else if (curr.left == null) { // case 3: no left child
					if (curr == root)
						root = curr.right;
					else if (isLeftChild)
						parent.left = curr.right;
					else
						parent.right = curr.right;
				} else { // case 4: with two children HARDEST!
					Node successor = getSuccessor(curr);

					if (curr == root)
						root = successor;
					else if (isLeftChild)
						parent.left = successor;
					else
						parent.right = successor;

					// need to take care of final connection with curr's left
					successor.left = curr.left;
				}
			}

	getSuccessor():找到某节点下方的下一个节点（如果有的话）。是右子树的最左子树。注意这里说的successor不含上面的，因这种情况下不会有上面是successor的情况。
			private Node getSuccessor(Node keyNode) {
				Node successorParent = keyNode;
				Node successor = keyNode;
				Node curr = keyNode.right;

				// move to left as far as possible in the right subtree
				while (curr != null) {
					successorParent = successor;
					successor = curr;
					curr = curr.left;
				}

				/*
				 * if successor is not the right child of the node to be deleted Then
				 * need to take care of two connections in the right subtree
				 */
				if (successor != keyNode.right) {
					successorParent.left = successor.right; // link the right subtree of successor to its parent on the left
					successor.right = keyNode.right; // the right tree of the deleted node
				}

				return successor;
			}

5. Iterator iterator()		
	- 方法：非递归的dfs，要保存遍历的位置，这里用栈。
	- 为什么？实际上就是走迷宫，顺序是指定顺序（一般是中序），那么每次走完左死角，需要能够回来，走右边的那个死角。stack记录了走的路径，回到其top的元素，就是上一个分叉口的记号。

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

为什么用stack？想象一下二叉树，我们要压平它，一个node的左边和右边，决定了iteration能捉到的部分。所以，我们就得找一个node的前边和后边，previous和next（所谓successor就是next）。那么，怎么找一个node的前边和后边？其实只有几种情况。我们以后边next元素为例。一个node的next，要么是其右子树的最左端，要么是其ancestor中第一个处于它右边的。前者比较好找，但后者不好记录，这就是为什么我们要用栈来记录之。
栈到底记录了什么？当前节点的右父辈。
为什么是O(1)？均摊分析。

[二叉树题目总结](http://blog.csdn.net/luckyxiaoqiang/article/details/7518888)

###多叉树

##问题
1. 树和divide and conquer不一样，后者的子问题是每一个都contribute to result的，但树搜索的结果不一定是。
