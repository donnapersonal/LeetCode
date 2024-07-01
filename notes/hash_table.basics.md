# 哈希表

### 理论基础

`哈希表`（`Hash table`，也称为`散列表`或`散列映射`），是**根据关键码的值而直接进行访问**的数据结构
> 是一种实现了关联数组抽象数据类型的数据结构，它通过一个`哈希函数`将键(key)映射到表中的一个位置来访问记录，以加快查找的速度
> 
> 这个映射过程称为`哈希化`，这个位置称为`哈希值`

其实直白来讲数组就是一张哈希表，哈希表中关键码就是数组的索引下标，然后通过下标直接访问数组中的元素，如下图所示：

![alt text](hash1.png)

哈希表能解决什么问题呢？-- **一般哈希表是用来快速判断一个元素是否出现集合里**，如要查询一个名字是否在这所学校里

> 要枚举的话时间复杂度是 `O(n)`，但若使用哈希表则只需要 `O(1)` 就可以做到

只需初始化把这所学校里学生的名字都存在哈希表里，查询时通过索引直接就可知道这位同学在不在这所学校里 -- 将学生姓名映射到哈希表上就涉及到了`哈希函数（hash function）`

### 几个关键概念及原理

**key 是唯一的，value 可以重复**

哈希表中，不可能出现两个相同的 `key`，而 `value` 是可以重复的

类比数组，数组里每个索引都是唯一的，不能说这个数组有两个索引 `0`，至于数组里存什么元素，随便

**哈希函数**

哈希函数的作用是把任意长度的输入（`key`）转化成固定长度的输出（`索引`）

增删查改的方法中都会用到哈希函数来计算索引，若设计的这个哈希函数复杂度是 `O(n)`，则哈希表的增删查改性能就会退化成 `O(n)`，因此这个函数的性能很关键

这个函数要保证：输入相同的 `key`，输出也要相同，这样才能保证哈希表的正确性

一个好的哈希函数应尽量减少冲突，即不同的键尽量映射到表的不同位置
  
![alt text](hash2.png)

若 `hashCode` 得到的数值大于哈希表的大小，即 `tableSize`，怎么办？
- 为了保证映射出来的索引数值都落在哈希表上，会再次对数值做一个`取模`的操作，这样就保证学生姓名一定可以映射到哈希表上

若学生数量大于哈希表的大小，此时就算哈希函数计算的再均匀，也避免不了会有几位学生的名字同时映射到哈希表（即，同个索引下标的位置）

此时就需要`冲突解决`

![alt text](hash3.png)

**数组**
- 这是存储实际数据（键值对）的地方
- 哈希表的大小通常是预先设定的，有些哈希表实现可以动态扩展大小

**冲突解决（哈希碰撞）**
  
当两个键的哈希值相同时，则会发生冲突
> 一定会出现哈希冲突，因为`哈希函数`相当于是把一个无穷大的空间映射到了一个有限的索引空间，必然会有不同的 `key` 映射到同一个索引上

- 常见的解决方法：
  > `链表法`，即在每个数组槽位存储一个链表，所有哈希值相同的元素都放在同一个链表中  
  > `开放寻址法（线性探查法）`，即探测数组的其他位置，直到找到空槽位
  
- 链表法(拉链法)：

  相当于是哈希表的底层数组并不直接存储 `value` 类型，而是存储一个链表，当有多个不同的 `key` 映射到了同一个索引上，这些 `key -> value` 对儿就存储在这个链表中，这样就能解决哈希冲突的问题

  ![alt text](hash4.png)

  > 数据规模是 `dataSize`， 哈希表的大小为 `tableSize`
  > 
  > 注意：该方法就是要选择适当的哈希表的大小，这样既不会因为数组空值而浪费大量内存，也不会因为链表太长而在查找上浪费太多时间

