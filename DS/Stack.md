{{TOC}}
#Stack
##问题
我们刚刚介绍了几种线性的数据结构，有Array，ArrayList, LinkedList，和几种线程安全的List结构。日常生活中，很多操作都需要线性数据结构来记录，比如课程名单等等。

互联网已经成为我们生活不可或缺的一部分，我们在使用浏览器浏览网页时，经常需要点击浏览器的“后退”键，它帮助我们链接到当前页面的上一个页面。我们应当使用什么样的数据结构来实现这个“后退”功能呢？（我们暂时不考虑很多浏览器也有的“前进”功能）。

我们能够想到，其实以前学过的几种线性数据结构，几乎都可以解决这个问题。比如ArrayList，可以随着浏览过程，把我们浏览过的网页地址一个个add进去，需要后退的时候就把当前网页地址remove，再把它前边的那个网页地址提取出来，就可以了。这个操作的复杂度是O(1)。【图】
![](http://cs.lmu.edu/~ray/images/arraystack.gif)

再比如LinkedList，每浏览一个网页，就把它link到当前list的最前端，要回到上一个网页，只需要把head节点的网页remove，新的head就是所需要的网页了。这个操作的复杂度也是O(1)。
![](http://cs.lmu.edu/~ray/images/linkedstack.gif)

不过，fixed size的array是不可以的，因为网页太多，array不会动态更新长度，就溢出了。如果这个array是动态的，在java里面，就是ArrayList了。

（冯老师七个字）

可见，ArrayList和LinkedList，都可以实现我们想要的功能。只是，每次都进行复杂的remove、提取操作，比较麻烦，而类似”网页后退“的功能，我们经常会用到，需要更方便快捷的数据结构来处理这个功能。

（也可以两个结合）

这种功能，叫做LIFO：last in first out。我们在美国都见过餐馆弹出盘子的机器，新盘子放在上面，客人来了，先把最上面的盘子拿走，然后再拿它下面的。因为第一个弹出来的，是最后一个进去的，所以叫做LIFO。我们浏览网页，要访问上一个网页，就要把上一个放进去的网页地址拿出来，所以也是LIFO的功能。【图】

![](http://cs.lmu.edu/~ray/images/stack.gif)

一种叫做“栈”（Stack）的数据结构，就很好地实现了这个功能。不仅复杂度都是O(1)，而且“放入”和“弹出”这两个核心操作，非常方便。

如果我们不使用栈，用ArrayList和LinkedList也能完成这些功能，但问题有两个。第一，要多写一代码，容易出错。第二，我们上次介绍过，ArrayList和LinkedList都不是线程安全的，但Java里面，”栈“是线程安全的，它继承了上次提到过的Vector类。

##功能、方法、复杂度
以下是Stack各主要功能的时间复杂度：
|DS |Push |Pop |Peek |Empty Check |
|:--- |:---- |:----:| ----:| ----:| ----:|
|Stack| O(1) | O(1) | O(1)  | O(1)   |

Stack有四个核心功能：
- push: 把元素放入栈的顶端，就像把盘子放进弹槽。
- pop: 把栈顶端的元素弹出来并返回它的引用，就像拿走顶端的盘子。
- peek: 返回栈顶端的元素的引用（“看到顶端的盘子”），但不弹出来。
- isEmpty: 查看栈是否为空，即是否没有盘子了。

Java的Stack还可以search，复杂度O(N)。

对应的java方法：
	void push(AnyType e);//O(1)
	AnyType pop();//O(1)
	AnyType peek();//O(1)
	boolean isEmpty();//O(1)
	int search(AnyType e);//O(N)

我们可以看到，四个核心操作的复杂度都有良好的性质，都是常数时间O(1)。而且ArrayList、LinkedList和Vector的实现方式，都是O(1)。为什么？虽然实现方法可以各自不同，但都是线性数据结构，顺序地放进元素，再弹出它最末端（也就是“最新放入”）的元素，这几个操作必然是O(1)的。

特别注意，Java中的栈是线程安全的数据结构，不必担心操作过程中被其他线程干扰。

##实现
1. Vector
为何Java栈是线程安全的？因为在Java中，栈是用Vector实现的。【Vector在Java中很少被直接使用，大多用于实现其他的数据结构和功能。】

Vector就是一种线程安全的动态数组（Java中就是线程安全的ArrayList），所以用Vector实现栈的思路很简单，和上文所说ArrayList的实现思路基本是一样的：push即元素加入Vector的末尾，pop即remove末尾元素，peek即返回末尾元素但不remove，isEmpty直接查看Vector的size是否为0。

	public E push(E item) {
	    addElement(item);
	    return item;
	}
	
	public synchronized E pop() {
	    E obj;
	    int len = size();
	
	    obj = peek();
	    removeElementAt(len - 1);
	
	    return obj;
	}
	
	public synchronized E peek() {
	    int len = size();
	
	    if (len == 0)
	        throw new EmptyStackException();
	    return elementAt(len - 1);
	}
	
	public boolean isEmpty() {
	    return size() == 0;
	}

用Vector实现Stack是Java的设计缺点之一。为什么？慢，线程安全性质不必要。所以我们需要学习自己实现Stack。

2. 此外，Stack功能也可以用LinkedList实现。在Java中，对一个LinkedList进行addFirst操作就可以加入元素，removeFirst就可以弹出元素。如果我们手头只有一个LinkedList，就可以用这两种操作来模拟出Stack的功能。
Cache not friendly

3. 最后，很多时候我们还需要用Array以十分“原始”的方式实现Stack。现在介绍一种简单的实现，我们确定元素数不会超过Array的初始长度。

	思路：
	- 维护一个顶端index为top，初始值为-1，这里很tricky，当Stack为空时，顶端元素不存在，所以top是-1。
	- 标识底层数组中的尾端元素，每push一个元素，先检查top是否等于arr.length - 1，如果等于，就抛异常表示底层数组已满。如果没有满，将arr[++top]设定为该元素。
	- pop元素是相反过程，先看数组是否为空，如果为空则返回null；如果不为空，先将arr[top]提取出来，再设定它为null，然后top- -，然后返回提取的元素。
	- isEmpty直接检查top是否为-1，这个很tricky，我们下意识会想要检查arr.length，但我们想看的其实并不是数组的长度，而是数组里面有多少个元素。弹出最后一个元素的时候，top- -，就成了-1。

			@Override
			public void push(AnyType e) {
				if(top == array.length-1)
					throw new StackException("Stack is full");
				array[++top] = e;	
			}
			
			@Override
			public AnyType pop() {
				if(isEmpty()) 
					return null;
				
				AnyType e = array[top];
				array[top] = null;
				top--;
		 		return e;
			}
			
			public AnyType peek() {
				if(isEmpty()) 
					throw new StackException("Stack is full");
				return array[top];
			}
			
			@Override
			public boolean isEmpty() {
				return top == -1;
			}

##注意事项
1. Java栈是线程安全的。
2. 栈的基本功能是push/pop/peek/isEmpty，它没有定位功能，虽然是线性，但无法替代ArrayList和LinkedList。
3. minStack：每次弹出的都是最小值（而不是顶端值）的Stack。
常用于颠倒顺序。
算法题里讲。
[Min Stack](http://www.jiuzhang.com/solutions/min-stack/)
解决的方案是再维护一个栈，我们称为最小栈，如果遇到更小的值则插入最小栈，否则就不需要插入最小栈（注意这里正常栈是怎么都要插进去的）。这里的正确性在于，如果后来得到的值是大于当前最小栈顶的值的，那么接下来pop都会先出去，而最小栈顶的值会一直在，而当pop到最小栈顶的值时，一起出去后接下来第二小的就在pop之后最小栈的顶端了。如此push时最多插入两个栈一个元素，是O(1)，top是取正常栈顶，自然是O(1)，而pop时也是最多抛出两个栈的栈顶元素，O(1)。最后getMin只需要peek最小栈顶栈顶即可，所以仍是O(1)，实现了所有操作的常量操作，空间复杂度是O(n)，最小栈的大小。

##应用
栈思想的应用：
1. 底层的栈（Stack）和栈桢（Stack Frame）。我们call函数的时候这些函数的信息也是用栈的方式管理的，出现错误的时候，可以从上到下看到发生错误的函数，最上面的是最近一次call的。
2. 程序里面检测花括号是否对称。
“我们很快想到了可以用堆栈来解决，思路很简单，我们构造一个堆栈，这个堆栈中的每个元素都包含2部分，一是当前的括号是开括号还是闭括号，当前括号在数学表达式中的位置。 然后我们从左到右扫描这个表达式，一旦找到小括号，就和栈顶的符号比较，如果和栈顶的符号一致，那么就说明是同向括号，于是把这个新找到的括号压入栈顶，如果和栈顶的符号相反（比如当前栈顶是开括号，你刚好查到了一个闭括号），说明找到了一个匹配的括号，于是就把当前括号和栈顶括号连同他们的位置信息一起输出。 最后如果表达式开闭括号数量不对等，那么一定在扫描完表达式之后，堆栈中内容不为空，于是把这些不匹配的括号也依次输出。”
Unlatch