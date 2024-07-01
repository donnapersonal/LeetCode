# 数组

### 数组定义

`数组（Array）`：是一种线性表数据结构，使用`连续的内存空间`来存储一组具有相同类型的数据

> `线性表`：是所有数据元素排成像一条线一样的结构，线性表上的数据元素都是相同类型，且每个数据元素最多只有前、后两个方向。数组就是一种线性表结构，此外，栈、队列、链表都是线性表结构
> 
> 线性表有两种存储结构：`顺序存储结构`和`链式存储结构`
> - `顺序存储结构`是指占用的内存空间是连续的，相邻数据元素之间，物理内存上的存储位置也相邻
> 数组就是采用了顺序存储结构，且存储的数据都是相同类型

简单地说，**数组**是实现线性表的顺序结构存储的基础

数组可以方便的通过`下标索引`的方式获取到`下标下对应的数据`

> 注意：  
> - 数组下标是从 `0` 开始的  
> - 数组内存空间的地址是`连续`的
> 
> 因为数组的在内存空间的地址是连续的，因此在删除或增添元素时就需要`移动`其他元素的地址

> 若使用 `C++`，要注意 `vector` 和 `array` 的区别，`vector` 的底层实现是 `array`，严格来讲 `vector` 是容器，不是数组

> 数组的元素是不能删的，只能`覆盖`

### 随机访问数组元素

数组的一个最大特点：**可以进行随机访问**，即数组可以根据下标直接定位到某个元素存放的位置

计算机是如何实现根据下标随机访问数组元素的？
- 计算机给一个数组分配了一组连续的存储空间，其中第一个元素开始的地址被称为`首地址`，每个数据元素都有对应的下标索引和内存地址，计算机通过地址来访问数据元素
- 当计算机需要访问数组的某个元素时，会通过`寻址公式`计算出对应元素的内存地址，然后访问地址对应的数据元素
> 寻址公式：`下标 i 对应的数据元素地址 = 数据首地址 + 单个数据元素所占内存大小 * i`

### 二维数组