- 开放寻址法(线性探测法)：
  
  线性探查法的思路：一个 `key` 发现算出来的 `index` 值已被别的 `key` 占了，它就去 `index + 1` 的位置看看，若还是被占则继续往后找，直到找到一个空的位置为止

  > 注意：使用该方法时，一定要保证 `tableSize` 大于 `dataSize`，因为需要依靠哈希表中的空位来解决碰撞问题

  ![alt text](hash5.png)

**扩容和负载因子**

拉链法和线性探查法虽然能解决哈希冲突的问题，但会导致性能下降

如拉链法，算出 `index = hash(key)` 索引，当发现是个链表，就得遍历这个链表，在里面找到要的 `value`，这个过程的时间复杂度是 `O(k)`，`k` 是这个链表的长度

同理，线性探查法算出 `index = hash(key)`，你去这个索引位置查看，发现存储的不是要找的 `key`，并不能确定这个 `key` 真的不存在，须顺着这个索引往后找，直到找到一个空的位置或找到这个 `key` 为止，这个过程的时间复杂度也是 `O(k)`，`k` 为连续探查的次数

若频繁出现哈希冲突，则 `K` 的值就会增大，这个哈希表的性能就会显著下降

为什么会频繁出现哈希冲突？
- 哈希函数设计的不好，导致 `key` 的哈希值分布不均匀，很多 `key` 映射到了同一个索引上
- 哈希表里已装了太多的 `key-value` 对，这种情况下即使哈希函数再完美，也没办法避免哈希冲突

对于第一个问题，开发编程语言标准库已经设计好了哈希函数，只要调用即可

对于第二个问题，就引出了`负载因子`的概念
- 负载因子是一个哈希表装满的程度的度量，一般来说，负载因子越大，说明哈希表里面的 `key-value` 对越多，哈希冲突的概率就越大
- 负载因子的计算公式即 `size / table.length`，其中 `size` 是哈希表里的 `key-value` 对的数量，`table.length` 是哈希表底层数组的容量

可以发现，用`拉链法`实现的哈希表，其负载因子可以无限大，因为链表可以无限延伸；用线性探查法实现的哈希表，负载因子不会超过 `1`

当哈希表内元素达到负载因子时，哈希表会扩容，就是把哈希表底层 table 数组的容量扩大，把数据搬移到新的大数组中，这样负载因子就减小了

### 哈希表的特点

`时间复杂度`：
- 理想状态下，哈希表的插入、删除和查找操作的时间复杂度接近于 `O(1)`
- 但最坏的情况下，如发生大量冲突时，操作的时间复杂度可能退化到 `O(n)`  
  
`应用广泛`：哈希表在编程中非常常用，尤其是在`需要快速数据查找的场景`，如`数据库索引`、`缓存实现`、`对象存储`等  

`动态调整`：哈希表通常会在`负载因子`（**表中已有元素占总槽数的比例**）达到一定阈值时进行扩展，以保持操作的高效性

### 常见的三种哈希结构

当想使用哈希法来解决问题的时候，一般会选择如下三种数据结构：
- 数组
- set(集合)
- map(映射)

在 `C++` 中，set 和 map 分别提供以下三种数据结构，其底层实现以及优劣如下表所示：

| 集合 | 底层实现 | 是否有序 | 数值是否可以重复 | 能否更改数值 | 查询效率 | 增删效率 |
| --- | --- | --- | --- | --- | --- | --- |
| std::set | 红黑树 | 有序 | 否 | 否 | O(log n) | O(log n) |
| std::multiset | 红黑树 | 有序 | 是 | 否 | O(log n) | O(log n) |
| std::unordered_set | 哈希表 | 无序 | 否 | 否 | O(1) | O(1) |

`std::unordered_set` 底层实现为`哈希表`

`std::set` 和 `std::multiset` 的底层实现是`红黑树(一种平衡二叉搜索树)`，所以 `key` 值是有序的，但 **`key` 不可以修改**，改动 `key` 值会导致整棵树的错乱，所以只能`删除和增加`

