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
这里VALUE会被定性为public static final，属于这个interface，同时不能再被修改了。
6. 在JAVA8之后，你可以通过添加default关键字来让写出一个带有implementation的method，这个method可以被子类直接使用,可以被重写。
7. 可以写static method，也就是直接使用**接口名字.方法**。这个static method不能被重写。
8. 也可以写private helper method去帮助接口内部的方法实现，不暴露给外部。
9. 如果一个x.comparaTo(y) == 0,那么x.equals(y)必须是true。
10. 如果Manager extends Employee, Employee implements Comparable<Employee>,那么实际上Manager implements Comparabale<Emplyee>而不是Comparable<Manager>.
11. 如果subclass定义了不同的比较规则，那么禁止不同类之间比较。如果有相同的比较规则，那么就实现一个final compareTo方法在superclass。
12. 如果一个class继承了一个superclass和一个interface，那么superclass的default method会被使用，interface的同名default method会被忽略。
13. 如果一个class实现了两个interface，且两个interface有相同的method，那么class必须要重写这个方法，否则会发生compliation error。
