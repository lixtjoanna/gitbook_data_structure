# Map

## 四种基本实现

http://www.programcreek.com/2013/03/hashmap-vs-treemap-vs-hashtable-vs-linkedhashmap/

![](http://www.programcreek.com/wp-content/uploads/2009/02/MapClassHierarchy-600x354.jpg)

- HashMap is implemented as a hash table, and there is no ordering on keys or values.
- TreeMap is implemented based on red-black tree structure, and it is ordered by the key.
- LinkedHashMap preserves the insertion order
- Hashtable is synchronized, in contrast to HashMap. It has an overhead for synchronization.

This is the reason that HashMap should be used if the program is thread-safe.



## HashMap



### 问题与原理

Google开Career Fair，需要记录每一个visitor的姓名和简历，迅速查找。

怎么找？来一个存一个，array，添加、删除、查找都是O(n)。

有没有更快的方法？上次讲到的Hash，可以解决“一一对应”问题。将姓名作为key，简历作为value来保存（想象成一个姓名对应成一个小盒子，简历放在盒子里）。

编哈希码——比如，笔画数。一个姓名，数完查找，没有则添加，有则更新。

1. 不可重复：如果姓名一样怎么办呢？很遗憾，这里就没办法了，但如果用邮箱地址就不会一样了。
2. 冲突解决：如果哈希码一样？冲突解决，可以用linear probing或separate chaining。
3. Rehash: 如果冲突太多？Rehash，增加小盒子，哈希码变成笔画数+笔画数%盒子数，更加均匀分散。
   - 如何测量“冲突太多”？ Load Factor = numElement / sizeOfArray
4. 没有顺序： 注意，笔画数或姓名拼音等编码看起来是有顺序的，但我们也可以用其他方式编码，那时顺序就不好预测了。Hash法自身是无法给元素排序的。

这种用哈希码存储key-value pair的数据结构，叫HashMap。



### 功能、方法、复杂度

[Frequently Used Methods of HashMap](http://www.programcreek.com/2013/04/frequently-used-methods-of-java-hashmap/)

1. 功能：添加、删除、搜索（提取）全部O(1)
   ```java
    V put(K key, V value)
    V get(Object key)
    V remove(Object key)
    Boolean containsKey(Object key)
   ```

2. 不能做到：定位，排序

3. Traverse方法：没有顺序怎么遍历？

   [Traverse Map 4 Methods](http://blog.csdn.net/tjcyjd/article/details/11111401)

   - Iterator
   - entrySet/keySet/valueSet
   - Java中不要通过key找value来遍历（不完全是O(1))



### 实现

[Java HashMap Source](http://developer.classpath.org/doc/java/util/HashMap-source.html)

-  数据存放：Entry<K, V>是AbstractMap中实现的，存放key, value, hash和链接到同一个hash bucket的下一个Entry的指针next。HashMap中的HashEntry是static class，继承该类，并增加几种新的方法。
   ```java
   static class Entry<K,V> implements Map.Entry<K,V> {
      	final K key;
      	V value;
      	Entry<K,V> next;
      	final int hash;
      		//...
   }
   ```

   - put<K,V>
     将定义好的key和value放进HashMap中。先计算出hash code，而后在对应链表中检查是否已存在该key。如存在，更新对应value；如不存在，将新的entry加入该链表。

     ```java
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

       // At this point, we know we need to add a new entry
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
     ```

   loadFactor思想。Java7源码。

   - get\<K>
     得到hash->扫描链表->key相等时取出value->没有key返回null

     ```java
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
     ```

   - containsKey\<K>
     一样的扫描链表方式，扫到key以后返回true，没扫到返回false。

     ```java
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
     ```

   - remove<K>

     扫描，找到元素并跳过它重新接续链表（若元素为第一位则需要assign bucket[idx]）。

     ```java
     public V remove(Object key) {
        int idx = hash(key);
        HashEntry<K, V> e = buckets[idx];
        HashEntry<K, V> last = null;

        while (e != null)
        {
          if (equals(key, e.key))
          {
            modCount++;
            if (last == null)
              buckets[idx] = e.next;
            else
              last.next = e.next;
            size--;
      // Method call necessary for LinkedHashMap to work correctly.
            return e.cleanup();
          }
          last = e;
          e = e.next;
        }
        return null;
     }
     ```

   - resize\<int newCapacity>
     当加入新entry以后新size > threshold时call此函数，扩展array长度至newCapacity，java用的是2*oldCapacity（WHY?），并将旧的entries挪移到新的array中来。
     时间复杂度O(N)（挪移时扫描全table）
     难点：挪移。
     注意：较新版本的Java用的index经过了优化处理。

     ```java
     // the "rehash" function in JAVA 8 that directly takes the key
     static final int hash(Object key) {
         int h;
         return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
         }
     // the function that returns the index from the rehashed hash
     static int indexFor(int h, int length) {
         return h & (length-1);
     }
     ```

Rehash Function in Java 6 (Java 8 is too long…)

```java
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
```

### 注意事项

1. No order.
2. No duplicate.
3. Key可以是null。



## HashTable

The HashMap class is roughly equivalent to Hashtable, except that it is unsynchronized and permits nulls.



## LinkedHashMap

怎么把Visitors按照来的顺序记录？保持一个Linked structure，每来一个就连到末尾。

表现LinkedHashMap性质的代码：

```java
public static void main(String[] args) {
		Dog d1 = new Dog("red");
		Dog d2 = new Dog("black");
		Dog d3 = new Dog("white");
		Dog d4 = new Dog("white");
 
		LinkedHashMap<Dog, Integer> linkedHashMap = new LinkedHashMap<Dog, Integer>();
		linkedHashMap.put(d1, 10);
		linkedHashMap.put(d2, 15);
		linkedHashMap.put(d3, 5);
		linkedHashMap.put(d4, 20);
 
		for (Entry<Dog, Integer> entry : linkedHashMap.entrySet()) {
			System.out.println(entry.getKey() + " - " + entry.getValue());
		}		
}
```

Output一定是：

```
red dog - 10
black dog - 15
white dog - 20
```

如果用HashMap, output不一定能保持这个顺序。



## TreeMap

### 功能

那如果想对visitor按照年龄等因素进行排序呢？LinkedHashMap就不能满足了，它只能做到按put的顺序输出。

TreeMap：仍然不可以针对value进行排序，但可以针对key进行排序。重写hashCode function就可以实现某种想要的顺序。这说明：用TreeMap的话，key必须是comparable的！可以用override compareTo function来实现。

当我们需要对HashMap中的元素进行排序时，可以将其转换入TreeMap来排。

这段代码把Dog进行了按size的排序：

```java
class Dog implements Comparable<Dog>{
	String color;
	int size;
 
	Dog(String c, int s) {
		color = c;
		size = s;
	}
 
	public String toString(){	
		return color + " dog";
	}
 
	@Override
	public int compareTo(Dog o) {
		return  o.size - this.size;
	}
}
 
public class TestTreeMap {
	public static void main(String[] args) {
		Dog d1 = new Dog("red", 30);
		Dog d2 = new Dog("black", 20);
		Dog d3 = new Dog("white", 10);
		Dog d4 = new Dog("white", 10);
 
		TreeMap<Dog, Integer> treeMap = new TreeMap<Dog, Integer>();
		treeMap.put(d1, 10);
		treeMap.put(d2, 15);
		treeMap.put(d3, 5);
		treeMap.put(d4, 20);
 
		for (Entry<Dog, Integer> entry : treeMap.entrySet()) {
			System.out.println(entry.getKey() + " - " + entry.getValue());
		}
	}
}
```



### 方法

http://www.tutorialspoint.com/java/java_treemap_class.htm

关键：可以提取头部、尾部的key和map。



###实现及复杂度

为了能够维护顺序，用红黑树来实现（自平衡的BST）。

没有用Hash所以添删搜复杂度不再保持O(1)，而是Tree的O(log N)



### 注意事项

- 因为基于HashMap，所以entry如果是新class要override equals()和hashCode()，
- 同时，entry如果是新class也必须implement Comparable interface，override compareTo()。
- Key不能有duplicate，如果有的话就会替换，这点和HashMap一样。