| 集合 | 底层实现 | 是否有序 | 数值是否可以重复 | 能否更改数值 | 查询效率 | 增删效率 |
| --- | --- | --- | --- | --- | --- | --- |
| std::set | 红黑树 | key 有序 | key 不可重复 | key 不可修改 | O(log n) | O(log n) |
| std::multiset | 红黑树 | key 有序 | key 可重复 | key 不可修改 | O(log n) | O(log n) |
| std::unordered_set | 哈希表 | key 无序 | key 不可重复 | key 不可修改 | O(1) | O(1) |

`std::unordered_map` 底层实现为`哈希表`

`std::map` 和 `std::multimap` 的底层实现是`红黑树`；同理，`std::map` 和 `std::multimap` 的 `key` 也是有序的（这个问题也经常作为面试题，考察对语言容器底层的理解）

当要使用`集合`来解决哈希问题时，优先使用 `unordered_set`，因为它的查询和增删效率是最优的 
> - 若需要集合是`有序`的，就用 `set`
> 
> - 若要求`不仅有序还要有重复数据`，就用 `multiset`

再来看一下 `map`，`map` 是一个 `key value` 的数据结构，`map` 中对 `key` 是有限制但对 `value` 没有限制（因为 `key` 的存储方式使用`红黑树`实现的）

> 其他语言例如：`Java` 里的 `HashMap、TreeMap` 也是一样的原理，可以灵活贯通

虽然 `std::set、std::multiset` 的底层实现是`红黑树`，使用`红黑树`来`索引和存储`，不过使用方式还是`哈希法`，即 **`key` 和 `value`** -- 所以使用这些数据结构来解决映射问题的方法依然称之为`哈希法`， `map` 同理

### 总结

1. 当遇到要快速判断一个元素是否出现集合里时，要考虑`哈希法`

2. 哈希法是`牺牲了空间换取时间`，因为要使用额外的`数组`、`set` 或 `map` 来存放数据

> 因此，若做面试题时遇到需要`判断一个元素是否出现过的场景`也应第一时间想到`哈希法`！

3. 为什么常说哈希表的增删查改效率都是 `O(1)`？-- 因为哈希表底层就是操作一个`数组`，其主要的时间复杂度来自于`哈希函数计算索引和哈希冲突`。只要保证哈希函数的复杂度在 `O(1)`，且合理解决哈希冲突的问题，则增删查改的复杂度就都是 `O(1)`
   
4. 哈希表的遍历顺序为什么会变化？-- 因为哈希表在达到负载因子时会扩容，这个扩容过程会导致哈希表底层的数组容量变化，哈希函数计算出来的索引也会变化，所以哈希表的遍历顺序也会变化
   
5. 哈希表的增删查改效率一定是 `O(1)` 吗？-- 不一定，只有哈希函数的复杂度是 `O(1)`，且合理解决哈希冲突的问题，才能保证增删查改的复杂度是 `O(1)`
  - 哈希冲突好解决，都是有标准答案的，关键是哈希函数的计算复杂度
  - 若使用了错误的 `key` 类型，则哈希表的复杂度就会退化成 `O(n)`

### 用链表法实现哈希表

