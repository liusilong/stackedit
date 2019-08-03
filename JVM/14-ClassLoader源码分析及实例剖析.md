


> Written with [StackEdit](https://stackedit.io/).

## ClassLoader

ClassLoader 是从 jdk1.0 就已经存在了，该类是一个抽象类，我们不能直接实例化它，需要实现一个子类来继承它。

当我们制定一个 class 的二进制的名字的时候，类加载器就会尝试定位或者生成构成该类定义的数据。一个典型的策略就是将一个二进制的名字转化为一个文件名然在文件系统中读取这个名字对应的 class 文件。

二进制名字实例：
	java.lang.String ： 表示Java中的 String 类
	java.net.URLClassLoader$3$1 : 表示 URLClassLoader 类中的第三个匿名内部类中的第一个匿名内部类

```java
public abstract class ClassLoader {...}
```

以下是数组类的加载信息：

> Class objects for array classes are not created by class loaders, but are created automatically as required by the Java runtime. The class loader for an array class, as returned by [`Class.getClassLoader()`](https://docs.oracle.com/javase/7/docs/api/java/lang/Class.html#getClassLoader()) is the same as the class loader for its element type; if the element type is a primitive type, then the array class has no class loader.

数组类型对应的class对象并不是由类加载器来创建的，是由JVM在运行期间帮助我们创建的。

验证上述描述：

```java
public class Demo1 {
    public static void main(String[] args) {
        String[] strings = new String[2];
        // 获取数组的 Class 类型
        System.out.println(strings.getClass());
        System.out.println("-----");
        // 获取数组的 ClassLoader
        System.out.println(strings.getClass().getClassLoader());

        System.out.println("--------Dividing line---------");

        Demo1[] demo1s = new Demo1[2];
        System.out.println(demo1s.getClass());
        System.out.println("-----");
        System.out.println(demo1s.getClass().getClassLoader());

        System.out.println("--------Dividing line----------");
		//  if the element type is a primitive type, then the array class has no class loader.
        int[] ints = new int[2];
        System.out.println(ints.getClass());
        System.out.println("-----");
        System.out.println(ints.getClass().getClassLoader());
    }
}
```
输出：
```java
class [Ljava.lang.String;
-----
null
--------Dividing line---------
class [Lday14.Demo1;
-----
sun.misc.Launcher$AppClassLoader@18b4aac2
--------Dividing line--------
class [I
-----
null
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTkyNDEwNDYzM119
-->