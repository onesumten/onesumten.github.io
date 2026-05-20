# Collection interface中常用的实现类
1. List
   * ArrayList: An indexed sequence that grows and shrinks dynamically.
   * LinkedList: An ordered sequence that allows efficient insertion and removal at any location.
2. Deque
   * ArrayDeque: A double-ended queue that is implemented as a circular array.
3. Queue
   * PriorityQueue: A collection that allows efficient removal of the smallest element.
4. Set
   * HashSet: An unordered collection that rejects duplicates, and allows for efficient lookup of elements.
   * TreeSet: A sorted set.
   * EnumSet: A set of enumerated type values.
   * LinkedHashSet: A set that remembers the order in which the elements were inserted.

# Collection interface
1. 有两个基本的方法在Collection接口中
```java
public interface Collection<E> extends …
    {
    // 添加一个e到collection里，如果成功添加，返回true,如果失败，返回false
        boolean add(E e);
        Iterator<E> iterator();
        …
    }
```
2. 所有实现Collection接口的必须实现下面的方法
```java
int size()
boolean isEmpty()
boolean contains(Object obj)
boolean containsAll(Collection<?> c)
boolean add(E obj)
boolean addAll(Collection<? extends E> from)
boolean remove(Object obj)
boolean removeAll(Collection<?> c)
boolean retainAll(Collection<?> c)
void clear()
Object[] toArray()
<T> T[] toArray(T[] a)
default <T> T[]
toArray(IntFunction<T[]> generator)
```
3. AbstractCollection class是继承了Collection接口的一个抽象类，目的是为了减少重复代码书写，除了两个方法外其余的方法都有实现体，只要实现了这两个方法那么AbstractCollection类就可以帮你推导并实现其他方法。
```java
public abstract Iterator<E> iterator();
public abstract int size();
```

# Interator interface
```java
public interface Iterator<E>
    {
        // 如果至少还有一个元素需要访问，会返回true,否则false
        boolean hasNext();
        // 如果在迭代器末尾，再使用next(),会触发NoSuchElementException异常
        E next();
        default void remove();
        default void forEachRemaining(Consumer<? super E> action);
}
```
1. 对于interator来说，需要先请求一个迭代器，然后不断next，直到hasNext方法返回false.
```java
Collection<String> coll = …
Iterator<String> iter = coll.iterator()
    while (iter.hasNext()) {
        String element = iter.next();
        do something with element;
}
```
2. 也可以使用for-each loop实现,因为for-each loop实现了Interator接口，而Collection接口extends了Interator接口，因此，任何实现了Collection接口的都可以直接用在for-each中。
```java
for (String element : coll ) {
    do something with element;
}
```
3. 一开始，迭代器会生成在第一个元素左边，使用next方法后迭代器会跳转到第二个元素左边的空隙中，然后会返回第一个元素的引用。
4. 有两个方法分别是
   * Collection类的`forEach`：这个方法会遍历集合的所有元素。
    ```java
    // 假设这是你的集合
    ArrayList<Integer> coll = new ArrayList<>(); 
    coll.forEach(element -> System.out.println(element));
    ```
   * Interator类的`forEachRemaining`：只对迭代器还没访问完的，剩下的元素遍历。
    ```java
    // 先获取迭代器
    Interator<Integer> interator = coll.interator();
    // 开始遍历
    interator.forEachReamining(element -> System.out.println(element));
    ```

# List interface
1. 在List接口中，有一个方法叫remove(),这个方法有两个overloading method,如果有一个list里面只有[10,20,30],现在你使用remove(20)，由于20是int，所以会优先找到int index的overloading版本，造成异常，因为这里并没有索引为20的内容。
```java
boolean remove(int index)
boolean remove(Object o)
```

# ListIterator interface
1. add方法修改内容后新的内容会放在ListIterator左边。ListIterator光标如果没有其他移动永远放在一开始List的左边的位置。
```java
interface ListIterator<E> extends Iterator<E> {
    void add(E element); …
}
```
2. 有两个方法，和next()使用类似。
```java
boolean hasprevious()
E previous()
```
3. ListIterator可以使用方法在一开始就规定光标在哪里，Iterator不行。
```java
// 直接指定光标在 index 3 的位置 “A B C | D”
ListIterator<E> it = list.listIterator(3);
```

