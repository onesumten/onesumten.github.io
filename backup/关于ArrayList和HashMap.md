ArrayList是一个实现类，它使用一个dynamic array来实现它的功能。它可以让后面的数据无限制的加入到这个List中。
我们可以使用get.(0)类似的功能来获取到这个List的第一个元素。（其中0是其index)

HashMap是一个实现类，它可以让使用者快速查找其value对应的key.

`Arrays.asList()` 是一个静态工具方法，它的作用是：

1. 接受任意数量的参数（零个、一个或多个）。
2. 返回一个**固定大小的 `List` 视图**，该列表包含你传入的这些元素。
### 示例解释

|**调用方式**|**预期返回的 List 类型（基于上下文）**|**内部元素**|
|---|---|---|
|`asList()`|`List<GraphNode<Integer>>` 或 `List<Integer>`|空列表 `[]`|
|`asList(node1)`|`List<GraphNode<Integer>>`|`[node1]`|
|`asList(2)`|`List<Integer>`|`[2]`|
|`asList(node1, node2)`|`List<GraphNode<Integer>>`|`[node1, node2]`|
- `Arrays.asList()` 返回的列表是**固定大小**的。你不能使用 `.add()` 或 `.remove()` 来改变它的大小，否则会抛出 `UnsupportedOperationException`。
    
- **但是**，如果你修改了列表中的**元素本身**（例如，如果你传入的是一个对象引用），那么原始对象是会被修改的。