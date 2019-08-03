


> Written with [StackEdit](https://stackedit.io/).

# Java 对象创建指南
[原文链接](https://www.baeldung.com/java-initialization)

## 1. Overview
简单的说，在我们可以使用 JVM上的对象之前，这个对象必须已经被初始化了的。在接下来的几个部分中，我们将介绍初始化基本数据类型及 Object 类型的对象的各种初始化方法。

## 2. Declaration vs. Initialization(声明和初始化)
声明是定义一个变量类型及其名字的过程。这我们声明一个 id 变量：

```java
int id;
```

初始化就是为声明的变量分配一个值，如：

```java
id = 1;
```

为了演示，我们创建一个 User 类，在里面声明 name 和 id 属性：

```java
public class User{
  private String name;
  private int id;
}
```

接下来，我们将看到它们初始化时的不同，初始化的方式取决于我们要初始化的字段类型。

## 3. Object vs. Primitives(对象类型和原始类型)
Java 提供了两种数据类型的表示：原始类型和引用类型。在这个部分，我们将讨论二者在初始化方面的差异。

Java 有 8 中内置的数据类型，称为原始数据类型，这种类型的变量直接保存它们的值。

引用类型包含对象（类的实例）的引用。与在分配变量的内存中保存其值的原始类型不同，引用不包含它们引用的对象的值。

相反，引用通过存储对象所在存储器地址指向对象。

请注意，Java 不允许我们去发现内存地址是什么。相反，我们只能使用引用来引用该对象。

让我们看看一个声明并初始化一个引用类型的 User 类的例子：

```java
public void test(){
  User user = new User();
}
```

正如我们所看到的，可以使用关键字 new 来为新引用分配引用，该关键字负责创建新的 User 对象。

让我们继续学习更过的关于对象的创建。

## 5. Creating Objects(创建对象)

不像原始类型，Object 类型的创建有些复杂。这是因为我们不只是将值添加到声明的变量上，相反，我们使用了 new 关键字来触发初始化。这将会调用构造方法并初始化内存中的对象。

让我们更加详细的讨论构造方法和 new 关键字。

**new 关键字负责通过构造函数为新对象分配内存。**

构造方法通常用于初始化表示所创建对象主要属性的实例变量。

如果我们没有明确的提供一个构造方法，编译器将会创建一个默认的无参数的构造方法，这个构造方法仅仅用来为这个对象分配内存。

一个类可以有多个构造方法只要他们的参数集合不同。

Every constructor that doesn’t call another constructor in the same class has a call to its parent constructor whether it was written explicitly or inserted by the compiler through _super()_.
> 每个不在同一个类中调用另一个构造函数的构造函数都会调用其父构造函数，无论它是由显式编写还是由编译器通过super（）编写。

让我们给 User 类加上一个构造方法：

```java
public User(String name, int id) {
  this.name = name;
  this.id = id;
}
```

现在，我们可以使用构造方法去创建一个 User 对象，并且为其属性传入初始值：

```java
User user = new User("Alice", 1);
```


## 6. Variable Scope(变量范围)
在下面的部分中，我们将了解Java中的变量可以存在的不同类型的范围以及它如何影响初始化过程。

### Instance and Class Variables
实例变量和类变量不需要我们初始化它们。一旦我们声明这些变量，就会给他们一个默认值，如下所示：

|  Type| Default Value  |
|--|--|
| boolean | false |
| byte、short、int、long | 0 |
| float、double | 0.0 |
| char | '\u0000' |
| 引用类型 | null |

  
现在，让我们尝试定义一些与实例和类相关的变量，并测试它们是否具有默认值：

```java
public void test(){
  User user = new User();
  assetThat(user.getName()).isNull();
  assetThat(user.getId() == 0);
}
```

### Local Variables (局部变量)
局部变量在使用之前必须初始化，因为它们没有默认值，并且编译器不让我们使用一个没有初始化过的值。

例如，一下代码会生成一个编译错误：

```java
public void print(){
  int i;
  System.out.print(i);
}
```

## 7. The Final keyword

final 关键字用在变量上意味着这个变量的值一旦初始化之后就不能再次改变。通过这种方式，我们可以在 Java 中声明常量。

下面，我们在 User 类中添加一个常量：

```java
private static final int YEAR = 2000;
```

常量要么在声明时初始化，要么在构造方法中初始化。

## 8. Initializers in Java
在 Java 中，初始化程序是一个代码块，它没有关联的名称或数据类型，并且放在任何方法，构造方法或其他代码块之外。

Java 提供了两种类型的初始化器，静态和实例初始化器，然我们看看如何使用他们。

### 8.1 Instance Initializers
我们可以使用他们来初始化实例变量。

为了演示，让我们在 User 类中使用实例化初始化器为 id 赋值：

```java
{
  id = 0;
}
```

### 8.2 Static Initialization Block
一个静态的初始化器或者静态代码块是一个用来初始化静态变量的代码块。

```java
private static String forunm;
static{
  forum = "Java"
}
```

## 9. Order of Initialization(初始化的顺序)
当编写初始化不同类型字段的代码时，我们必须密切关注初始化的顺序。

在 Java 中，初始化语句的执行顺序如下：

- 首先按照顺序执行静态变量和静态代码块
- 接着按照顺序执行实例变量和实例代码块
- 最后执行构造方法


## 10. Object Life Cycle (对象的声明周期)

到目前为止，我们讨论了对象的声明和初始化，让我们继续讨论当对象不再使用之后会发生什么。

不想其他语言一样我们需要担心对象的销毁，Java 通过垃圾收集器处理过时的对象。

Java 中的所有对象都存储在我们程序的堆内存上。事实上，堆表示为我们引用分配的大量未使用的内存池。

另一方面，垃圾收集器是一个Java程序，它通过删除不再可访问的对象来处理自动内存管理。

要是 Java 对象变的无法访问，必须遇到以下情况之一：

- 这个对象不再有任何引用指向它
- 指向该对象的所有引用都超出范围

总之，首先从一个类创建一个对象，通常使用关键字new。然后，该对象将继续其生命，并为我们提供访问其方法和变量的途径。

最后，当它不再需要的时候，la'ji'shou
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEwNTg0NjM4NjksLTIxNDUyMjk1MjksLT
k2ODc4MjA4NywxNTgyNzQ1NjQxLDcyMjkwNjI4OV19
-->