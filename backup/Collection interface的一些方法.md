```
contains(itemName)
```
`contains` 这个方法的返回类型就是 **`boolean`**（布尔值）。

- 如果 `items` 集合中**包含**你要查找的 `itemName`，它就返回 `true`。
- 如果 `items` 集合中**不包含**你要查找的 `itemName`，它就返回 `false`。

**元素添加 (Adding Elements)**
```
boolean add(E e)
```
确保此 collection 包含指定的元素。如果此 collection 发生了更改，则返回 `true`。
```
boolean addAll(Collection<? extends E> c)
```
将指定 collection 中的所有元素添加到此 collection 中。如果此 collection 发生了更改，则返回 `true`。
`add(E e)` 的设计是为了防止在集合中意外地放入错误类型的对象，保证了集合的**类型一致性**和**类型安全**。

**元素移除 (Removing Elements)**
```
boolean remove(Object o)
```
从此 collection 中移除指定元素的单个实例（如果存在）。
```
boolean removeAll(Collection<?> c)
```
移除此 collection 中也包含在指定 collection 中的所有元素。
```
boolean retainAll(Collection<?> c)
```
仅保留此 collection 中也包含在指定 collection 中的元素（取交集）。
```
void clear()
```
移除此 collection 中的所有元素。

**查询与检查 (Querying and Checking)**
```
int size()
```
如果此 collection 不包含任何元素，则返回 `true`。
```
boolean contains(Object o)
```
如果此 collection 包含指定的元素，则返回 `true`。
```
boolean containsAll(Collection<?> c)
```
如果此 collection 包含指定 collection 中的所有元素，则返回 `true`。

**迭代与转换 (Iteration and Conversion)**
```
Iterator<E> iterator()
```
返回在此 collection 的元素上进行迭代的迭代器。
```
Object[] **toArray**()
```
返回包含此 collection 中所有元素的数组。
```
<T> T[] toArray(T[] a)
```
返回包含此 collection 中所有元素的数组，并指定返回数组的运行时类型。

```
stream()
```
将一个集合（Collection）转换成一个流（Stream）

**List,Set,Queue都是Collection接口下的子接口**