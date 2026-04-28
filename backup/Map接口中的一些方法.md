```
V merge(K key, V value, BiFunction remappingFunction)
```
- **`K key`**： 你要操作的**键**。
    
- **`V value`**： 你要合并的**新值**。如果这个 `key` 不存在，这个 `value` 就会被直接 `put` 进去。
    
- **`BiFunction remappingFunction`**： **这是最关键的部分。** 这是一个函数（通常用 Lambda 表达式来写），它告诉 `merge`：“**如果 key 已经存在**，该如何合并**旧值**和**新值**？”
```
public static void mergeMap(Map<String,Integer> mainMap,
Map<String,Integer> childMap)
{  
    childMap.forEach((key,value)->mainMap.merge(key,value,Integer::sum));  
}
```
这是一种写法用来不断更新key对应的value值，mainMap是主要的一个Map,而childMap是用来给mainMap更新数据用的数据集。
#### 添加和修改数据

- `V put(K key, V value)`
    
    - **作用**：将指定的“键”和“值”放入 `Map` 中。
        
    - **注意**：
        
        1. 如果这个 `key` **不存在**，它会添加这个新的键值对，并返回 `null`。
            
        2. 如果这个 `key` **已经存在**，它会用新的 `value` **覆盖**旧的 `value`，并返回**被覆盖的那个旧值**。
            
    ```
    Map<String, String> map = new HashMap<>();
    map.put("user.name", "Alice"); // 添加 "user.name" -> "Alice"
    map.put("user.role", "Admin"); // 添加 "user.role" -> "Admin"
    
    String oldValue = map.put("user.name", "Bob"); // 覆盖 "Alice"
    // 此时 map 中 "user.name" 对应 "Bob"
    // oldValue 变量的值是 "Alice"
    ```
#### 访问和获取数据

- `V get(Object key)`
    
    - **作用**：根据 `key` 获取对应的 `value`。
        
    - **注意**：如果 `key` 在 `Map` 中**不存在**，这个方法会返回 `null`。
        
    ```
    String name = map.get("user.name"); // name 会得到 "Bob"
    String email = map.get("user.email"); // email 会得到 null，因为这个 key 不存在
    ```
`V getOrDefault(Object key, V defaultValue)`

- **作用**：这个方法更安全。它尝试根据 `key` 获取 `value`，如果 `key` 不存在，它会返回你指定的 `defaultValue`（默认值），而不是 `null`。
    
```
// 假设 map 中没有 "user.email"
String email = map.getOrDefault("user.email", "default@example.com");
// email 会得到 "default@example.com"
```
#### 检查 Map 的状态

- `int size()`
    
    - **作用**：返回 `Map` 中键值对的**数量**。
        
    ```
    int count = map.size(); // count 会是 2 (因为 map 中有 "user.name" 和 "user.role")
    ```
    
- `boolean isEmpty()`
    
    - **作用**：检查 `Map` 是否为**空**（即 `size()`是不是 0）。
        
- `boolean containsKey(Object key)`
    
    - **作用**：检查 `Map` 中是否**包含**指定的 `key`。
        
    ```
    boolean hasName = map.containsKey("user.name"); // true
    boolean hasEmail = map.containsKey("user.email"); // false
    ```
    
- `boolean containsValue(Object value)`
    
    - **作用**：检查 `Map` 中是否**包含**指定的 `value`。（这个方法相对用得少一点，而且效率可能不高，因为它需要遍历所有值）。
#### 删除数据

- `V remove(Object key)`
    
    - **作用**：根据 `key` 删除这个键值对。
        
    - **返回值**：返回这个 `key` 之前对应的 `value`。如果 `key` 不存在，返回 `null`。
        
    ```
    String removedValue = map.remove("user.role"); // removedValue 会是 "Admin"
    // 此时 map 中不再有 "user.role"
    ```
    
- `void clear()`
    
    - **作用**：清空整个 `Map`，删除所有的键值对。
#### 遍历 Map (非常重要)

你不能像遍历 `List` 那样直接遍历 `Map`，但有三种主要的方式来获取 `Map` 里的内容：

1. **遍历所有的键 (Keys)**：`keySet()`
    
    - 获取一个包含所有 `key` 的 `Set` 集合，然后你可以遍历这个 `Set`，并使用 `get()` 来获取每个 `value`。
        
    ```
    // map 中有 "user.name" -> "Bob"
    for (String key : map.keySet()) {
        String value = map.get(key);
        System.out.println("键: " + key + ", 值: " + value);
    }
    // 输出: 键: user.name, 值: Bob
    ```
    
2. **遍历所有的值 (Values)**：`values()`
    
    - 获取一个包含所有 `value` 的 `Collection` 集合。这在你**只关心值，不关心键**的时候很有用。
        
    ```
    for (String value : map.values()) {
        System.out.println("值: " + value);
    }
    // 输出: 值: Bob
    ```
    
3. **遍历所有的键值对 (Entries)**：`entrySet()` (最高效的方式)
    
    - 这是**最推荐**的遍历方式。它获取一个包含所有“键值对”对象（`Map.Entry`）的 `Set` 集合。
        
    - `Map.Entry` 对象有 `.getKey()` 和 `.getValue()` 两个方法。
        
    ```
    for (Map.Entry<String, String> entry : map.entrySet()) {
        String key = entry.getKey();
        String value = entry.getValue();
        System.out.println("键: " + key + ", 值: " + value);
    }
    // 输出: 键: user.name, 值: Bob
    ```
    - Map.Entry<K,V>是nested interface，nested interface意思就是一个interface里嵌套一个interface，类似于SuperClass里有个SubClass。