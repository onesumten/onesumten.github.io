```
vertx.ifPresent(nextInventory::add)
```
**如果**这个 `Optional` 容器中**存在**一个值（Optional.of("RedKey")），那么就对这个值执行提供的操作；如果容器是空（Optional.empty()）的，就什么也不做。如果Optional容器里存在一个值，就将这个值添加到nextInventory这个对象中。类似于：
```
(item) -> nextInventory.add(item)
```
这里的item就是Optional里的值