```java
import java.util.LinkedList;
public class MyChainingHashMap<K, V> {
    // 拉链法使用的单链表节点，存储 key-value 对
    private static class KVNode<K, V> {
        K key;
        V value;
        // 因为使用了内置的 LinkedList 类，所以不用 next 指针
        // 不用我们自己实现链表的逻辑
        KVNode(K key, V val) {
            this.key = key;
            this.value = val;
        }
    }
    
    // 哈希表的底层数组，每个数组元素是一个链表，链表中每个节点是 KVNode 存储键值对
    private LinkedList<KVNode<K, V>>[] table;
    // 哈希表中存入的键值对个数
    private int size;
    // 底层数组的初始容量
    private static final int INIT_CAP = 4;

    public MyChainingHashMap() {
        this(INIT_CAP);
    }

    public MyChainingHashMap(int initCapacity) {
        size = 0;
        // 初始化哈希表
        table = (LinkedList<KVNode<K, V>>[]) new LinkedList[initCapacity];
        for (int i = 0; i < table.length; i++) {
            table[i] = new LinkedList<>();
        }
    }

    // 增/改
    // 添加 key -> val 键值对
    // 若键 key 已存在，则将值修改为 val
    public void put(K key, V val) {
        if (key == null) {
            throw new IllegalArgumentException("key is null");
        }
        LinkedList<KVNode<K, V>> list = table[hash(key)];
        // 若 key 之前存在，则修改对应的 val
        for (KVNode<K, V> node : list) {
            if (node.key.equals(key)) {
                node.value = val;
                return;
            }
        }
        // 若 key 之前不存在，则插入，size 增加
        list.add(new KVNode<>(key, val));
        size++;

        // 若元素数量超过了负载因子，进行扩容
        if (size >= table.length * 0.75) {
            resize(table.length * 2);
        }
    }

    // 删
    // 删除 key 和对应的 val
    public void remove(K key) {
        if (key == null) {
            throw new IllegalArgumentException("key is null");
        }
        LinkedList<KVNode<K, V>> list = table[hash(key)];
        // 若 key 存在，则删除，size 减少
        for (KVNode<K, V> node : list) {
            if (node.key.equals(key)) {
                list.remove(node);
                size--;

                // 缩容，当负载因子小于 0.125 时，缩容
                if (size <= table.length / 8) {
                    resize(table.length / 4);
                }
                return;
            }
        }
    }

    // 查
    // 返回 key 对应的 val，若 key 不存在，则返回 null
    public V get(K key) {
        if (key == null) {
            throw new IllegalArgumentException("key is null");
        }
        LinkedList<KVNode<K, V>> list = table[hash(key)];
        for (KVNode<K, V> node : list) {
            if (node.key.equals(key)) {
                return node.value;
            }
        }
        return null;
    }

    // 返回所有 key
    public List<K> keys() {
        List<K> keys = new LinkedList<>();
        for (LinkedList<KVNode<K, V>> list : table) {
            for (KVNode<K, V> node : list) {
                keys.add(node.key);
            }
        }
        return keys;
    }

    public int size() {
        return size;
    }

    // 哈希函数，将键映射到 table 的索引
    private int hash(K key) {
        return (key.hashCode() & 0x7fffffff) % table.length;
    }

    private void resize(int newCap) {
        // 构造一个更大容量的 HashMap
        MyChainingHashMap<K, V> newMap = new MyChainingHashMap<>(newCap);
        // 穷举当前 HashMap 中的所有键值对
        for (LinkedList<KVNode<K, V>> list : table) {
            for (KVNode<K, V> node : list) {
                // 将键值对转移到新的 HashMap 中
                newMap.put(node.key, node.value);
            }
        }
        // 将当前 HashMap 的底层 table 换掉
        this.table = newMap.table;
    }

    public static void main(String[] args) {
        MyChainingHashMap<Integer, Integer> map = new MyChainingHashMap<>();
        map.put(1, 1);
        map.put(2, 2);
        map.put(3, 3);
        System.out.println(map.get(1)); // 1
        System.out.println(map.get(2)); // 2

        map.put(1, 100);
        System.out.println(map.get(1)); // 100

        map.remove(2);
        System.out.println(map.get(2)); // null

        System.out.println(map.keys()); // [1, 3]（顺序可能不同）
    }
```

### 用开放寻址法实现哈希表

#### rehash 版本

