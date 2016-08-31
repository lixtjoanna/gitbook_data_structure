# Search and Sort



## Searching

### Linear Search

清点一遍全部，找到返回，没找到返回sentinel。O(N)


```java
public static int linerSearch(int[] arr, int key){
    int size = arr.length;
    for(int i=0;i<size;i++){
        if(arr[i] == key){
            return i;
        }
    }
    return -1;
}
```


### Binary Search

#### Java方法

java.util.Arrays.binarySearch(int[] a, int key)

`public static int binarySearch(int[] a, int key)`

### 实现

依照顺序而一次去掉一半的搜索方式，只有有顺序的线性可定位结构能够使用（LinkedList不能用，因为无法直接access mid）。O(log N)。


```java
int[] data;
public int binarySearch(int key) {
     int low = 0;
     int high = data.length - 1;

     while(high >= low) {
       int middle = (low + high) / 2; // Mind: this does not cover overflow
       if(data[middle] == key) {
         return middle;
       }
       if(data[middle] < key) {
         low = middle + 1;
       }
       if(data[middle] > key) {
         high = middle - 1;
       }
     }
     return -1; // sentinel
}
```


#### BST Binary Search

BST也可以做Binary Search，一次去掉一半即可。

285_Inorder Successor in BST (TreeNode root, TreeNode p)

- successor是右子树的左极或右父亲。
- while loop搜索到该元素，维护“右父亲”节点。若没有右子树就返回右父亲。

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

270_Closest Binary Search Tree Value

找最近的元素而不是exact search，在切割的时候保留root并比较哪个更近即可。



## Sorting

### O(N^2) Algorithms

#### Bubble Sort

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

  ```java
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
  ```

#### Selection Sort

- 原理
  將要排序的對象分作兩部份，一個是已排序的，一個是未排序的。如果排序是由小而大，從後端未排序部份選擇一個最小值，並放入前端已排序部份的最後一個。

- 时间复杂度
  也是O(N^2)但小于BubbleSort，因为swap只发生于outer loop。
  How about number of swaps? What is the time complexity?

  ```java
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
  ```

#### Insert Sort

將要排序的對象分作兩部份，一個是已排序的，一個是未排序的。每次從後端未排序部份取得最前端的值，然後插入前端已排序部份的適當位置。
很符合生活中排序的直觉。
也是O(N^2)但是比前二者小，因为不用swap而是用shift。

```java
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
```

There is a time that this insertion sort can run even faster as much as in O(N) time. Can you think of it?
正序排列，12345，就是O(N)。

那么，反序54321会比BubbleSort快吗？



### O(N log N) Algorithms

#### MergeSort

1. 原理

   Merge Sort is an example of divide and conquer algorithm: 

   - Divide the given problem into simpler versions of itself. 

   - Conquer each problem using the same process. 

   - Finally, combine the results of simpler ones to have the final answer. 

     ​

   ![MergeSort](http://www.java2novice.com/images/merge_sort.png)

2. 步骤

   There are three steps involved in merge sort.

   - Sort the first half using merge sort. 
   - Sort the second half using merge sort.
   - Merge the sorted two halves to create the final sorted result. 

3. 实现

   在不同数据结构上Merge元素的各种方法经常被考。

   ```java
   public static int[] mergeSort(int[] data) {
   	if (data.length <= 1) // base case
   		return data;

   	// divide into two
   	// create left half
   	int[] left = new int[data.length / 2];
   	System.arraycopy(data, 0, left, 0, left.length);

   	// create right half
   	int[] right = new int[data.length - left.length];
   	System.arraycopy(data, left.length, right, 0, right.length);

   	// call itself to sort left half
   	left = mergeSort(left);
   	// call itself to sort right half
   	right = mergeSort(right);

   	// merge the sorted left and right and return
   	return merge(left, right);
   }

   // Precondition: two input arrays are sorted.
   private static int[] merge(int[] left, int[] right) {
   	// create a new array to hold merged elements
   	int[] merged = new int[left.length + right.length];

   	int index_left = 0, index_right = 0, index_m = 0;

   	// traverse and put proper values into the merged array from left or
   	// right array
   	while (index_left < left.length && index_right < right.length) {
   		if (left[index_left] < right[index_right])
   			merged[index_m] = left[index_left++];
   		else
   			merged[index_m] = right[index_right++];

   		index_m++;
   	}

   	// need to handle the case where elements are not all copied over to
   	// the merged array
   	if (index_left < left.length) { // elements are still in left array
   		for (int i = index_left; i < left.length; i++)
   			merged[index_m++] = left[i];
   	} else { // elements are still in right array
   		for (int i = index_right; i < right.length; i++)
   			merged[index_m++] = right[i];
   	}

   	return merged;
   }
   ```
   [Merge Sorted Array](https://leetcode.com/problems/merge-sorted-array/)

   [Merge 2 Sorted Lists](https://leetcode.com/problems/merge-two-sorted-lists/)

   [Merge K Sorted Lists](https://leetcode.com/problems/merge-k-sorted-lists/)



#### QuickSort

[Visualize Quick Sort](http://me.dt.in.th/page/Quicksort/)

1. 原理

   找一个pivot分开两边，然后递归地进行这个过程直到所有元素均被选为pivot一次并得到处理，就排好顺序了。

2. 步骤

   - Pick a “pivot” element.
   - “Partition” the array into 3 parts:
     - First part: all elements in this part is less than the pivot.
     - Second part: the pivot itself (only one element!)
     - Third part: all elements in this part is greater than or equal to the pivot.
   - Then, apply the quicksort algorithm to the first and the third part. (recursively)

3. 实现

   - Parititioning

     ```java
     private static int partition(int[] data, int left, int right, int pivot) {
     	int leftPointer = left - 1;
     	int rightPointer = right;

     	while (true) {
     		// scanning to find out-of-place values
     		while (data[++leftPointer] < pivot);
     		while (rightPointer > 0 && data[--rightPointer] > pivot);

     		if (leftPointer >= rightPointer) // nothing to swap
     			break;
     		else { // swap out-of-place values
     			swap(data, leftPointer, rightPointer);
     		}
     	}
     	// put pivot value into the right location
     	swap(data, leftPointer, right);
     	return leftPointer; // return where the pivot value is (index)
     }

     private static void swap(int[] data, int one, int two) {
     	int tmp = data[one];
     	data[one] = data[two];
     	data[two] = tmp;
     }
     ```

   - Wrap Up

     ```java
     public static void quickSort(int[] data) {
     	recQuickSort(data, 0, data.length - 1);
     }

     private static void recQuickSort(int[] data, int left, int right) {
     	if (left >= right) // base case
     		return;
     	else {
     		int pivot = data[right];
     		// partition first and get the location of the pivot
     		int partition = partition(data, left, right, pivot);
     		// then perform recursive calls
     		recQuickSort(data, left, partition - 1);
     		recQuickSort(data, partition + 1, right);
     	}
     }
     ```

4. 注意事项

   You cannot simply say that the running time complexity of quick sort is  O(N*logN). In fact, its worst case running time complexity is O(N^2). It  is important for you to know its possibility of degeneration and  strategies to mitigate it. 

#### Heap Sort

