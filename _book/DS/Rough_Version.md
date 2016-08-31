[TOC]

#Big O
##Definition
Big O is the way of measuring the efficiency of an algorithm and how well it scales based on the size of the dataset. Big O is a way of measuring how an algorithm scales.  Big O references how complex an algorithm is.
就是计算次数随n变化的情况，比如n就是线性的变化，n^2^就是二次的变化。
低一级不算在内，O(N^2^ + 2N)即可表示为O(N^2^)。
线性系数都是1，O(2N)也可表示为O(N)。为什么？
Best, worst and expected case.

##Most Common
- O(n!)
  - N! = 1 * 2 * 3…(N-1) * N. The running time increases even  more than exponential.  
- O(2^n^)
  - Exponential
  - Fibonacci
    	public int Fibonacci(n) {
    		if (n == 1 || n == 2)
    			return 1;
    		if (n > 2)
    			return Fibonacci(n-1) + Fibonacci(n-2);
    	}
    为什么是2^n^?
    每一个stack都长出两个stack，直到hit 2或1为止，等于是一个层数为n - 1的（几乎）完全的二叉树，每一层约有2^i^项，i = 0, … , n-1。根据等比数列求和公式，一共有2^n-1^ - 1个节点。所以复杂度是2^n^。
    如同求“所有的组合”一样，2^n^的复杂度经常是2^0^+2^1^+....+2^n-1^导致的。
- O(n^2^)
  - Quadratic
  - N doubles, running time increases 4 fold.(2N)^2 = 4(N^2^)
  - 常见于嵌套for loop里面，如BubbleSort。
- O(n log n)
  - Simply means a product of a linear and a logarithmic term. In many cases, linearithmic time is the result of performing O(logN) operation N times or performing O(N) operation log N times.  
  - 常见：QuickSort, MergeSort，分log n层，每一层n次operation。
- O(n)
  - The running time grows as much as N grows. (When N doubles, so does  the running time.) 
  - 常见于loop整个input的情况：LinearSearch
- O(log n)
  - An algorithm gets slightly slower as N grows. 
  - 常见：BinarySearch 
- O(1)
  - An algorithm does NOT depend on the input size.

##Figure Out During Interview