```java
import java.util.LinkedList;
import java.util.List;

public class MyLinearProbingHashMap1<K, V> {

    private static class KVNode<K, V> {
        K key;
        V val;

        KVNode(K key, V val) {
            this.key = key;
            this.val = val;
        }
    }

    // 真正存储键值对的数组
    private KVNode<K, V>[] table;
    // HashMap 中的键值对个数
    private int size;
    // 默认的初始化容量
    private static final int INIT_CAP = 4;

    public MyLinearProbingHashMap1() {
        this(INIT_CAP);
    }

    public MyLinearProbingHashMap1(int initCapacity) {
        size = 0;
        table = (KVNode<K, V>[]) new KVNode[initCapacity];
    }

    // 增/改
    public void put(K key, V val) {
        if (key == null) {
            throw new IllegalArgumentException("key is null");
        }
        // 我们把负载因子默认设为 0.75，超过则扩容
        if (size >= table.length * 0.75) {
            resize(table.length * 2);
        }

        int index = getKeyIndex(key);
        // key 已存在，修改对应的 val
        if (table[index] != null) {
            table[index].val = val;
            return;
        }

        // key 不存在，在空位插入
        table[index] = new KVNode<>(key, val);
        size++;
    }

    // 删
    // 删除 key 和对应的 val
    public void remove(K key) {
        if (key == null) {
            throw new IllegalArgumentException("key is null");
        }

        // 缩容，当负载因子小于 0.125 时，缩容
        if (size <= table.length / 8) {
            resize(table.length / 4);
        }

        int index = getKeyIndex(key);

        if (table[index] == null) {
            // key 不存在，不需要 remove
            return;
        }

        // 开始 remove
        table[index] = null;
        size--;
        // 保持元素连续性，进行 rehash
        index = (index + 1) % table.length;
        for (; table[index] != null; index = (index + 1) % table.length) {
            KVNode<K, V> entry = table[index];
            table[index] = null;
            // 这里减一，因为 put 里面又会加一
            size--;
            put(entry.key, entry.val);

        }
    }

    // 查
    // 返回 key 对应的 val，如果 key 不存在，则返回 null
    public V get(K key) {
        if (key == null) {
            throw new IllegalArgumentException("key is null");
        }
        int index = getKeyIndex(key);
        if (table[index] == null) {
            return null;
        }
        return table[index].val;
    }

    // 返回所有 key（顺序不固定）
    public List<K> keys() {
        LinkedList<K> keys = new LinkedList<>();
        for (KVNode<K, V> entry : table) {
            if (entry != null) {
                keys.addLast(entry.key);
            }
        }
        return keys;
    }

    public int size() {
        return size;
    }

    // 哈希函数，将键映射到 table 的索引
    // [0, table.length - 1]
    private int hash(K key) {
        // int: 0000 0000 0000 ... 0000
        //    : 0111 1111 1111 ... 1111
        return (key.hashCode() & 0x7fffffff) % table.length;
    }

    // 对 key 进行线性探查，返回一个索引
    // 若 key 不存在，返回的就是下一个为 null 的索引，可用于插入
    private int getKeyIndex(K key) {
        int index;
        for (index = hash(key); table[index] != null; index = (index + 1) % table.length) {
            if (table[index].key.equals(key))
                return index;
        }
        return index;
    }

    private void resize(int newCap) {
        MyLinearProbingHashMap1<K, V> newMap = new MyLinearProbingHashMap1<>(newCap);
        for (KVNode<K, V> entry : table) {
            if (entry != null) {
                newMap.put(entry.key, entry.val);
            }
        }
        this.table = newMap.table;
    }
}
```

#### 特殊值标记版本