# Queue and Deque
1. 这两个接口均不支持在中间添加或者删除元素，因此不支持random access。
## Queue interface
```java
// 往queue的末尾添加元素，如果queue满了，add方法抛出IllegalStateException异常，offer方法返回false。
boolean add(E e);
boolean offer(E e);

// 移除queue第一个元素，如果queue为空，remove方法抛出NoSuchElementException异常,poll方法返回null。
E remove();
E poll();

// 查看queue第一个元素,如果queue为空，，element方法抛出NoSuchElementException异常，peek方法返回null。
E element();
E peek();
```
## Deque interface
```java
// 往这个Deque添加第一个元素前或者最后一个元素，如果Deque满了，返回false。
boolean offerFirst(E element);
boolean offerLast(E element);

// 移除并且返回第一个元素或者最后一个元素，如果Deque是空的，返回null。
E pollFirst();
E pollLast();

// 返回这个Deque的第一个元素或者最后一个元素，如果Deque是空的，返回null。
E peekFirst();
E peekLast();
```

## ArrayDeque class
```java
// 构建一个初始容量为16的无界Deque
ArrayDeque() 

// 构建一个指定的初始容量的无界Deque
ArrayDeque(int initialCapacity) 
```

# PriorityQueue interface
1. 允许无论任何顺序(正序还是倒叙或者无顺序)的数据进入到priorityQueue中，但是当使用remove这样的方法的时候也就是要里面的数据，priorityQueue只会给你优先级最高的。这里优先级最高的意思就是最小的意思。
2. 但是不能插入/移除priorityQueue内部某个地方，因为内部没有index之类的东西，所以内部是不会排序的，只能保证heap top也就是马上要弹出的是优先级最高的那个。
3. 内部heap是一个二叉树结构，每当add或者remove的时候会自发的把优先级最高的元素顶到树的顶点。性能最高。
4. 要么实现Comparable接口，要么实现Comparator接口。
   * Comparable接口需要你的数据拥有一个自带的规则，比如int从小到大，String会按照字典顺序首字母排序。
   * Comparator接口需要你自己创建规则。

# SequencedCollection interface
1. 有以下方法
```java
// 往这个collection的第一个元素前或者最后一个元素后添加元素，如果collection满了，抛出IllegalStateException异常。
void addFirst(E element);
void addLast(E element);

// 移除这个collection的第一个元素或者最后一个元素，如果collection为空，抛出NoSuchElementException异常。
E removeFirst();
E removeLast();

// 返回这个collection的第一个元素或者最后一个元素，如果这个collection为空，抛出NoSuchElementException异常。
E getFirst();
E getLast();
```

# Hash Table
1. 为了找到一个对象在表里的物理位置，哈希表会先计算这个对象的 hash code（哈希码），然后用它对桶的总数（也就是数组的长度）进行取模运算（求余数 %）。计算出来的余数就是该对象应该存放的桶的索引（Index）。如果一个对象的哈希码是 72268，现在表里有 128 个桶，那么它应该去几号桶？用数学算一下：72268/128 = 564.....76。所以这个对象会被精准投放到 76 号桶。
2. 如果这个bucket里面是空的，直接放进入。如果这个bucket里面不是空的，就使用equal方法挨个对比，看看我们要插入的这个元素是不是已经存在于这个桶里了（因为哈希表不允许重复）。
3. 如果哈希表装得太满，桶里的元素就会堆积，导致冲突越来越严重，查起来越来越慢。这时候它就需要彻底翻新，这个过程叫 Rehash（重新哈希）。哈希表会创建一个拥有更多桶的新数组，把老数组里的所有老员工重新计算余数、重新塞进新表里，最后把旧表直接扔掉。
4. 在 Java 中，控制什么时候开始翻新的指标叫加载因子（Load Factor）。Java 的默认值是 0.75。这意味着，当哈希表被装满了 75% 以上时（比如 100 个格子用了 75 个），Java 就会自动触发 Rehash。扩容时，新表的桶的数量通常是老表的两倍（用两倍多的座位来确保未来不容易冲突）。

# Set interface
## HashSet class
1. Set里不允许有重复的元素，当我们使用add方法的时候，它会优先找存不存在，如果不存在才会塞进去。
2. 惊人的单桶查找（Fast Look-up）：它的 contains(Object o) 方法之所以快到极致（接近 O(1) 时间复杂度），是因为它绝对不会去遍历整个集合。它拿着你要找的元素，通过哈希公式一算，直接“精准空降”到那一个特定的桶（Bucket）里。如果那个桶里没有，它就立马告诉你“没有”，根本不需要看其他的桶。
3. 如果使用Iterator去遍历hashSet，会发现顺序是混乱的，因为哈希表为了防止冲突，会把元素均匀地散射（Scatter）在不同的桶里。迭代器在遍历时，是按照桶的物理编号（0号桶、1号桶、2号桶……）挨个过去看的。由于元素在桶里的排布本来就是乱的，所以数出来的顺序自然也是乱的。
4. 对于一个对象元素存入hashSet中的，切记不能修改这个元素的某些可能会影响hashcode的内容，因为如果修改了会导致这个元素的hashCode改变，单桶查询会根据新的hashCode查询，但是数据还是存在原本的旧桶里面，就会导致找不到要的。
```java
// 构建一个空的hashSet
HashSet()

// 构建一个hashSet并且往这个hashSet中加入指定的collection。
HashSet(Collection<? extends E> elements) 

// 可以构建一个设定的初始容量的hashSet，loadFactor也可以设置，loadFactor默认是0.75，如果hashSet的元素数量也就是size和capacity的比值超过了load factor，那么就会扩容。
HashSet(int initialCapacity)
HashSet(int initialCapacity, float loadFactor)

// numMappings为你的元素数量，如果你知道会有多少个元素，就可以用这个。
static <E> HashSet<E> newHashSet(int numMappings)
```

