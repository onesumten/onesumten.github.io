# Interface
1. interface意味着能做什么，也就是can-do。
2. 所有在interface中的method都必须声明为public，如果不是那就是default，并且无法正确被重写因为default意思是访问权限只允许在同一个package内。
3. compareTo是一个abstract,任何继承了Comparable的类必须提供compareTo方法同时传入参数只能是Object，并且返回是int。在java5之后，
Comparable接口被定义为Comparable<T>。
```
public interface Comparable {
    int compareTo(Object other);
}
```
4. Arrays.sort()方法可以对object的array进行分类，从小打大，如果想对这个object的array使用这个sort方法那么这个object的类必须implement Comparable接口。
5. Interface可以拥有constant，当你写一个：
```
int VALUE = 10;
```
   * 这里VALUE会被定性为public static final，属于这个interface，同时不能再被修改了。
6. 在JAVA8之后，你可以通过添加default关键字来让写出一个带有implementation的method，这个method可以被子类直接使用,可以被重写。
7. 可以写static method，也就是直接使用**接口名字.方法**。这个static method不能被重写。
8. 也可以写private helper method去帮助接口内部的方法实现，不暴露给外部。
9. 如果一个x.comparaTo(y) == 0,那么x.equals(y)必须是true。
10. 如果Manager extends Employee, Employee implements Comparable<Employee>,那么实际上Manager implements Comparabale<Emplyee>而不是Comparable<Manager>.
11. 如果subclass定义了不同的比较规则，那么禁止不同类之间比较。如果有相同的比较规则，那么就实现一个final compareTo方法在superclass。
12. 如果一个class继承了一个superclass和一个interface，那么superclass的default method会被使用，interface的同名default method会被忽略。
13. 如果一个class实现了两个interface，且两个interface有相同的method，那么class必须要重写这个方法，否则会发生compliation error。

# ActionListener interface
1. 当一个class implements这个接口的时候，必须在这个class实现一个actionPerformed方法用来显示当botton被按下的时候会发生什么。
```
class TimePrinter implements ActionListener {
    public void actionPerformed(ActionEvent event) {
        System.out.println("At the tone, the time is "
            + Instant.ofEpochMilli(event.getWhen()));
        Toolkit.getDefaultToolkit().beep();
    }
}
```
2. 随后创建一个对象：
```
var listener = new TimePrinter();
Timer t = new Timer(1000, listener);
t.start();
```
# Comparator interface
1. 使用这个接口可以同时写两种不同的比较方法在一个compare方法里。比如：
```
// 需求：按年龄升序排，如果年龄相同，按成绩降序排
Comparator<Student> complexComparator = new Comparator<Student>() {
    @Override
    public int compare(Student s1, Student s2) {
        // 规则 1: 年龄升序
        if (s1.getAge() != s2.getAge()) {
            return s1.getAge() - s2.getAge(); // 负数在前（小的在前）
        }
        // 规则 2: 年龄相同时，成绩降序
        // 注意：double比较建议用 Double.compare
        return Double.compare(s2.getScore(), s1.getScore()); // s2和s1调换位置实现降序
    }
};
//下面是使用方法，第一个参数是想要比较的内容，第二个参数是规则。
Collections.sort(studentList, complexComparator);
```
1. reflexivity（自反性）
   * 如果两个object是equal的，那么comapre(x,y)必须返回0。
2. 如果compare(x,y)是positive,那么compare(y,x)必须是negative。

# Cloneable interface
1. 这个接口称为标记接口，只有一个类在implements这个接口后才能表示拥有clone的能力，也就可以重写clone方法，并且一定要public方便在其他类中使用。
2. Objetc.clone()是protected，而protected的规则是子类只能在子类内部使用父类的protected方法。如果一个在一个Main类中，Employee类的对象使用clone方法是会产生compliation error。
3. 重写的clone方法需要实现deep copy,也就是说先shallow copy，然后再copy内容来达到deep copy。
```
public class Employee implements Cloneable {
    private String name;
    private Date hireDay;

    @Override
    public Employee clone() throws CloneNotSupportedException {
        // 1. 先执行默认的浅拷贝
        Employee cloned = (Employee) super.clone();
        
        // 2. 为了防止共享 Date 对象，需要手动克隆可变对象（实现深拷贝）
        // cloned.hireDay = (Date) hireDay.clone(); 
        
        return cloned;
    }
}
```

