
#Queue
##问题和原理
上次我们介绍了如何处理LIFO，也就是Last in First Out的问题。
还有一种非常类似的情形，就是我们熟悉的排队。我们进入一个队列，等待排到我们，每次排到的都是队列里最早来的人，也就是FIFO，First in First Out。
![](https://bittigerimages.s3.amazonaws.com/gitbookImages/DataStructures/queue1.png)
这种功能，应该如何实现呢？你也许会猜到，类似于Stack，用Array、ArrayList、LinkedList也一样都能实现，但也都比较麻烦。同样类似于Stack，我们也可以设计一种直接实现FIFO功能的数据结构，也就是队列Queue。

##功能、方法及复杂度
1. Queue实现以下功能：

   - enqueue(x): add an item at the tail
   - dequeue: remove the item at the head
   - peek: return the item at the head (without removing it)
   - size: return the number of items in the queue
   - isEmpty: return whether the queue has no items

   | DS    | Enqueue | Dequeue | Peek | size | isEmpty |
   | :---- | :------ | :-----: | ---: | ---: | ------: |
   | Queue | O(1)    |  O(1)   | O(1) | O(1) |    O(1) |

2. 对应的Java方法：Java里没有Queue class，而是用LinkedList来implement Queue interface。方法主要是poll()和offer()。

   http://tutorialspointexamples.com/queue-interface-in-java/

   - add(Object obj): Inserts the specified element into this queue. Returns true if queue changed after the operation otherwise returns false.

   - offer(Object obj): Inserts the specified element into this queue. Returns true if the element was added to this queue otherwise returns false.

   - remove(): Returns and removes the head of this queue. It throws an exception if this queue is empty.

   - poll(): Returns and removes the head of this queue. Returns null if this queue is empty.

   - element(): Returns but does not removes the head of this queue. It throws an exception if this queue is empty.

   - peek(): Returns but does not removes the head of this queue. Returns null if this queue is empty.

     ​

##实现
1. LinkedList
   用LinkedList实现Queue，和实现Stack类似，但更复杂一些，因为Stack是在同一头操作，而LinkedList不是。
   如果我们把排队操作放在list尾部，排到了就从头部删除，那么要enqueue就得先traverse到尾部，时间O(N)，没效率。
   如何改进呢？保持一个到尾部的Node：rear即可。每次enqueue，用rear.next把newNode连上，然后rear = newNode。dequeue直接弹head的data，然后head = head.next。

2. Array

   用Array实现Queue，就复杂一些了。我们用一个数组，维护头尾两个位置。添加在尾端，提取在头端，循环流动。而头端和尾端相碰的时候，就需要resize，从头到尾复制到新的内存空间中，将原来的空间舍弃。

   Things to note here:

   ```
   - The capacity is set at creation time
   - Adds have a precondition that the queue isn't already full
   - Adding to a full queue is a state exception, not an argument exception
   - The array itself is effectively circular
   - When removing, we take care to nullify the newly unused slot in the array to prevent a possible memory leak
   ```

   这里最值得注意的就是循环的方式。【index = tail % arr. length】？

3. Stack

   LeetCode: Implement Queue Using Stacks

   用两个stack，push的时候进入第一个，pop的时候则把所有第一个里面的元素pop到第二个，顺序就反过来了，然后pop掉第二个stack头部元素即可。

   http://www.jiuzhang.com/solutions/implement-queue-by-two-stacks/

   ```java
   public class Queue {
       private Stack<Integer> stack1;
       private Stack<Integer> stack2;

       public Queue() {
          stack1 = new Stack<Integer>();
          stack2 = new Stack<Integer>();
       }

       private void stack2ToStack1() {
   		while (!stack2.empty()) {
   			stack1.push(stack2.pop());
   		}
   	}

       public void push(int number) {
           stack2.push(number);
       }

       public int pop() {
           if (stack1.empty() == true) {
   			this.stack2ToStack1();
   		}
           return stack1.pop();
       }

       public int top() {
           if (stack1.empty() == true) {
   			this.stack2ToStack1();
   		}
           return stack1.peek();
       }
   }
   ```

   ​


##注意事项
1. Queue不是线程安全的。Java中线程安全的Queue是Blocking Queue。
   [Blocking Queue](http://www.infoq.com/cn/articles/java-blocking-queue/)
   阻塞队列（BlockingQueue）是一个支持两个附加操作的队列。这两个附加的操作是：在队列为空时，获取元素的线程会等待队列变为非空。当队列满时，存储元素的线程会等待队列可用。阻塞队列常用于生产者和消费者的场景，生产者是往队列里添加元素的线程，消费者是从队列里拿元素的线程。阻塞队列就是生产者存放元素的容器，而消费者也只从容器里拿元素。这两个方法分别是put()和take()。
2. ArrayDeque
   Queue是Java里的一个Interface，除此之外，还有一个Deque Interface，与之类似。Deque简而言之就是Stack和Queue的结合，我们可以通过它在线性结构的头尾两端自由地存取元素。
   [Deque Interface](https://docs.oracle.com/javase/tutorial/collections/interfaces/deque.html)

   而ArrayDeque使用动态数组实现Dequeue Interface的一个类。它有以下几个特点：
   - Array deques have no capacity restrictions so they grow as necessary to support usage.
   - They are not thread-safe; in the absence of external synchronization.
   - They do not support concurrent access by multiple threads.
   - Null elements are prohibited in the array deques.
   - They are faster than Stack and LinkedList.

   普通数组只能快速在末尾添加元素，为了支持FIFO，从数组头快速取出元素，就需要使用循环数组：有队头队尾两个下标：弹出元素时，队头下标递增；加入元素时，如果已到数组空间的末尾，则将元素循环赋值到数组0，同时队尾下标指向0，再插入下一个元素则赋值到数组[1]，队尾下标指向1。如果队尾的下标追上队头，说明数组所有空间已用完，进行双倍的数组扩容。



##应用

1. BFS里会用到queue，而它具备逐层记录性质（参见102Binary Tree Level Order Traversal）。