## TreeSet class
1. TreeSet class对于传入的元素不需要规定其是否一定需要按照某个顺序，但是当我们遍历TreeSet里的时候，会自动按照初始规则排序。这是和hashSet不同的地方。
2. 内部数据结构是`red-black tree`，这个数据结构意味着当你放入一个元素的时候会自动排列将这个元素放入到规则要求的位置。
3. 红黑树比hash table要慢，但是比array和链表要快。
4. 和PriorityQueue interface类似，其可以通过自定义规则来排序使其遍历的时候按照你的规则。
```java
// 创建一个空的treeset
TreeSet()
// 创建一个空的treeset，并且把自己写的规则放进去。
TreeSet(Comparator<? super E> comparator)
// 创建一个空的treeset，并且把指定的collection放进去。 
TreeSet(Collection<? extends E> elements)
// 创建一个空的treeset，但是会继承s的排序规则。
TreeSet(SortedSet<E> s) 
```

## SortedSet interface
```java
// 可以自定义比较器，和treeset配合使用，如果依靠默认的比较器，返回null
Comparator<? super E> comparator()
// 情况 A：使用自然排序（不传比较器）
        SortedSet<Integer> setA = new TreeSet<>();
        setA.add(10);
        
        // 技术行为：因为靠 Integer 自己的 compareTo 排序，所以返回 null
        System.out.println(setA.comparator()); // 输出: null


        // 情况 B：传入定制的自定义比较器（降序排列）
        Comparator<Integer> descOrder = (num1, num2) -> Integer.compare(num2, num1);
        SortedSet<Integer> setB = new TreeSet<>(descOrder);
        setB.add(10);
        
        // 技术行为：因为使用了定制规则，所以返回对应的比较器对象实例
        System.out.println(setB.comparator()); 
```

```java
// 获取sorted set中第一个，如果没有则抛出NoSuchElementException异常。
E first();
// 获取sorted set中最后一个，如果没有则抛出NoSuchElementException异常。
E last();
```

## NavigableSet interface
1. 这个类是SortedSet interface的子类。
```java
// 返回小于value的最大元素,没有就返回null
E higher(E value); 
// 返回大于value的最小元素,没有就返回null
E lower(E value);

// 返回小于等于value的最大元素，没有就返回null
E ceiling(E value); 
// 返回大于等于value的最小元素，没有就返回null
E floor(E value);

// 移除set中最左边的元素，如果没有则返回null
E pollFirst(); 
// 移除set中最右边的元素，如果没有则返回null
E pollLast();
```

# Map interface
1. 在Map中，一个key只能对应一个value。
2. Collection Type：
   * HashMap：使用hash table
   * TreeMap：使用红黑树
   * EnumMap：这里的key都是enum里的内容。
   * LinkedHashMap：hashMap+双向链表。这里和hashMap的区别就是hashMap是乱序的，但是这里由于双向链表顺序就是和你传入的顺序一致。
   * WeakHashMap：一般来说hashMap不消失，那么就算key是null，GC回收机制也不会回收。但是如果使用weakHashMap的话，一旦key为null或者不用了，那么GC会直接抹除这个key。
   * IdentityHashMap：当比较两个key的时候，使用`==`这个符号而不是equal方法，只比较这两个key的在内存中的绝对地址。
3. map里的针对的都是key，不会对value进行针对。一般来说hash是最快的，除非你有顺序排序需求。
4. 如果使用`get(key)`后，key是不存在的，那么就会返回null。
5. 使用`getOrDefault()`方法当key不存在的时候，返回自己设定的值。
```java 
// 如果studetntUid不存在，则返回0
int score = scores.getOrDefault(studentUid, 0);
```
6. 使用`put(key, value)`方法后，会返回被替换前的old value。如果之前没有任何绑定过的value那么就返回null。
7. `remove(key)`方法会删除key及其对应的value，并且返回被删除的value。如果key不存在就返回null。
8. `size()`这个方法会返回目前在Map中的所有key-value，一个key-value就返回1，两个key-value就返回2。