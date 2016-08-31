#Set

[HashSet-TreeSet-LinkedHashSet](http://www.programcreek.com/2013/03/hashset-vs-treeset-vs-linkedhashset/)

![](https://bittigerimages.s3.amazonaws.com/gitbookImages/DataStructures/set1.jpeg)



##HashSet
###问题与原理
Google办career fair，来的人一人一件T-shirt，如何决定是否要发给某个人？需要检查他是否已经领取过了。每个人领取的时候，写入名单，来一个新人的时候，问她的名字，在名单中查询。如何快速定位呢？可以按照姓名字母来定位。
这一功能，也就是“查重”，HashSet可以完成。
####什么是Set？
A Set is an abstract data type that can store certain values, without any particular order, and no repeated values.
1. No order
2. No repeat
####为何用Hashing来查重？
快速add/remove/contains(search): O(1)。
###功能/方法/复杂度

| Methods            | Complexity |
| ------------------ | ---------- |
| add(Object o)      | O(1)       |
| remove(Object o)   | O(1)       |
| contains(Object o) | O(1)       |

1. 没有顺序，所以无法排序
2. 不能重复
3. 无法定位
4. 用iterator()来遍历

###实现
1. 底层结构：用一个HashMap来实现，但value部分用dummy object，只保留key。
2. It stores and retrieves elements by using a hash function that converts  elements into an integer.
3. 如果重复，和HashMap一样会替换，但因为value都是dummy，所以不会发生变化。


```java
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
```
###应用及注意事项

1. No order.

2. No duplicate.

   ​

##TreeSet
###问题与原理

但是，如果我希望这个set有顺序，能够被排序，怎么办呢？比如说，我想在晚会结束以后把所有的人名按照字母顺序打印。

Hashing可以用LinkedHashSet来实现部分的顺序。输出和输入的顺序是一样的，基本操作都是O(1)。

但是，要排序，linked结构不方便。于是，我们想到BST是有顺序的！或许可以用BST？

尝试发现：balance时都很好，但如果不再balance，效率就降低了。

用树结构可以排序，但是需要解决平衡问题。于是我们可以用：Red-black tree：自平衡的BST。

[Red-Black Tree](http://www.geeksforgeeks.org/red-black-tree-set-1-introduction-2/)

###功能/方法/复杂度

因为是平衡树，所以各个主要操作都是O(logn)。

TreeSet is implemented using a tree structure(red-black tree in algorithm book). The elements in a set are sorted, but the add, remove, and contains methods has time complexity of O(log (n)). It offers several methods to deal with the ordered set like first(), last(), headSet(), tailSet(), etc.

###实现
###应用及注意事项

1. It is generally faster to add elements to a HashSet. And, if you need to traverse the elements in sorted order, convert the collection to a TreeSet.
2. TreeSet有顺序，所以内含的类必须要implement Comparable\<AnyType>。

[HashSet vs. TreeSet vs. LinkedHashSet](http://www.programcreek.com/2013/03/hashset-vs-treeset-vs-linkedhashset/)
There are 3 commonly used implementations of Set: HashSet, TreeSet and LinkedHashSet. When and which to use is an important question. In brief, if you need a fast set, you should use HashSet; if you need a sorted set, then TreeSet should be used; if you need a set that can be store the insertion order, LinkedHashSet should be used.

- HashSet is Implemented using a hash table. Elements are not ordered. The add, remove, and contains methods have constant time complexity O(1).
- TreeSet is implemented using a tree structure(red-black tree in algorithm book). The elements in a set are sorted, but the add, remove, and contains methods has time complexity of O(log (n)). It offers several methods to deal with the ordered set like first(), last(), headSet(), tailSet(), etc.
- LinkedHashSet is between HashSet and TreeSet. It is implemented as a hash table with a linked list running through it, so it provides the order of insertion. The time complexity of basic methods is O(1).
