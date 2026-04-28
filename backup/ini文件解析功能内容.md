```
final Pattern SECTION_PATTERN = Pattern.compile("^\\[(.+)]$");
```
这里的Pattern是一个类
complie()：**将一个字符串形式的正则表达式，转换（或“编译”）成一个 `Pattern` 对象。**
- **`^`**: 代表一行的开始。这确保了模式从一行的最左边开始匹配。
- **`\\[`**: 匹配一个字面的左方括号 `[`。因为 `[` 在正则表达式中有特殊含义（用来定义字符集），所以我们需要用一个反斜杠 `\` 来“转义”它，告诉引擎我们就是要匹配 `[` 这个字符本身。
- **`(.+)`**: 这是一个“捕获组”（Capturing Group）。
    - **`.`** 匹配除了换行符以外的任何单个字符。
    - **`+`** 是一个量词，表示它前面的元素（在这里是 `.`）必须出现一次或多次。
    - 所以 `.+` 会匹配一个或多个任意字符。
    - 括号 `()` 的作用是“捕获”这部分匹配到的内容，这样你之后就可以从匹配结果中单独把它提取出来。
- **`]`**: 匹配一个字面的右方括号 `]`。它通常不需要转义。
- **`$`**: 代表一行的结束。这确保了 `]` 后面没有任何其他字符。
matcher()：**根据我（`Pattern`）的规则，去创建一个用来在指定文本上进行匹配的“匹配器”（也就是一个 `Matcher` 对象）**。
```
Matcher sectionMatcher = SECTION_PATTERN.matcher(line);
```
在Pattern规则创建后，使用这个规则对line（也就是每行）进行匹配器（match）创建。

对于 `A.method(B)` 这种形式的调用：

- **`A` (方法左边的对象):** 这是执行动作的 **主体 (Subject)** 或 **调用者 (Caller)**。它拥有 `method` 这个能力。我们可以称它为 **目标对象**。它决定了 _做什么_ 和 _怎么做_。
    
- **`B` (方法括号内的对象):** 这是被动作影响的 **客体 (Object)** 或 **参数 (Argument)**。它是被处理、被使用、或作为方法执行时所需要的数据。我们可以称它为 **被处理的对象**。
```
return items.toArray(new GameItem[0]);
```
为什么toArray()方法在这里的小括号内必须要有内容：
1：**无参数的 `toArray()` 只会返回一个通用的 `Object[]`**
2：**由于类型擦除（type erasure）特性，在代码编译后，`List<GameItem>` 在运行时层面只知道它是一个 `List`，它“忘记”了自己专门存放 `GameItem`。所以必须使用一个new GameItem[0]告诉toArray()“我需要一个 `GameItem` 类型的数组。请你用这个类型作为模板，为我创建一个大小合适的新数组，并把列表里的所有元素都装进去。”**

### 总结

| **数据类型**            | **如何定义**                | **如何获取长度/大小**        |
| ------------------- | ----------------------- | -------------------- |
| **数组 (Array)**      | `int[] myArr;`          | `myArr.length` (属性)  |
| **集合 (Collection)** | `List<Integer> myList;` | `myList.size()` (方法) |
将数组类型的变量变为字符串以为了能够输出出来。（两种方法）
```
System.out.println(Arrays.toString(resultCircle));
```

```
System.out.println("Area: " + circleResults[0]);
System.out.println("Perimeter: " + circleResults[1]);
```