# Lambda expression
1. lambda必须对应一个functional interface which means it has one abstract method，但可以有多个default和static方法。
2. 必须满足三个一致：参数数量一致；参数类型一致；返回类型一致。
3. 如果lambda表达式的方法没有参数，比如roll()，那么仍然需要一个空号“()”，比如：
```
() -> { return 1 + (int)(Math.random() * 6); }
```
4. 如果只有一个parameter且这个parameter可以被左边的进行推断，那么这个parameter可以不需要写类型，因为complier可以从左边的内容推断。
5. 在lambda expression右边你不需要加上大括号，如果你加上大括号你就需要加上return，如果没有大括号就不能用return。
```
//不加大括号的
(x) -> x * 2
//加大括号的
(x) -> { return x * 2; }
```
6. 一个functional interface中的abstract method和java.lang.Objects类中的方法名字一样，那么这些方法不会被计入functional interface的abstract method。
```
@FunctionalInterface
public interface MyInterface {
    // 真正的抽象方法（计数 +1）
    void execute();

    // 以下方法虽然是抽象的，但覆盖了 Object 的方法（计数 +0）
    boolean equals(Object obj);
    String toString();
    int hashCode();
}
```
7. lambda expression可以访问的四种变量
    * 自己的参数 (its own parameters): 比如代码中的 x。
    * 外层方法的局部变量 (local variables from the enclosing method): 比如代码中的 factor。
    * 实例变量 (instance variables): 属于类的成员变量。
    * 静态变量 (static variables): 属于类的静态成员。
```
int factor = 2;
Function<Integer, Integer> multiply = x -> x * factor;
```
8. 在lambda expression内部，不能对local variable进行修改。因为local variable存储在stack中，当这个方法结束后，这个local variable将会消失；而lambda存储在heap中。
9. instance and static variable可以被修改因为这两个内容存储在heap中。


# Function<T, R>
1. R apply(T t)，这个apply方法的目的是激活的作用，如果我们有一个lambda对象是add，那么通过add.apply(x)来激活add对象。
2. default andThen(), default compose()。这里addThen()方法的作用是承接两个lambda对象的具体实现。
3. static identity()

# BiFunction<T,U,R>
1. R apply(T t, U u)
2. default andThen(Function)
3. static minBy(Comparator), static maxBy(Comparator)

# Predicator<T,R>(测试内容，返回true or false)
1. boolean test(T t)
2. default and(Predicate), default or(Predicate), default negate()
3. static not(Predicate), static isEqual(Object)

# Consumer<T>传入一个参数但是不返回内容，用来对数据进行操作。
1. void accept(T t)
2. default andThen(Consumer)

# Supply<T>无传入参数，返回一个内容
1. T get()

# UnaryOperator<T>传入参数的类型是什么，返回参数的类型就必须是什么。Function<T,T>
1. T apply(T t)
# BinaryOperator<T>跟UnaryOperator<T>类似，不过是传入两个参数。BiFunction<T,T,T>

# Method reference
1. 有三种写法：
   * ClassName::staticMethod 
   * ClassName::instanceMethod
   * objectReference::instanceMethod

# Inner class
1. 内部类可以访问outer class的field和local variable（必须是不可变的），这个outer class是相对于这个inner class的外一层的class。

# Anonymous inner class
1. anonymous inner class不需要class名字，用来简化代码量。这里的ActionListerner是一个接口，创建了一个实现该接口的匿名子类实例。
```
public class MyTimerClass {
    
    // 这是一个成员方法
    public void start(int interval, boolean beep) {
        
        // 1. 定义匿名内部类（并立刻实例化）
        ActionListener listener = new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent event) {
                System.out.println("Time is " + Instant.ofEpochMilli(event.getWhen()));
                if (beep) {
                    Toolkit.getDefaultToolkit().beep();
                }
            }
        };

        // 2. 使用这个实例
        Timer timer = new Timer(interval, listener);
        timer.start();
    }
}
```
1. 可以被定义在方法内部也可以在类内部作为一个field提供给这个类中其他方法进行使用
```
public class MyClass {
    // 这是一个在类内部、方法外部定义的匿名内部类
    // 它本质上是一个“成员变量”
    private Comparator<String> stringComparator = new Comparator<String>() {
        @Override
        public int compare(String s1, String s2) {
            return s1.length() - s2.length();
        }
    };

    public void useComparator() {
        // 在这里可以使用上面定义好的 stringComparator
    }
}
```

