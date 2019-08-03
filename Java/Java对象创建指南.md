


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







<!--stackedit_data:
eyJoaXN0b3J5IjpbMzA1MTQ5NzY0XX0=
-->