#Collection/Container
这里面说的collection/container只是一个暂时的指称，不是Java Collection
[Java Collection](http://www.programcreek.com/java-collections/)
Conceptually, collections are objects that group other objects.
In Java, collections are a set of interfaces and classes that are available for you to use in the java.util package.
##Basic Functions
- Addition (Insertion)
- Removal (Deletion)
- Sorting
- Searching
- Iteration (Traversal)
- Copying (Cloning)

##Diagrams and Parts
[Java Collection Diagram and Parts](http://www.programcreek.com/2009/02/the-interface-and-class-hierarchy-for-collections/)

##List
###List Interface
[Comparison of ArrayList, LinkedList and Vector Classes](http://www.programcreek.com/2013/03/arraylist-vs-linkedlist-vs-vector/)

- List是有序的Collection，使用此接口能够精确的控制每个元素插入的位置。用户能够使用索引（元素在List中的位置，类似于数组下标）来访问List中的元素，这类似于Java的数组。
- 和下面要提到的Set不同，List允许有相同的元素。
- 除了具有Collection接口必备的iterator()方法外，List还提供一个listIterator()方法，返回一个 ListIterator接口，和标准的Iterator接口相比，ListIterator多了一些add()之类的方法，允许添加，删除，设定元素， 还能向前或向后遍历。
- 实现List接口的常用类有LinkedList，ArrayList，Vector和Stack。 

###Arrays
1. Methods
   - 等同：
     - == : check if they are the same object
     - arr.equals : also same object
     - Arrays.equals : same content (Shallow)
     - Arrays.deepEquals : same content (deep)
   - 复制：
     [java copy](http://www.cnblogs.com/yxnchinahlj/archive/2010/09/20/1831615.html)
     - Arrays.copyOf(AnyType[] arr, int len)
       len can be any length: for that longer than arr. length, just compensate with default value.
     - System.arraycopy(AnyType[] ore, int start, AnyType[] dest, int end, it len)
     - arr.clone()
     - 所有的copy都是shallow copy吗？如何Deep copy？

       	/* Shallow  Copy */
       	int[] f = {1,2,3,4,5};
       	int[] g = new int[f.length]; 
       	System.arraycopy(f, 0, g, 0, 3); 
       	System.out.println(Arrays.toString(g));// {1,2,3,0,0}
       	
       	int[] h = Arrays.copyOf(a, a.length);
       	System.out.println(Arrays.toString(h));//{1,2,3,4,5}
       	
       	int[] i = {1,2,3,4,5}; 
       	int[] j = i.clone();
       	System.out.println(Arrays.toString(j));//{1,2,3,4,5}
   - 排序： Arrays.sort(T[] a, (optional)int fromIndex, (optional)int toIndex, (optional)Comparator<? super T> c)
     - 升序，该函数的变体可以指明排序index（start, end），还可以放进Comparator。
     - 内部实现比较复杂，小于O(n log n）。
2. 功能
   - insert / delete: 要移动后面的元素，worst case O(N)
   - Sort：我们学过的是O(n log n)，用quickSort或mergeSort。但实际的排序方法更复杂，表现好于此。
   - Search: 
     - Unordered: Linear Search (O(N))
     - Ordered: Binary Search (O(log N))
3. 注意：
   - FIXED Length (Immutable)
     How to delete an element from an array and return a new array of 1 less size?
   - No Holes
     array的两个原则：fixed length & no holes，这导致了add和delete的worst case是线性时间。
   - 线程安全？
4. 适用
   Small and predictable length

###ArrayList
1. Methods
   - Specific
      - add(object) : adds a new element to the end 
        - add(index, object) : inserts a new element at the specified index 
        - set(index, object) : replaces an existing element at the specified index with the new element.   
        - get(index) : returns the element at the specified index. 
        - remove(index) : deletes the element at the specified index. 
        - size() : returns the number of elements.
      - size，isEmpty，get，set方法运行时间为常数。
      - add, remove运行时间为分摊的常数， worst case O(N)。
   - General
      - sort(List<T> list, Comparator<? super T> c)
        Fewer than n(log n).Tim Peters's list sort for Python (TimSort).
2. Functions
   - Insert/delete: O(N)移动剩余元素。
   - Sort: same as array
   - Search: same as array
3. Implementation
   [Simplified Implementation](http://www.java2novice.com/java-interview-programs/arraylist-implementation/)
   - EnsureCapacity
     每个ArrayList实例都有一个容量（Capacity），即用于存储元素的数组的大小。这个容量可随着不断添加新元素而自动增加，但是增长算法并没有定义。当需要插入大量元素时，在插入前可以调用ensureCapacity方法来增加ArrayList的容量以提高插入效率。目前java所用的增长函数是(oldCapacity*3)/2 + 1;
     	public boolean add(E e) {
     		ensureCapacity(size+1); // Increments modCount!! 
     		elementData[size++] = e;
     		return true;
     	}
     		
     	public void ensureCapacity(int minCapacity) { 
     		modCount++;
     		int oldCapacity = elementData.length; 
     		if (minCapacity > oldCapacity) {
     			Object oldData[] = elementData;
     			int newCapacity = (oldCapacity*3)/2 + 1; 
     			if (newCapacity < minCapacity)
     				newCapacity = minCapacity;
     			elementData = Arrays.copyOf(elementData, newCapacity);
     		}
     	}

   - TrimToSize
     Also, those remove methods do not reduce the size of the underlying  array, which can be a concern in terms of memory usage. But, good  news is that arrays only hold references to each object and that is not big.
     If you had any issue with this unused memory, you need to manually call *trimToSize()* method to free the memory.  

     注意：ArrayList仍然是array表示的，所以仍然符合array的两个原则：fixed length & no holes。这导致了add和delete的worst case是线性时间。

   - Insert/Remove
     	public Object remove(int index){
     	        if(index < actSize){
     	            Object obj = myStore[index];
     	            myStore[index] = null;
     	            int tmp = index;
     	            while(tmp < actSize){
     	                myStore[tmp] = myStore[tmp+1];
     	                myStore[tmp+1] = null;
     	                tmp++;
     	            }
     	            actSize--;
     	            return obj;
     	        } else {
     	            throw new ArrayIndexOutOfBoundsException();
     	        }
     	 }
   - Iterator
     [Iterator vs. ListIterator](http://beginnersbook.com/2014/06/difference-between-iterator-and-listiterator-in-java/)
     关键：是否可以在Iterate的过程中修改。
4. Notes
   - Dynamic length
   - Thread Security
     Unsynchronized; efficient, but insecure.
     源码实现中有一个modCount，它的作用在于：iterator遍历ArrayList的过程中，如果其他线程改变了该ArrayList的内在结构，需要通知iterator，及时抛出异常，就是检查modCount的数量。

     	This field is used by the iterator and list iterator implementation returned by the iterator and listIterator methods. If the value of this field changes unexpectedly, the iterator (or list iterator) will throw a ConcurrentModificationException in response to the next, remove, previous, set or add operations. This provides fail-fast behavior, rather than non-deterministic behavior in the face of concurrent modification during iteration.
   - Amortized Analysis
     ArrayList的append，一般是O(1)，个别时候需要expand array，就不是O(1)了，但均摊到每个操作，还是O(1)。
     In computer science, amortized analysis is a method for analyzing a given algorithm's time complexity, or how much of a resource, especially time or memory in the context of computer programs, it takes to execute. The motivation for amortized analysis is that looking at the worst-case run time per operation can be too pessimistic.
     [Wiki Amortized Analysis](https://en.wikipedia.org/wiki/Amortized_analysis)
     [中文讲解](http://blog.csdn.net/touzani/article/details/1696399)
5. Applicability
   Small, unpredictable length.
###LinkedList
1. 原理和示意图
   Memory usage: 4 bytes more than ArrayList.
2. Methods
   - O(1)
     	addFirst(element: Object) : void 
     	addLast(element: Object) : void 
     	getFirst() : Object
     	getLast() : Object 
     	removeFirst() : Object 
     	removeLast() : Object
   - O(N)
     	insertAfter(AnyType key, AnyType toInsert) : void
     	insertBefore(AnyType key, AnyType toInsert) (triky) : void
     	iterator() : Iterator<AnyType>
   - 在java中LinkedList Class并不能在iterator方法以外直接访问next/prev/data等instance variable，它是被封装的。但是，LeetCode中经常会有直接使用LinkedList的ivar的题目，此时可以直接访问。
   - LinkedList提供额外的get，remove，insert方法在 LinkedList的首部或尾部。这些操作使LinkedList可被用作堆栈（stack），队列（queue）或双向队列（deque）。
3. Functions
   - Insertion/Deletion: O(1)
   - Sort: “Collections.sort uses a workaround there: it copies the contents to an array, sorts the array and copies it back. This will not take much longer than for an ArrayList”
   - Search: O(N)，不适合搜索，也无法直接access to particular element (by index in array and ArrayList）。
4. Implementation
   - Nested class: static and non-static
     static是附着在class上面的，所以不能access外面的outer class的内容; non-static是附着在object上面的，所以可以access外面的outer class的内容，即使private也可以。
     	class OuterClass { ....
     		class NestedClass { ....
     		} 
     	}
   - Node
     	private static class Node<AnyType> {
     		private AnyType data;
     		private Node<AnyType> next;
     		
     		public Node(AnyType data, Node<AnyType> next) {
     			this.data = data;
     			this.next = next;
     		}
     	}
   - Iterator
     - To iterate over the dataset, the data structure should implement Iterable interface and it also requires a class that implements the Iterator interface.一般都是嵌套结构：返回一个实现Iterable interface的类，这个类Override了需要的方法。
       ​	
       	public Iterator<AnyType> iterator() {
       		return new SinglyLinkedListIterator();
       	}
       	
       	private class SinglyLinkedListIterator implements Iterator<AnyType> {
       	
       		private Node<AnyType> nextNode;
       		
       		public SinglyLinkedListIterator() {
       			nextNode = head;
       		}
       		
       		public boolean hasNext() {
       			return nextNode!=null;
       		}
       	
       		public AnyType next() {
       			if(!hasNext()) throw new NoSuchElementException();
       			AnyType result = nextNode.data;
       			nextNode = nextNode.next;
       			return result;
       		}
       	
       		public void remove() {
       			throw new UnsupportedOperationException();		
       		}
       		
       	}
   - Other methods
     - 手动遍历，返回最后一个Node。不在API中，但是会很常用类似的操作。
       	private ListNode<T> lastNode(ListNode<T> head) {
       		if (head == null) // Remember to check head because you are going to use head.next.
       			return null;
       		ListNode<T> tmp = head;
       		while (tmp.next != null)
       			tmp = tmp.next();
       		return tmp; // tmp is the tail Node
       	}
     - insertBefore[^Check]
       难点：如果是insertAfter，那么scan到该元素所在的Node，后面加一个Node就行了。但insertBefore，需要在scan中同时记录当前元素和下一个元素，检测到下一个元素含key的时候，当前元素后面加新Node。这种方法产生一个边缘case，即head本身就含有key，head不是任何东西的next，所以检测不出来，需要另外开一个case。
       	public void insertBefore(AnyType key, AnyType toInsert) {
       		Node<AnyType> tmp = head;
       		if (head == null)
       			return;
       		// Edge case for head containing key, insert a new node before head
       		if (head.data.equals(key)) {
       			head = new Node<AnyType>(toInsert, head);
       			return;
       		}
       	
       		while (tmp.next != null && !tmp.next.data.equals(key))
       			tmp = tmp.next;
       	
       		if (tmp.next != null) {
       			tmp.next = new Node<AnyType>(toInsert, tmp.next);
       		}
       	}
     - remove[^CHECK]
       	public void remove(AnyType key) {
       		if (head == null)
       			return;
       		if (head.data.equals(key)) {
       			head = null;
       		}
       		else {
       			Node<AnyType> tmp = head;
       			while (tmp.next != null && !tmp.next.data.equals(key))
       				tmp = tmp.next;
       			if (tmp.next != null)
       				tmp.next = tmp.next.next;
       		}
       	}
     - reverseLinkedList
       一般API里不会有，但是是经典考题。给一个head，把list的连接关系反过来，返回一个linkedList的新head。
       有recursive和iterative两种经典的写法。
       	// Iterative
       	public ListNode reverseList(ListNode head) {
       		if (head == null || head.next == null) return head;
       		ListNode prev = null, cur = head;
       		ListNode next;
       		
       		while (cur != null) {
       			next = cur.next;
       			cur.next = prev;
       			prev = cur;
       			cur = next;
       		}
       		
       		return prev;
       	}
       	
       	// Recursive
       	public ListNode reverseList(ListNode head) {
       		if (head == null || head.next == null) return head;
       		ListNode second = head.next;
       		head.next = null;
       		
       		ListNode rest = reverseList(second);
       		second.next = head;
       		
       		return rest;
       	}
   - 总结：关于LinkedList的实现，最重要的是node之间loop的方法。注意check head，一个是是否为null，一个是是否contain key。任何时候都要check head是否为null。还要注意如果loop的是.next域，head的data可能扫描不到，要特别开一个edge case扫描一遍。
5. Notes
   - Thread
     不安全。
     注意LinkedList没有同步方法。如果多个线程同时访问一个List，则必须自己实现访问同步。一种解决方法是在创建List时构造一个同步的List：
     	List list = Collections.synchronizedList(new LinkedList(...));

6. Applicability
   经常插入删除，但不经常access to particular element。

###Vector
1. 原理及示意图
   [TutorialsPoint Vector Class](http://www.tutorialspoint.com/java/java_vector_class.htm)
   [Vector vs. ArrayList](http://beginnersbook.com/2013/12/difference-between-arraylist-and-vector-in-java/)
   - Vector implements a dynamic array. It is similar to ArrayList, but with two differences:
     - Vector is synchronized 线程安全. This means if one thread is working on Vector, no other thread can get a hold of it. Unlike ArrayList, only one thread can perform an operation on vector at a time.
     - Vector contains many legacy methods that are not part of the collections framework.
   - Vector proves to be very useful if you don't know the size of the array in advance or you just need one that can change sizes over the lifetime of a program.
2. Methods
3. Functions
4. Implementation
5. Notes
   - Fail Fast
     If the Vector is structurally modified at any time after the Iterator is created, in any way except through the Iterator's own remove or add methods, the Iterator will throw a ConcurrentModificationException.
     ArrayList iterator is also fail-fast.
   - Actually it is NOT part of java Collections.
6. Applicability

###Synchronized List
[JavaDoc Collections.synchronizedList](https://docs.oracle.com/javase/7/docs/api/java/util/Collections.html#synchronizedList(java.util.List))
Returns a synchronized (thread-safe) list backed by the specified list. In order to guarantee serial access, it is critical that all access to the backing list is accomplished through the returned list.
It is imperative that the user manually synchronize on the returned list when iterating over it

	//It is imperative that the user manually synchronize on the returned list when iterating over it:
	List<Integer> arrList = new ArrayList<Integer>();
	arrList.add(1);
	arrList.add(101);
	List list = Collections.synchronizedList(arrList);
	synchronized(list) {
		Iterator i = list.iterator(); // Must be in synchronized block
		while (i.hasNext())
			System.out.println(i.next());
	}
##Stack/Queue
###Stack
1.    原理和示意图
      LIFO container.
2.    Methods
      void push(AnyType e);//O(1)
      AnyType pop();//O(1)
      AnyType peek();//O(1)
      boolean isEmpty();//O(1)
3.    Functions
      LIFO，此功能可以用LinkedList在不实现class的情况下实现：addFirst/removeFirst
4.    Implementation
      Array/ArrayList/LinkedList.
      以下实现，没有处理overflow问题。

      	public class ArrayStack<AnyType> implements StackInterface<AnyType> {

      		private static final int DEFAULT_CAPACITY = 10;
      		private AnyType[] array;
      		private int top;
      		
      		public ArrayStack() {
      			this(DEFAULT_CAPACITY);
      		}
      		
      		/**
      *   construct a new stack with initial capacity
          *  @param initialCapacity an initial length of the stack
              */
             @SuppressWarnings("unchecked")
             public ArrayStack(int initialCapacity) {
             	if(initialCapacity <= 0)
             		array = (AnyType[]) new Object[DEFAULT_CAPACITY];	
             	else 
             		array = (AnyType[]) new Object[initialCapacity];
             	top = -1;
             }

             /**
             * add a new element onto the top of the stack
                */
               @Override
               public void push(AnyType e) {
               if(top == array.length-1)
               	throw new StackException("Stack is full");
               array[++top] = e;	
               }

             /**
             *  get and delete (pop) an element from the top of the stack
                 */
                @Override
                public AnyType pop() {
                if(isEmpty()) 
                	return null;

                AnyType e = array[top];
                array[top] = null;
                top--;
                 return e;
                }

             /**
             *  get (peek) the top element. Does not delete it!
                 */
                @Override
                public AnyType peek() {
                if(isEmpty()) 
                	throw new StackException("Stack is full");
                return array[top];
                }

             /**
             * check if the stack is empty or not
                 */
                @Override
                public boolean isEmpty() {
                return top == -1;
                }

          }

          @SuppressWarnings("serial")
          class StackException extends RuntimeException {
          	public StackException(String message) {
          		super(message);
          	}
          	
          	public StackException() {
          		super();
          	}
          }

NOTE: need to free memory by setting to null when pops out.
1. Notes
   - stack和queue互相转化。
2. Applicability
   常用于颠倒顺序。
   算法题里讲。
   [Min Stack](http://www.jiuzhang.com/solutions/min-stack/)
   解决的方案是再维护一个栈，我们称为最小栈，如果遇到更小的值则插入最小栈，否则就不需要插入最小栈（注意这里正常栈是怎么都要插进去的）。这里的正确性在于，如果后来得到的值是大于当前最小栈顶的值的，那么接下来pop都会先出去，而最小栈顶的值会一直在，而当pop到最小栈顶的值时，一起出去后接下来第二小的就在pop之后最小栈的顶端了。如此push时最多插入两个栈一个元素，是O(1)，top是取正常栈顶，自然是O(1)，而pop时也是最多抛出两个栈的栈顶元素，O(1)。最后getMin只需要peek最小栈顶栈顶即可，所以仍是O(1)，实现了所有操作的常量操作，空间复杂度是O(n)，最小栈的大小。


###Queue
1. 原理和示意图
   FIFO.两道门。
   Not thread safe.
2. Methods
   	public interface QueueInterface<AnyType> { 
   		void enqueue(AnyType e);// O(1)
   		AnyType dequeue(); // O(1)
   		AnyType peekFront(); // O(1)
   		boolean isEmpty();// O(1)
   	}
3. Functions
4. Implementation
   用Array, ArrayList, LinkedList都可以，还可以用stack。用Array的话，两个难点：circular, overflow。
   08722提供的实现，没有处理overflow问题。circular则是用mod实现的。
   - Enqueue(AnyType e)
     	@Override
     	public void enqueue(AnyType e) {
     		if(isFull()) 
     			throw new RuntimeException("Queue is full");
     		back++;
     		int index = back % array.length; // circular
     		array[index] = e;
     		nItems++;
     	}

   - Dequeue()
     	@Override
     	public AnyType dequeue() {
     		if(isEmpty())
     			throw new NoSuchElementException();
     	
     		AnyType e = array[front%array.length];
     		array[front%array.length] = null;
     		front++; // front moves up to the right
     		nItems--;
     		return e;
     	}


[Linked Structure Implementation](http://algs4.cs.princeton.edu/43mst/Queue.java.html)
[Linked Structure Implementation2](http://www.tutorialspoint.com/javaexamples/data_queue.htm)

[Implement Queue Using Stacks](http://www.jiuzhang.com/solutions/implement-queue-by-two-stacks/)

1. Notes
   - Not thread safe.
2. Application
   多用于队列场合，如nodeJS的EventQueue。还有很重要的，BFS也用queue来储存需要遍历的Nodes。

#### Blocking Queue
1. 原理和示意图
   [Blocking Queue](http://codereview.stackexchange.com/questions/7002/java-blocking-queue)
2. Methods
3. Functions
4. Implementation
5. Notes
6. Applicability

####ArrayQueue
1. 原理和示意图
   [ArrayDequeue](http://blog.csdn.net/youxin2012/article/details/50576041)
2. Methods
3. Functions
4. Implementation
5. Notes
6. Applicability


##Set
![](http://www.programcreek.com/wp-content/uploads/2009/02/java-collection-hierarchy.jpeg)

1. Set最大的特点是不能duplicate。Set interface extends Collection interface. In a set, no duplicates are allowed. Every element in a set must be unique. You can simply add elements to a set, and duplicates will be removed automatically.
   [TutorialsPoint Java Set Interface](http://www.tutorialspoint.com/java/java_set_interface.htm)
2. 实现
   [HashSet vs. TreeSet vs. LinkedHashSet](http://www.programcreek.com/2013/03/hashset-vs-treeset-vs-linkedhashset/)
   There are 3 commonly used implementations of Set: HashSet, TreeSet and LinkedHashSet. When and which to use is an important question. In brief, if you need a fast set, you should use HashSet; if you need a sorted set, then TreeSet should be used; if you need a set that can be store the insertion order, LinkedHashSet should be used.

   - HashSet is Implemented using a hash table. Elements are not ordered. The add, remove, and contains methods have constant time complexity O(1).
   - TreeSet is implemented using a tree structure(red-black tree in algorithm book). The elements in a set are sorted, but the add, remove, and contains methods has time complexity of O(log (n)). It offers several methods to deal with the ordered set like first(), last(), headSet(), tailSet(), etc.
   - LinkedHashSet is between HashSet and TreeSet. It is implemented as a hash table with a linked list running through it, so it provides the order of insertion. The time complexity of basic methods is O(1).


###TreeSet
1. 原理及示意图
   Red-black tree，always balance, have order.
2. Methods
   - ceiling(e)
   - floor(e)
   - higher(e)
   - lower(e)
   - pollFirst()
   - pollLast()
   - first()
   - last()
   - headSet(e)
   - tailSet(e)
   - descendingSet()
3. Functions
4. Implementation
5. Notes
   - It is generally faster to add elements to a HashSet. And, if you need to traverse the elements in sorted order, convert the collection to a TreeSet.
   - TreeSet是Sorted的，里面的元素必须implement Comparable\<AnyType>。
6. Applicability

###HashSet
1. 原理及示意图
2. Methods
   - add(key) : adds a key to the set. 
   - contains(key) : returns true if the set has that key  
   - iterator() : returns an iterator over the elements 
3. Functions
   - Insertion/Deletion
   - Search
   - NO Sorting because there is no order
   - NO DUPLICATE: can be used to sort out “how many distinct items are there”.
4. Implementation
   It creates HashMap instance and it uses HashMap’s put method to add a new element but with dummy object as its value which means it has  only key.
   And, it adds, or puts, a new element only if it is not already present in  the map. 
   查重：
   It stores and retrieves elements by using a hash function that converts  elements into an integer.

   	public class HashSet<E> implements Set<E> {
   	    private HashMap<E, Object> map;
   	    // Dummy value to associate with an Object in the backing Map
   	    private static final Object PRESENT = new Object();
   	    
   	    public HashSet() {
   	        map = new HashMap<>();
   	    }
   	
   	    public boolean add(E e) {
   	        return map.put(e, PRESENT)==null;
   	    } 
   	} 
5. Notes
6. Applicability

##Map
![](http://www.programcreek.com/wp-content/uploads/2009/02/MapClassHierarchy-600x354.jpg)    

[HashMap vs. TreeMap vs. HashTable vs. LinkedHashMap](http://www.programcreek.com/2013/03/hashmap-vs-treemap-vs-hashtable-vs-linkedhashmap/)
1. 功能
   [TutorialsPoint Java Map Interface](http://www.tutorialspoint.com/java/java_map_interface.htm)
   Key-Value对应，使得insert/delete/contains等操作都是O(1)。
2. Map vs. Set
   Set是不能重复，Map可以重复value，不能重复key。
3. 实现
   - HashMap is implemented as a hash table, and there is no ordering on keys or values.
   - TreeMap is implemented based on red-black tree structure, and it is ordered by the key.
   - LinkedHashMap preserves the insertion order.
   - HashTable is synchronized, in contrast to HashMap. It has an overhead for synchronization.

   This is the reason that HashMap should be used if the program is thread-safe.

###HashMap

1. 原理及示意图
   ![How HashMap Works](http://coding-geek.com/wp-content/uploads/2015/03/internal_storage_java_hashmap.jpg)
   [Hash Map原理及实现1](http://zhangshixi.iteye.com/blog/672697)
   [Hash Map原理及实现2](http://yikun.github.io/2015/04/01/Java-HashMap%E5%B7%A5%E4%BD%9C%E5%8E%9F%E7%90%86%E5%8F%8A%E5%AE%9E%E7%8E%B0/)
   [Hash Map原理及实现3](http://blog.csdn.net/vking_wang/article/details/14166593)
   [How Does a HashMap Work in Java](http://coding-geek.com/how-does-a-hashmap-work-in-java/)
   [常见hash算法](http://blog.csdn.net/eaglex/article/details/6310727)
   [hash算法原理及常见函数](http://blog.csdn.net/pngynghay/article/details/22433715)
   散列表,它是基于快速存取的角度设计的，也是一种典型的“空间换时间”的做法。顾名思义，该数据结构可以理解为一个线性表，但是其中的元素不是紧密排列的，而是可能存在空隙。
   散列表（Hash table，也叫哈希表），是根据关键码值(Key value)而直接进行访问的数据结构。也就是说，它通过把关键码值映射到表中一个位置来访问记录，以加快查找的速度。这个映射函数叫做散列函数，存放记录的数组叫做散列表。

重点：
	- Hash函数的方法hashFunc()
	- Collision Solutions
		- Open Address (Linear Probing…)
		- Separate Chaining (What Java uses).
	- Rehash when collision happens too frequently (Load Factor)
	- Bucket size: 16 by default, load factor is 75%
	- 
1. Methods
   - [Frequently Used Methods of HashMap](http://www.programcreek.com/2013/04/frequently-used-methods-of-java-hashmap/)
     [Traverse Map 4 Methods](http://blog.csdn.net/tjcyjd/article/details/11111401)
     - Sorting
     - Printing
     - Looping
   - Below are all O(1) (amortized) REALLY?!
     - Insertion / Deletion
     - Search for key
     - Retrieval of value
     - CANNOT sort because there is no order; and order may change.
       	V put(K key, V value)
       	V get(Object key)
       	V remove(Object key)
       	Boolean containsKey(Object key)
2. Functions
   - O(1)的insertion, deletion, search, retrieval。
   - 不能保存顺序。
3. Implementation
   需要全部掌握，source code在Try文件夹中。
   [Java HashMap Source](http://developer.classpath.org/doc/java/util/HashMap-source.html)
   - 数据存放：Entry<K, V>是AbstractMap中实现的，存放key, value, hash和链接到同一个hash bucket的下一个Entry的指针next。HashMap中的HashEntry是static class，继承该类，并增加几种新的方法。
     	static class Entry<K,V> implements Map.Entry<K,V> {
     		final K key;
     		V value;
     		Entry<K,V> next;
     		final int hash;
     			//...
     	}
   - put<K,V>
     将定义好的key和value放进HashMap中。先计算出hash code，而后在对应链表中检查是否已存在该key。如存在，更新对应value；如不存在，将新的entry加入该链表。
     	public V put(K key, V value) {
     	  int idx = hash(key); // get the bucket index
     	  HashEntry<K, V> e = buckets[idx];
     	
     	  while (e != null) { // While this hash bucket is not empty
     	    if (equals(key, e.key)) { // If the key already exists, update the value and return old value
     	      e.access(); // Must call this for bookkeeping in LinkedHashMap. In HashMap it does not do anything.
     	      V r = e.value;
     	      e.value = value;
     	      return r; // return the replaced old value
     	    }
     	    else // if the key does not exist, loop through until e is null
     	      e = e.next;
     	  }
     	
     	  // At this point, we know we need to add a new entry.
     	    modCount++; // For fail-fast
     	      if (++size > threshold){ // if there are too many key-value pairs, rehash
     	        rehash();
     	         // Need a new hash value to suit the bigger table.
     	        idx = hash(key);
     	      }
     	
     	  // LinkedHashMap cannot override put(), hence this call.
     	  addEntry(key, value, idx, true); // add a new entry to the beginning of the bucket
     	  return null; // no old value so return null to show key does not exist before
     	}

   loadFactor思想。Java7源码。

   - get\<K>
     得到hash->扫描链表->key相等时取出value->没有key返回null
     	public V get(Object key) 
     	{
     	   int idx = hash(key);
     	   HashEntry<K, V> e = buckets[idx];
     	   while (e != null)
     	   {
     	     if (equals(key, e.key))
     	       return e.value;
     	     e = e.next;
     	   }
     	   return null;
     	}
   - containsKey\<K>
     一样的扫描链表方式，扫到key以后返回true，没扫到返回false。
     	public boolean containsKey(Object key)
     	{
     	   int idx = hash(key);
     	   HashEntry<K, V> e = buckets[idx];
     	   while (e != null)
     	   {
     	     if (equals(key, e.key))
     	       return true;
     	     e = e.next;
     	   }
     	   return false;
     	}
   - remove<K>
   - resize\<int newCapacity>
     当加入新entry以后新size > threshold时call此函数，扩展array长度至newCapacity，java用的是2*oldCapacity（WHY?），并将旧的entries挪移到新的array中来。
     时间复杂度O(N)（挪移时扫描全table）
     难点：挪移。
     注意：较新版本的Java用的index经过了优化处理。

     	// the "rehash" function in JAVA 8 that directly takes the key
     	static final int hash(Object key) {
     	    int h;
     	    return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
     	    }
     	// the function that returns the index from the rehashed hash
     	static int indexFor(int h, int length) {
     	    return h & (length-1);
     	}

Rehash Function in Java 6 (Java 8 is too long…)
			/*Rehashes the contents of this map into a new array with a larger capacity. This method is called automatically when the number of keys in this map reaches its threshold. If current capacity is MAXIMUM_CAPACITY, this method does not resize the map, but sets threshold to Integer.MAX_VALUE. This has the effect of preventing future calls.
				Parameters:
				newCapacity the new capacity, MUST be a power of two; must be greater than current capacity unless current capacity is MAXIMUM_CAPACITY (in which case value is irrelevant).*/
				
			void resize(int newCapacity) {
			   Entry[] oldTable = table;
			   int oldCapacity = oldTable.length;
			   if (oldCapacity == MAXIMUM_CAPACITY) {
			       threshold = Integer.MAX_VALUE;
			       return;
			   }
			
			   Entry[] newTable = new Entry[newCapacity];
			   transfer(newTable);
			   table = newTable;
			   threshold = (int)(newCapacity * loadFactor);
			}


			/* Transfers all entries from current table to newTable. */

			void transfer(Entry[] newTable) {
			   Entry[] src = table;
			   int newCapacity = newTable.length;
			   for (int j = 0; j < src.length; j++) {
			       Entry<K,V> e = src[j];
			       if (e != null) {
			           src[j] = null;
			           do {
			               Entry<K,V> next = e.next;
			               int i = indexFor(e.hash, newCapacity);
			               e.next = newTable[i];
			               newTable[i] = e;
			               e = next;
			           } while (e != null);
			       }
			   }
			}
1. Notes
   - [Java equals() and hashCode() contract](http://www.programcreek.com/2011/07/java-equals-and-hashcode-contract/)实现一个新class的时候需要override: hashCode(), equals()两个方法才能使新class适用于HashMap。
   - Key不能有duplicate，如果有的话就会替换。
2. Applicability
   - 空间换时间，结合了ArrayList和LinkedList的好处：insert/delete/search/retrieval through index都是O(1)（index和search都是key）
###TreeMap
1. 原理和示意图
   是用Red-Black Tree实现的，总是balanced，有order。所以，当我们需要顺序遍历HashMap的时候，就把它转换成TreeMap来遍历。
2. Methods
   - ceilingKey(key) 
   - floorKey(key)
   - higherKey(key) 
   - lowerKey(key) 
   - pollFirstEntry() 
   - pollLastEntry() 
   - firstEntry()
   - lastEntry()
   - firstKey()
   - lastKey()
   - descendingMap()
3. Function
4. Implementation
5. Notes
   - 因为基于HashMap，所以entry如果是新class要override equals()和hashCode()，
   - 同时，entry如果是新class也必须implement Comparable interface，override compareTo()。
   - Key不能有duplicate，如果有的话就会替换，这点和HashMap一样。
6. Applicability

###HashTable
The HashMap class is roughly equivalent to Hashtable, except that it is unsynchronized and permits nulls.


##Tree
树结构可以汇聚ordered array和LinkedList的优点，特别是二叉搜索树：
The answer is because it combines the advantages of two other data  structures: 
- Ordered Array: Search is quick using the binary search
- Linked List: Insert and delete items quickly (in a sense that there 
  is no need to shift other items.) 

###General
1. 原理和示意图
   A tree is an extension of the concept of linked data structures.    
   It has nodes with more than one self#referenced field.  

   - Root: The node at the top of the tree.  
   - Parent: When any node (except the root) has exactly one edge 
   - running upward to another node. The node above is called parent 
   - of the node.  
   - Child: Any node may have one or more lines running downward 
   - to other nodes. These nodes below the given node called its children.  
   - Leaf: A node that has no children is called a leaf. There can be only 
   - one root in a tree but there can be many leaves. 
   - Levels: the level of a particular node refers to how many 
   - generations the node is from the root. The root is level 0 and its 
   - children are level 1 and so on.  
   - Balanced vs. unbalanced
2. Methods
3. Functions
4. Implementation
5. Notes
   - Building block, not usually direct use
6. Applicability

####Traversal
1. Depth First
   “先顺完一个根须，再顺其他根须”，和breadth first一层一层推进的顺序相对。注意顺完一个往回倒的时候是从下往上倒的。
   根据parent和child的visit顺序，分为Pre/Post/In-order，就是指先visit根节点还是其children。
   Pre就是先根节点，后children；In就是左边children-根节点-右边children。
   所有节点都被visit了一遍，所以是O(n)。
2. Breadth First
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

###Binary Tree
[6天通吃树结构-二叉查找树](http://www.cnblogs.com/huangxincheng/archive/2012/07/21/2602375.html)
A binary tree has nodes that contain a data element, a left reference, and  a right reference. In other words, every node in a binary tree can have at  most two children.  
	- A full binary tree is a binary tree where each node has exactly zero or  two children.  
	- A complete binary tree is a binary tree that is completely filled (from left  to right) with the possible exception of the bottom level.

####Binary Search Tree
1. 原理和示意图
   查找树的定义非常简单，一句话就是左孩子比父节点小，右孩子比父节点大，还有一个特性就是”中序遍历“可以让结点有序。
   ![](http://pic002.cnblogs.com/images/2012/214741/2012072113392755.png)
   The defining characteristic, or the ordering invariant, of a  binary search tree: At any node with a key value, k, in a binary  search tree, all keys of the elements in the left sub:tree must be less  than k, while all keys of the elements in the right sub:tree must be  greater than k. (Meaning no duplicates are allowed.) 
   注意是否要allow duplicate取决于设计，如果要allow，那么在结点里要记录该值出现的次数。
2. Methods
   - boolean find(int key)
   - void insert(int key, double value)
   - void delete(int key)
   - void traverse()
   - Iterator<E> iterator()??
3. Functions
   - Insertion
     平均情况下O(log N)，用类似binarySearch的方式找spot，层层推进。有三种可能：重复（不做任何事结束），左树找到SPOT，右树找到SPOT。因为一次cut掉一半，故是O(log N)。
     最坏情况下O(N)（实际上是比input数大或小的元素总数，大or小看链状树在左边还是右边），树不平衡，和LinkedList结构一样，但需要搜索到正确的SPOT。
   - Searching
     平均情况下O(log N)，和binarySearch原理类似（都有order所以都可以搜一次截取一半）。
     最坏情况下O(N)（实际上是比input数大或小的元素总数，大or小看链状树在左边还是右边），树不平衡，和LinkedList结构一样。
   - Deletion
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
   - Traversal 
     和general traversal是一样的，只是left和right child最多只有一个。
4. Implementation
   -    static class Node
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
   -    boolean find(int key)
        	@Override
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
   -    void insert(int key, double value)
        注意需要记录一个parent，这样找到spot以后，可以保存其父节点的指针。
        	@Override
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
   -    void delete(int key)
        	@Override
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
        		} else { // case 4: with two children
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

        getSuccessor() retrieves the smallest node in the right subtree of the node to be deleted, and does relink work for this change (relink the right subtree of successor as the left subtree of its parent, and the successor should have an updated right subtree that was the original right subtree of the deleted node.

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
        *   if successor is not the right child of the node to be deleted Then
            * need to take care of two connections in the right subtree
               */
              if (successor != keyNode.right) {
              	successorParent.left = successor.right; // link the right subtree of successor to its parent
              	successor.right = keyNode.right; // the right tree of the deleted node
              }

              return successor;
              }
   -    void traverse()
        	@Override
        	public void traverse() {
        		inOrderHelper(root);
        		System.out.println("");
        	}
        	
        	// private helper method for inorder traversal
        	private void inOrderHelper(Node toVisit) {
        		if (toVisit != null) {
        			inOrderHelper(toVisit.left);
        			System.out.print("[" + toVisit.key + " " + toVisit.value + "]");
        			inOrderHelper(toVisit.right);
        		}
        	}
   -    Iterator<E> iterator()
        -   方法：非递归的dfs，要保存遍历的位置，这里用栈。
        -   为什么？实际上就是走迷宫，顺序是指定顺序（一般是中序），那么每次走完左死角，需要能够回来，走右边的那个死角。stack记录了走的路径，回到其top的元素，就是上一个分叉口的记号。
        -   极其精巧的设计！
            	/**
            *  Definition for binary tree
               * public class TreeNode {
               * int val;
               * TreeNode left;
               * TreeNode right;
               * TreeNode(int x) { val = x; }
               * }
                  */

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

               /**
               * Your BSTIterator will be called like this:
               * BSTIterator i = new BSTIterator(root);
               * while (i.hasNext()) v[f()] = i.next();
                   */
5. Notes
6. Applicability

###Trie Tree
1. 原理和示意图
   [Trie Tree原理和实现](http://dongxicheng.org/structure/trietree/)
   Trie树，又称字典树，单词查找树或者前缀树，是一种用于快速检索的多叉树结构，如英文字母的字典树是一个26叉树，数字的字典树是一个10叉树。
   Trie一词来自retrieve，发音为/tri:/ “tree”，也有人读为/traɪ/ “try”。
   Trie树可以利用字符串的公共前缀来节约存储空间。如下图所示，该trie树用10个节点保存了6个字符串tea，ten，to，in，inn，int：
   ![](http://dongxicheng.org/wp-content/uploads/2011/04/trietree.jpg)
   在该trie树中，字符串in，inn和int的公共前缀是“in”，因此可以只存储一份“in”以节省空间。当然，如果系统中存在大量字符串且这些字符串基本没有公共前缀，则相应的trie树将非常消耗内存，这也是trie树的一个缺点。
   Trie树的基本性质可以归纳为：
   （1）根节点不包含字符，除根节点意外每个节点只包含一个字符。
   （2）从根节点到某一个节点，路径上经过的字符连接起来，为该节点对应的字符串。
   （3）每个节点的所有子节点包含的字符串不相同。
   leetcode有道题

2. Methods
3. Functions
4. Implementation
   字母树的插入（Insert）、删除（Delete）和查找（Find）都非常简单，用一个一重循环即可，即第i次循环找到前i个字母所对应的子树，然后进行相应的操作。实现这棵字母树，我们用最常见的数组保存（静态开辟内存）即可，当然也可以开动态的指针类型（动态开辟内存）。
   至于结点对儿子的指向，一般有三种方法：
   - 对每个结点开一个字母集大小的数组，对应的下标是儿子所表示的字母，内容则是这个儿子对应在大数组上的位置，即标号；
   - 对每个结点挂一个链表，按一定顺序记录每个儿子是谁；
   - 使用左儿子右兄弟表示法记录这棵树。
     三种方法，各有特点。第一种易实现，但实际的空间要求较大；第二种，较易实现，空间要求相对较小，但比较费时；第三种，空间要求最小，但相对费时且不易写。
     [Implement Trie Tree in Java](http://www.jiuzhang.com/solutions/implement-trie/)
5. Notes
6. Applicability
   第一：词频统计。
   可能有人要说了，词频统计简单啊，一个hash或者一个堆就可以打完收工，但问题来了，如果内存有限呢？还能这么玩吗？所以这里我们就可以用trie树来压缩下空间，因为公共前缀都是用一个节点保存的。

第二: 前缀匹配
就拿上面的图来说吧，如果我想获取所有以"a"开头的字符串，从图中可以很明显的看到是：and,as,at，如果不用trie树，你该怎么做呢？很显然朴素的做法时间复杂度为O(N2) ，那么用Trie树就不一样了，它可以做到h，h为你检索单词的长度，可以说这是秒杀的效果。

###B+ Tree
1. 原理和示意图
   [B,B+,R树](http://blog.csdn.net/v_july_v/article/details/6530142)
2. Methods
3. Functions
4. Implementation
5. Notes
6. Applicability

###Segment Tree
1. 原理和示意图
   [线段树](http://yutianx.info/2014/11/18/2014-11-18-segment-tree/)
2. Methods
3. Functions
4. Implementation
5. Notes
6. Applicability

##Heap
1. 原理和示意图
2. Methods
3. Functions
4. Implementation
5. Notes
6. Applicability

##Summary
| DS                                | Addition | Removal  | Searching | Get Index | Sort |
| :-------------------------------- | :------- | :------: | --------: | --------: | ---: |
| Array                             | O(N)     |   O(N)   |      O(N) |      O(1) |      |
| Ordered Array                     | O(N)     |   O(N)   |  O(log N) |      O(1) |      |
| Linked List                       | O(1)     |   O(1)   |      O(N) |    O(ind) |      |
| Ordered Linked List               | O(N)?    |   O(1)   | O(log N)? |    O(ind) |      |
| Stack                             | O(1)     |   O(1)   |      N.A. |      N.A. | N.A. |
| TreeSet                           | O(log N) | O(log N) |  O(log N) |         ? |      |
| HashSet                           | O(1)     |   O(1)   |      O(1) |         ? |      |
| BST (average)                     | O(log N) | O(log N) |  O(log N) |         ? |      |
| BST (worst)                       | O(N)     |   O(N)   |      O(N) |         ? |      |
| Balanced Tree (average and worst) | is       |    is    |        is |        is |      |
|Hash Map| O(1) | O(1) | O(1)  | N.A. | N.A.

#Sorting and Searching
##Searching
###Linear Search
###Binary Search

##Sorting
###Comparative
####O(N^2) Algorithms
[选择、插入、气泡排序](http://openhome.cc/Gossip/AlgorithmGossip/SelectionInsertionBubble.htm)
1. Bubble Sort
   - 原理
     - 两套循环：
       内循环对比每一个元素及其右边元素，将相对较大的元素swap到右边。一个循环结束后，未排序元素中最大的元素就被顶到右边了。
       外循环则控制内循环的次数，也就是最多length - 1次。
       内循环检查的元素数量，到排过的顶层元素为止。所以每进行一次内循环，右侧需要检查的元素都向左推进一位。
       当一次内循环结束后，若没有发现逆序现象，说明已经排完了，左边的元素自动进入排好状态，即使外循环还有次数剩下，也可以提前结束。
     - 两个分类：
       將要排序的對象分作兩部份，一個是已排序的，一個是未排序的。排序時若是從小到大，最大元素會如同氣泡一樣移至右端，其利用比較相鄰元素的方式，將較大元素交換至右端，所以較大的元素會不斷往右移動，直到適當的位置為止。 

   - 时间复杂度：
     N^2 / 2 comparisons, N^2/4 swaps.
     **When is the case that we need to swap in every comparison on every pass?**
     我认为是全部逆序的时候才会出现全部swap。12345。

     	public void BubbleSort(int[] arr) {
     		// Boundry Check
     		if (arr == null || arr.length == 0)
     			return;
     		for (int i = 0; i < arr.length - 1; i++) {
     			boolean swapped = false; // Record if any two neighbors are in wrong order and needs swapping
     			for (j = 0; j < arr.length - 1 - i; j++) { // Scan the subarray that is not yet bubbled
     				if (arr[j] > arr[j + 1]) { // Scan very pair of neighbors, if they need swapping
     					swapped = true; // Note there is swap in this big round of i
     					int temp = arr[j + 1]; // Swap them
     					arr[j + 1] = arr[j];
     					arr[j] = temp;
     				}
     			}
     			if (!swapped) 
     				break;// If this round of scan shows absolutely correct order	
     		}
     	}
2. Selection Sort
   - 原理
     將要排序的對象分作兩部份，一個是已排序的，一個是未排序的。如果排序是由小而大，從後端未排序部份選擇一個最小值，並放入前端已排序部份的最後一個。
   - 时间复杂度
     也是O(N^2)但小于BubbleSort，因为swap只发生于outer loop。
     How about number of swaps? What is the time complexity?

     	// O(n^2) faster than bubble sort because swap only happens in the outer loop
     	public static void selectionSort(int[] data) {
     		int min; // set min variable for tmp min value
     		// move forward to right
     		for(int out=0; out < data.length-1; out++) {
     			min = out; // set initial min index to be out
     			// move forward to right from out+1 to the end
     			for(int in = out+1; in < data.length; in++) 
     				if(data[in] < data[min]) // if data is smaller than current min value
     					min = in; // set a new min index
     			
     			// swap min value with the first one as we move to the right
     			// swap is happening within the outer loop
     			swap(data, out, min); 
     		}
     	}

3. Insert Sort
   將要排序的對象分作兩部份，一個是已排序的，一個是未排序的。每次從後端未排序部份取得最前端的值，然後插入前端已排序部份的適當位置。
   很符合生活中排序的直觉。
   也是O(N^2)但是比前二者小，因为不用swap而是用shift。

   	 /*
   	 O(n^2) fastest among the three because there is less number of comparisons 
   	 and uses shifting instead of swapping
   	 */
   	public static void insertionSort(int[] data) {
   		// start from 1 till the last index
   		for(int out=1; out < data.length; out++) {
   			int tmp = data[out]; // save the first value as tmp
   			int in = out; // initial in variable index
   			// move backward till it finds the location to insert
   			while(in > 0 && data[in-1] >= tmp) {
   				// shift to right to make a room
   				data[in] = data[in-1];
   				in--;
   			}
   			// finally insert the tmp value to the right position
   			data[in] = tmp;
   		}
   	}

There is a time that this insertion sort can run even faster as much as in O(N) time. Can you think of it?
正序排列，12345，就是O(N)。

那么，反序54321会比BubbleSort快吗？

####O(N log N) Algorithms
###Non-Comparative

#Recursion

#Graphs

#Questions
~~1. Running time是实证意义上的还是理论意义上的？理论意义上是否要讨论底层实现？Basic operation (flop~~)?
~~2. 线性系数都是1，O(2N)也可表示为O(N)。为什么~~？If k <<< n, ignore; if k near n, n^2^.
~~3. BubbleSort复杂度小于O(N^2^)，但是N越大越趋近于N^2^，这个趋近是怎么证明出来的~~？等差数列求和
1. Array有线程安全这个维度吗？比如System.arraycopy之类。
   ~~5. ModCount~~?
   ArrayList里面的modCount是什么东西？干什么用的？[StackOverFlow Answer](http://stackoverflow.com/questions/1668901/java-modcount-arraylist)
   ~~6. Iterator和ListIterator有何区别~~: 
2. Vector的iterator到底是不是fail-fast的？
   ~~9. 是否需要implement iterator for BST（我觉得需要~~）
   HashMap应该介绍iterator，tree自己会就行了。
3. ~~Tree线程安全？不用考虑~~
   ~~11. MinStack没想清楚~~
   *12. BubbleSort: swap time*? 
   *When is the case that we need to swap in every comparison on every pass*?
   *13. SelectionSort: how about number of swaps? What is the time complexity*?
   *14. Swap和shift时间复杂度的区别？都在一个Loop里，基本操作有区别，算区别么？Big O是变化率的话，如何consisitent*？
   *15. InsertionSort做反序54321会比BubbleSort快吗*？
   *16. 实际应用中是各种排序算法用的多，还是随时保持顺序的各种树用得多？树对于应用中的排序有多大的意义？（比如，加一个元素进去可以随时保持顺序，复杂度小于O(N*)?
4. 遍历：假如要访问所有的数据结构，我们需要走一座迷宫，当然数组是顺序排列的，这个迷宫就是最简单的一个直线。“遍历”，就是我们在走迷宫的时候随时标记一个记号，放置的方式使得该记号能够标记我们已走过及接下来要走的路。我们走到哪里，就把记号标到哪里，直到我们走完迷宫，访问完所有的元素。

   你可能会说，这不就是把全部元素都访问一遍吗？为什么还要搞得那么复杂，还要引入“记号”这个概念？这是因为，“遍历”并不仅仅是访问完所有的元素，我们需要随时记录我们访问了哪些元素、没访问哪些元素，在每一个时间点，我们都可以知道我们走到哪了。最简便的方法，就是在半路留一个记号。像数组这样线性顺序的数据结构，记号的标记方式很简单，就是走到哪里，把下一个要访问的元素标记起来。【讲的不够清楚】

