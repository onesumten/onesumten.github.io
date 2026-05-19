# Queue
1. ArrayDeque<E> and LinkedList<E> class

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
1. 也可以使用for-each loop实现,因为for-each loop实现了Interator接口，而Collection接口extends了Interator接口，因此，任何实现了Collection接口的都可以直接用在for-each中。
```java
for (String element : coll ) {
    do something with element;
}
```
1. 一开始，迭代器会生成在第一个元素左边，使用next方法后迭代器会跳转到第二个元素左边的空隙中，然后会返回第一个元素的引用。
2. 有两个方法分别是
   * Collection类的`forEach`：这个方法会遍历集合的所有元素。
    ```java
    // 假设这是你的集合
    ArrayList<Integer> coll = new ArrayList<>(); 
    // 【解答】
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
