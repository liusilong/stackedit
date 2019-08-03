


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

为了演示，我们创建一个 User 类，在里面声明 name 和 id

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTk2NTc3NTM0N119
-->