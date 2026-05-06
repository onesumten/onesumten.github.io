# 继承(inheritance)
1. 子类的constructor如果没有使用super，编译器会自动前往父类找寻无参构造器，如果父类没有无参构造器，程序报错。
2. 子类能继承除了private以外的所有内容，这也是为什么使用super的原因。

# 多态(Polymorphism)
1. 子类的方法重写不能违背父类的基本契约，如果父类的规定是返回不能是负数，但是子类重写的时候说可以抛出异常，这样就属于违背父类，子类的输入要求
需要更少(更宽松)，输出要求需要更严格(更精细)。

# 方法调用(Method call)
1. 在complier time中，第一步获得参数是哪一个类的，然后获得这个类中名字为f（我们目前正在调用的方法）的所有方法，包括从父类继承过来的。第二步根据参数类型寻找匹配程度最高的方法，即参数相同的方法。
2. 在run time中，JVM 根据对象的实际身份去执行代码，静态绑定：如果是 private、static、final 方法或构造函数。这些方法不能被重写，所以 JVM 直接原地执行编译时确定的那个版本。动态绑定：如果不属于上述情况，则进入“动态绑定”，这是实现多态的核心。
3. 如果子类中的方法和父类中的方法同名，子类方法会覆盖掉父类。
4. 为了避免重复的寻找方法，我们会在类加载的时候生成一个method signature和具体实现的映射表，这样到时候只需要查表就可以找到我们要的方法。子类和父类的表会合并为一张表。如果使用super(a,b)这个东西的话就会直接访问父类的表。这个映射的格式是*方法名 -> 类名.方法名*，如果a是继承b的，其中某些方法是没修改过的，那么还是*b.方法名*，如果是自己新添加的或者修改过的，就是*a.方法名*。

# Final字段
1. final class不会自动将自己的field变为final，除非你手动设置为final。但是这个class内部的所有method会隐式的变成final。

# Open-closed principle
1. 软件应该对扩展开放，对修改关闭。
2. 对扩展开放 (Open for extension)：当需求发生变化时，我们可以通过添加新的代码（新类、新方法）来改变程序的行为。对修改关闭 (Closed for modification)：在增加新功能时，不允许修改已经通过测试的源代码。

# Cast
1. 通常将子类转换成父类的情况是安全的，因为子类通常会有额外功能，但是其他功能和父类一致，所以是大范围转换为小范围。而把父类转换成子类有时会出错，所以需要采用**instanceof**
2. 只能在继承关系中使用instanceof，其次如果是null instanceof Class，代码判断不会抛出异常，而是返回false，因为null refer to no object。
3. 我们使用instanceof需要有意义，比如*父类变量 instanceof 子类*就是有意义的，因为存在成功和失败两种情况；如果*子类变量 instanceof 父类*，那么永远是成功的，所以是无意义的,就会出现complier-time error。

# equal(Object otherObject)方法重写
1. 先判断比较的两个对象是否指向同一个地址
```
if (this == otherObject) return true;
```
2. 然后判断传入参数是否为null，如果为null，返回false
```
if (otherObject == null) return false;
```
3. 然后比较这两个对象是否是同一个类
```
if (getClass() != otherObject.getClass()) return false;
```
4. 现在我们知道otherObject是一个non-null的Employee类了
```
Employee other = (Employee) otherObject;
```
5. 最后比较这两个对象的field内容是否相同
```
return Objects.equals(name, other.name) && salary == other.salary && Objects.equals(hireDay, other.hireDay);
```
6. 如果这个类有父类，需要在第四步之前使用super.equals(Object otherObject)比较父类中的field
```
if (!super.equals(otherObject)) return false;
```
7. 如果子类比父类多一些属性，那么最好用getClass()方法，如果想使用instanceOf关键字进行比较，必须在父类的equals方法加上final关键字来让子类无法重写父类的equals方法。
   
# hashCode()方法重写
1. 如果重写了equals方法，那么hashCode()方法也需要重写。因为如果不重写会造成Collection接口中一些类的使用，两个对象如果相等，那么hashCode必须相等，如果两个对象不同，它们的hashCode有可能相同也有可能不同。使用hash方法时需要使用hash(你的field)而不是hash()。
```
int hashCode(){
    return Objects.hash(你的field);
}
```
2. 每一个对象都有一个default hash code称为identity hash code。
3. 对于array,使用Arrays.hashCode()。对于multi-dimensional arrays,使用Arrays.deepHashCode()。

# ArrayList
1. ArrayList相比于Array是可伸缩的，也就是说不是fixed。
2. 如果不设置100，那么arraylist会自动设置为10，也就是说当达到11的时候会扩容1.5倍。先设置为100的好处是可以避免多次扩容影响程序运行效率。
```
ArrayList<Employee> staff = new ArrayList<>(100);
```
3. 添加新内容到arraylist
```
staff.add(new Employee("John", ...));
```
4. 返回这个arraylist对象目前的大小
```
staff.size();
```
5. 设置这个i索引位置的value为指定的john，必须先add后才能使用set
```
staff.set(i, john);
```
6. 获得i索引位置的value
```
Employee e = staff.get(i);
```
7. 先创建一个array和目前arraylist大小一样的，把arraylist里的内容填充到array中。
```
var arr = new Employee[list.size()];
list.toArray(arr);
```
8. 移除i索引处的value
```
Employee e = staff.remove(i);
```

# Wrapper and autoboxing
1. Integer本身有个缓存池，大小是-128到127，如果一个int的大小超过了这个，那么JVM会在heap中创建一个新的对象容纳这个过大的数字。

# Abstract
1. 抽象类可以有field,constructor,concrete method,abstract method。但是不能被实例化。

# varargs parameter(变长参数)
1. varargs parameter必须是最后的参数，如果变长参数放在中间，编译器将无法确定变长参数何时结束，以及后续的普通参数从哪里开始。这会导致歧义性。根据规定，当前面的参数有需要被挑走的就会被挑走，剩下的也就是最后的参数会被选中作为varargs parameter。
```
public void printInfo(String category, int... numbers) { 
    // ... 
}
```
2. 一个方法中最多只能有一个变长参数。

# Enum
1. enumerations可以有method。
2. Size在这里是一个class。
```
public enum Size { SMALL, MEDIUM, LARGE, EXTRA_LARGE }
```
3. 所有enumerated types都是Enum类的子类，比如这里的Size就是Enum类的子类。
4. Size.values()，这是Enum的static method,这个方法可以打印Size类中的内容按照从左到右的顺序。
5. enum内部可以为某个特定的常量重写方法逻辑，比如下面就是对EXTRA_LARGE这个对象进行toSrting重写。
```
EXTRA_LARGE {
public String toString() { return "XL"; } // method toString()
};
```