![image.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/b9965bfeff90495398c0b0a9515500f3~tplv-k3u1fbpfcp-jj-mark:0:0:0:0:q75.image#?w=2000&h=570&s=128933&e=png&b=fefefe)

**二维数组的特点**：从形状上看，相对于一维数组一条“线”一般的布局，二维数组更像是一个“面”

> 在数学中，形如这样**长方阵列排列的复数或实数集合**，被称为`“矩阵”`，因此**二维数组的别名就叫“矩阵”** 
> 
> 因此可以将二维数组看做是一个矩阵，并处理矩阵的相关问题，如转置矩阵、矩阵相加等

**二维数组在内存空间的地址是连续的吗？** 
- 不同编程语言的内存管理不一样，在 C++ 中二维数组是连续分布的
  
  ```js
  void test_arr() { 
    int array[2][3] = { {0, 1, 2}, {3, 4, 5} }; 

    cout << &array[0][0] << " " << &array[0][1] << " " << &array[0][2] << endl; 
    cout << &array[1][0] << " " << &array[1][1] << " " << &array[1][2] << endl; 
  } 

  int main() { 
    test_arr(); 
  } 

  // 测试地址为: 
  // 0x7ffee4065820 0x7ffee4065824 0x7ffee4065828 
  // 0x7ffee406582c 0x7ffee4065830 0x7ffee4065834
  ```
  地址为 `16` 进制，可以看出上面生成的二维数组地址是连续一条线的

  ![image.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/dfb1daf2bcda47ad86f4dfd1eaf0fc35~tplv-k3u1fbpfcp-jj-mark:0:0:0:0:q75.image#?w=2000&h=393&s=115486&e=png&b=fcfcfc)

  因此在 `C++` 中二维数组在地址空间上是`连续`的

- 而 `Java` 是`没有指针`的，同时**也不对程序员暴露其元素的地址，寻址操作完全交给虚拟机**，所以看不到每个元素的地址情况
  ```js
  public static void test_arr() { 
    int[][] arr = {{1, 2, 3}, {3, 4, 5}, {6, 7, 8}, {9,9,9}}; 
    System.out.println(arr[0]); 
    System.out.println(arr[1]); 
    System.out.println(arr[2]); 
    System.out.println(arr[3]); 
  } 

  // 输出的地址为： 
  // [I@7852e922 
  // [I@4e25154f 
  // [I@70dea4e
  // [I@5c647e05
  ```

  这里的数值也是 `16` 进制，但**不是真正的地址**，而是经过处理过后的数值，可以看出二维数组的每一行头结点的地址是没有规则的，就`不存在连续`

  所以 `Java` 的二维数组可能是如下排列的方式：

  ![image.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/d97a5d1fbd264eb287bf6f079bd83a5c~tplv-k3u1fbpfcp-jj-mark:0:0:0:0:q75.image#?w=2000&h=964&s=101907&e=png&b=fefefe)

### 数组的基本操作

#### 1. 访问元素

访问数组中第 i 个元素：
- 只需检查 i 的范围是否在合法的范围区间，超出范围的访问为非法访问
- 当位置合法时，根据给定下标得到元素的值

访问数组元素的操作不依赖于数组中元素个数，因此，该操作的时间复杂度为 `O(1)`

#### 2. 查找元素

查找数组中元素值为 val 的位置：
- 建立一个基于下标的循环，每次将 val 与当前数据元素 nums[i] 进行比较
- 找到元素时返回元素下标
- 遍历完，若找不到可以返回一个特殊值（如 -1）
  
在查找元素的操作中：
- 若数组无序，则只能通过将 val 与数组中的数据元素逐一对比进行查找（称为线性查找）
- 而线性查找操作依赖于数组中元素个数，因此，查找元素的时间复杂度为 `O(n)`

#### 3. 插入元素

插入元素操作分为两种：
- 在数组尾部插入元素
- 在数组第 i 个位置上插入元素
  
在数组尾部插入元素：
- 若数组尾部容量充足，则直接把元素放在数组尾部的空闲位置，并更新数组的元素计数值
- 若数组容量满了，则插入失败
  > Python 中的 list 列表做了处理，当数组容量满后会开辟新的空间进行插入
- 在数组尾部插入元素的操作不依赖数组个数，不会造成其他元素移动，因此其时间复杂度为 `O(1)`

在数组第 i 个位置上插入元素：
- 先检查插入下标 i 是否在合法范围内
- 确定合法位置后，通常情况下第 i 个位置上已有数据，要把第 `i ~ len - 1` 位置上的元素依次向后移动
- 然后再在第 i 个元素位置插入元素，并更新数组的元素计数值

在数组中间位置插入元素的操作中，由于移动元素的操作次数跟元素个数有关，因此，其最坏和平均时间复杂度都是 O(n)

#### 4. 改变元素

将数组中第 i 个元素值改为新值：
- 先检查 i 的范围是否在合法的范围区间
- 然后将第 i 个元素值赋为新值

改变元素的操作跟访问元素操作类似，因此，其时间复杂度为 O(1)

#### 5. 删除元素

删除元素分为三种情况：
- 删除数组尾部元素
- 删除数组第 i 个位置上的元素
- 基于条件删除元素
  
删除数组尾部元素：
- 只需将元素计数值减一
- 该操作不依赖于数组中的元素个数，因此，其时间复杂度为 `O(1)`

删除数组第 i 个位置上的元素：
- 检查下标 i 是否在合法范围内
- 若下标合法，则将第 `i + 1` 个位置到第 `len - 1` 位置上的元素依次向左移动
- 删除后修改数组的元素计数值
- 删除数组第 i 个位置的元素的操作同样涉及移动元素，而移动元素的操作次数跟元素个数有关，因此其最坏和平均时间复杂度都是 `O(n)`

基于条件删除元素，一般是给出一个条件要求删除满足这个条件的（一个、多个或所有）元素：
- 也是通过遍历检查元素，查找到满足条件的元素后将其删除
- 基于条件删除元素的操作同样涉及移动元素，因此，其最坏和平均时间复杂度都是 `O(n)`
  
### 数组的基础知识总结

数组是最基础的数据结构
- 数组是实现线性表的顺序结构存储的基础
- 它使用一组连续的内存空间，来存储一组具有相同类型的数据

**数组的最大特点是：`支持随机访问`**

访问数组元素、改变数组元素的时间复杂度为 `O(1)`，在数组尾部插入、删除元素的时间复杂度也是 `O(1)`

一般情况下插入、删除元素的时间复杂度均为 `O(n)`

### 动态数组

关键点：
- 自动扩缩容
  - 在实际使用动态数组时，缩容也是重要的优化手段。比如一个动态数组开辟了能够存储 1000 个元素的连续内存空间，但实际只存了 10 个元素，就有 990 个空间是空闲的。为了避免资源浪费，其实可以适当缩小存储空间，这就是缩容
  - 这里就实现一个简单的扩缩容的策略：
    - 当数组元素个数达到底层静态数组的容量上限时，扩容为原来的 2 倍
    - 当数组元素个数缩减到底层静态数组的容量的 1/4 时，缩容为原来的 1/2
- 索引越界的检查
- 删除元素谨防内存泄漏

```java
import java.util.Arrays;
import java.util.Iterator;
import java.util.NoSuchElementException;

public class MyArrayList<E> {
    // 真正存储数据的底层数组
    private E[] data;
    // 记录当前元素个数
    private int size;
    // 默认初始容量
    private static final int INIT_CAP = 1;

    public MyArrayList() {
        this(INIT_CAP);
    }

    public MyArrayList(int initCapacity) {
        data = (E[]) new Object[initCapacity];
        size = 0;
    }

    // 增
    public void addLast(E e) {
        int cap = data.length;
        // 看 data 数组容量够不够
        if (size == cap) {
            resize(2 * cap);
        }
        // 在尾部插入元素
        data[size] = e;
        size++;
    }

    public void add(int index, E e) {
        // 检查索引越界
        checkPositionIndex(index);

        int cap = data.length;
        // 看 data 数组容量够不够
        if (size == cap) {
            resize(2 * cap);
        }

        // 搬移数据 data[index..] -> data[index+1..]
        // 给新元素腾出位置
        for (int i = size - 1; i >= index; i--) {
            data[i + 1] = data[i];
        }

        // 插入新元素
        data[index] = e;

        size++;
    }

    public void addFirst(E e) {
        add(0, e);
    }

    // 删
    public E removeLast() {
        if (size == 0) {
            throw new NoSuchElementException();
        }
        int cap = data.length;
        // 可以缩容，节约空间
        if (size == cap / 4) {
            resize(cap / 2);
        }

        E deletedVal = data[size - 1];
        // 删除最后一个元素
        // 必须给最后一个元素置为 null，否则会内存泄漏
        data[size - 1] = null;
        size--;

        return deletedVal;
    }

    public E remove(int index) {
        // 检查索引越界
        checkElementIndex(index);

        int cap = data.length;
        // 可以缩容，节约空间
        if (size == cap / 4) {
            resize(cap / 2);
        }

        E deletedVal = data[index];

        // 搬移数据 data[index+1..] -> data[index..]
        for (int i = index + 1; i < size; i++) {
            data[i - 1] = data[i];
        }

        data[size - 1] = null;
        size--;

        return deletedVal;
    }

    public E removeFirst() {
        return remove(0);
    }

    // 查
    public E get(int index) {
        // 检查索引越界
        checkElementIndex(index);

        return data[index];
    }

    // 改
    public E set(int index, E element) {
        // 检查索引越界
        checkElementIndex(index);
        // 修改数据
        E oldVal = data[index];
        data[index] = element;
        return oldVal;
    }

    // 工具方法
    public int size() {
        return size;
    }

    public boolean isEmpty() {
        return size == 0;
    }

    // 将 data 的容量改为 newCap
    private void resize(int newCap) {
        E[] temp = (E[]) new Object[newCap];

        for (int i = 0; i < size; i++) {
            temp[i] = data[i];
        }

        data = temp;
    }

    private boolean isElementIndex(int index) {
        return index >= 0 && index < size;
    }

    private boolean isPositionIndex(int index) {
        return index >= 0 && index <= size;
    }

    /**
     * 检查 index 索引位置是否可以存在元素
     */
    private void checkElementIndex(int index) {
        if (!isElementIndex(index))
            throw new IndexOutOfBoundsException("Index: " + index + ", Size: " + size);
    }

    /**
     * 检查 index 索引位置是否可以添加元素
     */
    private void checkPositionIndex(int index) {
        if (!isPositionIndex(index))
            throw new IndexOutOfBoundsException("Index: " + index + ", Size: " + size);
    }

    private void display() {
        System.out.println("size = " + size + " cap = " + data.length);
        System.out.println(Arrays.toString(data));
    }

    public static void main(String[] args) {
        // 初始容量设置为 3
        MyArrayList<Integer> arr = new MyArrayList<>(3);

        // 添加 5 个元素
        for (int i = 1; i <= 5; i++) {
            arr.addLast(i);
        }

        arr.remove(3);
        arr.add(1, 9);
        arr.addFirst(100);
        int val = arr.removeLast();

        for (int i = 0; i < arr.size(); i++) {
            System.out.println(arr.get(i));
        }
    }
}
```

### 环形数组

> 环形数组技巧可以让我们用 `O(1)` 的时间在数组头部增删元素

环形数组的关键：它维护了两个指针 `start` 和 `end`，`start` 指向第一个有效元素的索引，`end` 指向最后一个有效元素的下一个位置索引

当在数组头部添加或删除元素时只需移动 `start` 索引，而在数组尾部添加或删除元素时只需移动 `end` 索引

当 `start/end` 移动超出数组边界（`< 0` 或 `>= arr.length`）时，可通过求模运算 `%` 让它们转一圈到数组头部或尾部继续工作，这样就实现了环形数组的效果

```java
public class CycleArray<T> {
    private T[] arr;
    private int start;
    private int end;
    private int count;
    private int size;

    public CycleArray() {
        this(1);
    }

    @SuppressWarnings("unchecked")
    public CycleArray(int size) {
        this.size = size;
        // Java 不支持直接创建泛型数组，所以这里使用了类型转换
        this.arr = (T[]) new Object[size];
        // start 指向第一个有效元素的索引，闭区间
        this.start = 0;
        // 切记 end 是一个开区间，
        // 即 end 指向最后一个有效元素的下一个位置索引
        this.end = 0;
        this.count = 0;
    }

    // 自动扩缩容辅助函数
    @SuppressWarnings("unchecked")
    private void resize(int newSize) {
        // 创建新的数组
        T[] newArr = (T[]) new Object[newSize];
        // 将旧数组的元素复制到新数组中
        for (int i = 0; i < count; i++) {
            newArr[i] = arr[(start + i) % size];
        }
        arr = newArr;
        // 重置 start 和 end 指针
        start = 0;
        end = count;
        size = newSize;
    }

    // 在数组头部添加元素，时间复杂度 O(1)
    public void addFirst(T val) {
        // 当数组满时，扩容为原来的两倍
        if (isFull()) {
            resize(size * 2);
        }
        // 因为 start 是闭区间，所以先左移，再赋值
        start = (start - 1 + size) % size;
        arr[start] = val;
        count++;
    }

    // 删除数组头部元素，时间复杂度 O(1)
    public void removeFirst() {
        if (isEmpty()) {
            throw new IllegalStateException("Array is empty");
        }
        // 因为 start 是闭区间，所以先赋值，再右移
        arr[start] = null;
        start = (start + 1) % size;
        count--;
        // 如果数组元素数量减少到原大小的四分之一，则减小数组大小为一半
        if (count > 0 && count == size / 4) {
            resize(size / 2);
        }
    }

    // 在数组尾部添加元素，时间复杂度 O(1)
    public void addLast(T val) {
        if (isFull()) {
            resize(size * 2);
        }
        // 因为 end 是开区间，所以是先赋值，再右移
        arr[end] = val;
        end = (end + 1) % size;
        count++;
    }

    // 删除数组尾部元素，时间复杂度 O(1)
    public void removeLast() {
        if (isEmpty()) {
            throw new IllegalStateException("Array is empty");
        }
        // 因为 end 是开区间，所以先左移，再赋值
        end = (end - 1 + size) % size;
        arr[end] = null;
        count--;
        // 缩容
        if (count > 0 && count == size / 4) {
            resize(size / 2);
        }
    }

    // 获取数组头部元素，时间复杂度 O(1)
    public T getFirst() {
        if (isEmpty()) {
            throw new IllegalStateException("Array is empty");
        }
        return arr[start];
    }

    // 获取数组尾部元素，时间复杂度 O(1)
    public T getLast() {
        if (isEmpty()) {
            throw new IllegalStateException("Array is empty");
        }
        // end 是开区间，指向的是下一个元素的位置，所以要减 1
        return arr[(end - 1 + size) % size];
    }

    public boolean isFull() {
        return count == size;
    }
    
    public int size() {
        return count;
    }

    public boolean isEmpty() {
        return count == 0;
    }
}
```

