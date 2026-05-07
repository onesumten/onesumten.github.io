# Record
1. record本身隐性的继承了java.lang.Record类，所以不能继承别的类，也不能被别的类继承，但是可以继承接口。
2. record的field特性都是private final，所以不能新加一个field，但是可以新加一个static field。
3. record内部可以重写访问器，也可以重写equals(),hashcode(),toString()等方法，如果不重写就默认record自己给的这三个方法，也可以自己新写方法。
```
public record User(String name, int age) {
    // 重写 name 的访问器
    @Override
    public String name() {
        return name != null ? name.toUpperCase() : "UNKNOWN";
    }
}
```
4. 可以写compact constructor，用于校验，可以写判断和异常抛出，不需要写参数，编译器会自动给你配好this.name = name这样的东西。
```
public record User(String name, int age) {
    public User {
        // 这里的 name 和 age 已经自动传进来了
        if (age < 0) {
            throw new IllegalArgumentException("年龄不能为负");
        }
        // 注意：这里不需要写 this.name = name; 
        // 执行完大括号里的代码后，编译器会自动完成赋值
    }
}
```
1. 自定义构造器的时候必须附带一个默认值，比如this(name, 18)而不是this.name = name。
2. 工厂函数在record中必须是static的，因为工厂函数是负责创建对象的，如果不是static说明需要先有一个对象才能使用。

# var
1. 类成员变量（Field）,var 仅限局部变量，类成员需要明确类型以确定内存布局。
2. 方法参数,方法签名必须明确，否则编译器无法进行类型检查。
3. 方法返回值类型,同样是为了保证 API 的确定性。
4. 未初始化的变量,var x; 编译器不知道该推断成什么。
5. 初始化为 null,var x = null; null 可以是任何引用类型，编译器会罢工。
6. Lambda 表达式,Lambda 需要目标类型，不能直接赋值给 var（除非强转）。
7. 数组初始化（简写）,"var arr = {1, 2, 3}; 不行，必须写 new int[]{1, 2, 3}。"

# 方法匹配优先级
```
1 (最高),精确原始类型匹配,"print(int a, int b)"
2,拓宽 (Widening),"print(long a, long b)"
3,自动装箱 (Boxing),"print(Integer a, Integer b)"
4 (最低),可变参数 (Varargs),print(int... sums)
```
1. 如果两个参数的类型不一样，会导致complier不知道对第一个参数是autobox还是unbox，对第二个参数也是同理。
```
// 如果你定义了这两个方法：
void print(Integer a, int b) {}
void print(int a, Integer b) {}

// 然后调用：
print(1, 2); // 编译报错！
```
2. unboxing只会发生在下面几种情况中：
   * 算术运算： 如 a + b, a - b, a * b 等。
   * 比较运算（含原始类型）： 如 a > 10 或 a == 100（一侧是包装类，另一侧是基本类型 int）。
   * 赋值给基本类型： int c = a;
   * 当我们使用a==b的时候，complier会比较a和b的引用地址如果a和b都是Integer这样的包装类，