```java
import java.util.LinkedList;
import java.util.List;

public class MyLinearProbingHashMap2<K, V> {

    private static class KVNode<K, V> {
        K key;
        V val;

        KVNode(K key, V val) {
            this.key = key;
            this.val = val;
        }
    }

    // 被删除的 KVNode 的占位符
    private final KVNode<K, V> DUMMY = new KVNode<>(null, null);

    // 真正存储键值对的 table 数组
    private KVNode<K, V>[] table;
    // HashMap 中的键值对个数
    private int size;
    // 默认的初始化容量
    private static final int INIT_CAP = 4;

    public MyLinearProbingHashMap2() {
        this(INIT_CAP);
    }

    public MyLinearProbingHashMap2(int cap) {
        size = 0;
        table = (KVNode<K, V>[]) new KVNode[cap];
    }

    // 增/改
    // 添加 key -> val 键值对
    // 如果键 key 已存在，则将值修改为 val
    public void put(K key, V val) {
        if (key == null) {
            throw new IllegalArgumentException("key is null");
        }

        // 负载因子默认设为 0.75，超过则扩容
        if (size >= table.length * 0.75) {
            resize(table.length * 2);
        }

        int index = getKeyIndex(key);
        if (index != -1) {
            // key 已存在，修改对应的 val
            table[index].val = val;
            return;
        }

        // key 不存在
        KVNode<K, V> x = new KVNode<>(key, val);
        // 在 table 中找一个空位或者占位符，插入
        index = hash(key);
        while (table[index] != null && table[index] != DUMMY) {
            index = (index + 1) % table.length;
        }
        table[index] = x;
        size++;
    }

    // 删
    // 删除 key 和对应的 val，并返回 val
    // 若 key 不存在，则返回 null
    public void remove(K key) {
        if (key == null) {
            throw new IllegalArgumentException("key is null");
        }

        // 缩容
        if (size < table.length / 8) {
            resize(table.length / 2);
        }

        int index = getKeyIndex(key);
        if (index == -1) {
            // key 不存在，不需要 remove
            return;
        }

        // 开始 remove
        // 直接用占位符表示删除
        table[index] = DUMMY;
        size--;
    }

    // 查 
    // 返回 key 对应的 val
    // 如果 key 不存在，则返回 null
    public V get(K key) {
        if (key == null) {
            throw new IllegalArgumentException("key is null");
        }

        int index = getKeyIndex(key);
        if (index == -1) {
            return null;
        }

        return table[index].val;
    }

    public List<K> keys() {
        LinkedList<K> keys = new LinkedList<>();
        for (KVNode<K, V> entry : table) {
            if (entry != null) {
                keys.addLast(entry.key);
            }
        }
        return keys;
    }

    public int size() {
        return size;
    }

    // 对 key 进行线性探查，返回一个索引
    // 根据 keys[i] 是否为 null 判断是否找到对应的 key
    private int getKeyIndex(K key) {
        int step = 0;
        for (int i = hash(key); table[i] != null; i = (i + 1) % table.length) {
            KVNode<K, V> entry = table[i];
            // 遇到占位符直接跳过
            if (entry == DUMMY) {
                continue;
            }
            if (entry.key.equals(key)) {
                return i;
            }
            step++;
            // 防止死循环
            if (step == table.length) {
                // 这里可以触发一次 resize，把标记为删除的占位符清理掉
                resize(table.length);
                return -1;
            }
        }
        return -1;
    }

    // 哈希函数，将键映射到 table 的索引
    // [0, table.length - 1]
    private int hash(K key) {
        // int: 0000 0000 0000 ... 0000
        //    : 0111 1111 1111 ... 1111
        return (key.hashCode() & 0x7fffffff) % table.length;
    }

    private void resize(int cap) {
        MyLinearProbingHashMap2<K, V> newMap = new MyLinearProbingHashMap2<>(cap);
        for (KVNode<K, V> entry : table) {
            if (entry != null && entry != DUMMY) {
                newMap.put(entry.key, entry.val);
            }
        }
        this.table = newMap.table;
    }
```

### 哈希集合代码实现

哈希集合的主要使用场景是`去重`，因为它的特性是：`不会出现重复元素`，可以在 `O(1)` 时间内增删元素，可以在 `O(1)` 时间内判断一个元素是否存在

```java
import java.util.HashMap;

public class MyHashSet<K> {
    // 随便创建一个值，作为 value 占位符
    private static final Object PRESENT = new Object();
    // 底层 HashMap，这里直接用标准库了，用前文实现的哈希表也可以
    private final HashMap<K, Object> map = new HashMap<>();

    // 向哈希表添加一个键值对
    public void add(K key) {
        
        map.put(key, PRESENT);
    }

    // 从哈希表中移除键 key
    public void remove(K key) {
        map.remove(key);
    }

    // 判断哈希表中是否包含键 key
    public boolean contains(K key) {
        return map.containsKey(key);
    }

    public int size() {
        return map.size();
    }
}
```