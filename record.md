## Java 8 forEach（）
Java forEach() 方法是一种效用函数来遍历集合，例如（list，set或map）和stream。它用于对集合的每个元素执行给定的操作。

该forEach()方法已添加到以下位置：

- Iterable接口–这使Iterable.forEach()方法可用于所有集合类，除了Map
- Map接口–这使forEach()操作可用于所有Map类。
- Stream接口–这使得所有类型的流都可以使用它forEach()和进行forEachOrdered()操作。

### 1.Iterable forEach()
#### 1.1 forEach()方法
给定的代码片段显示了Iterable接口中forEach()方法的默认实现。

在内部，它使用增强的for循环。因此，使用新的for循环将提供与方法相同的效果和性能forEach()。

```java
Iterable.java
default void forEach(Consumer<? super T> action) 
{
    Objects.requireNonNull(action);
    for (T t : this) {
        action.accept(t);
    }
}
```
该forEach()方法action对中的每个元素执行给定的操作，Iterable直到所有元素都已处理或action引发异常。

**示例1：Java程序使用forEach（）遍历List**
>使用forEach（）方法
```java
List<String> names = Arrays.asList("Alex", "Brian", "Charles");

names.forEach(System.out::println);
```
> 程序输出：
```
Alex
Brian
Charles
```

1.2 创造consumer action
在下面的示例中，action表示接受单个输入参数且不返回结果的操作。它是Consumer接口的一个实例。

通过创建这样的消费者动作，我们可以指定多个语句以类似于方法的语法执行。

创造消费者行动
```java
List<String> names = Arrays.asList("Alex", "Brian", "Charles");
 
Consumer<String> makeUpperCase = new Consumer<String>()
{
    @Override
    public void accept(String t) 
    {
        System.out.println(t.toUpperCase());
    }
};
 
names.forEach(makeUpperCase);   
```
> 程序输出：
```
ALEX
BRIAN
CHARLES
```
### 2. Map forEach()
#### 2.1 forEach() 方法
该方法遍历Map的每一个Entry并传递执行BiConsumer，直到所有entries都已经被处理或抛出异常。

> Map.java
```java
default void forEach(BiConsumer<? super K, ? super V> action) {
    Objects.requireNonNull(action);
    for (Map.Entry<K, V> entry : entrySet()) {
        K k;
        V v;
        try {
            k = entry.getKey();
            v = entry.getValue();
        } catch(IllegalStateException ise) {
            // this usually means the entry is no longer in the map.
            throw new ConcurrentModificationException(ise);
        }
        action.accept(k, v);
    }
}
```
**示例2：使用forEach（）遍历Map的Java程序**
使用Map.forEach（）方法
```java
Map<String, String> map = new HashMap<String, String>();
 
map.put("A", "Alex");
map.put("B", "Brian");
map.put("C", "Charles");
 
map.forEach((k, v) -> 
    System.out.println("Key = " + k + ", Value = " + v));
```
> 程序输出：
```
Key = A, Value = Alex
Key = B, Value = Brian
Key = C, Value = Charles
```

我们还可以创建一个自定义的BiConsumer操作，该操作将从中获取键值对Map并一次处理每个条目。

创建自定义BiConsumer
```java
BiConsumer<String, Integer> action = (a, b) -> 
{ 
    //Process the entry here as per business
    System.out.println("Key is : " + a); 
    System.out.println("Value is : " + b); 
}; 
 
Map<String, Integer> map = new HashMap<>();
     
map.put("A", 1);
map.put("B", 2);
map.put("C", 3);
 
map.forEach(action);
```
> 程序输出
```
Key is : A
Value is : 1
 
Key is : B
Value is : 2
 
Key is : C
Value is : 3
```
3.Stream forEach（）和forEachOrdered（）
在Stream，forEach()并且forEachOrdered()是终端的操作。

与Iterable类似，stream forEach()方法对Stream的每个元素执行一个动作。

对于顺序Stream，元素的顺序（迭代期间）与流源中的顺序相同，因此无论我们使用forEach()还是，输出都将相同forEachOrdered()。

在使用并行流时，forEachOrdered()如果元素的顺序在迭代过程中很重要，则使用。forEach()该方法不能保证元素顺序提供并行性的优点。

**示例3：Java forEach（）示例在Stream上进行迭代**
在此示例中，我们将打印数字流中的所有偶数。

Java 8 forEach over流元素
```java
List<Integer> numberList = Arrays.asList(1,2,3,4,5);
     
Consumer<Integer> action = System.out::println;
 
numberList.stream()
    .filter(n -> n%2  == 0)
    .forEach( action );
```
> 程序输出
```
2
4
```
示例4：Java forEachOrdered（）示例以在Stream上进行迭代
在此示例中，我们将打印数字流中的所有偶数。

Java 8 forEachOrdered流元素
```
List<Integer> numberList = Arrays.asList(1,2,3,4,5);
     
Consumer<Integer> action = System.out::println;
 
numberList.stream()
    .filter(n -> n%2  == 0)
    .parallel()
    .forEachOrdered( action );
```
> 程序输出
```
2
4
```
