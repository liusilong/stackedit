


> Written with [StackEdit](https://stackedit.io/).

我们分析字节码的工具为 [Hex Fiend](https://ridiculousfish.com/hexfiend/)
分析的程序如下：
```java
package day37;

public class Demo1 {
    private int a = 1;

    public int getA() {
        return a;
    }

    public void setA(int a) {
        this.a = a;
    }
}
```

使用 `javap -verbose` 命令分析一个字节码文件时，将会分析该字节码文件的魔数、版本号、常量池、类信息、类的构造方法、类中的方法信息、类变量与成员变量等信息。

如我们可以使用 `javap -verbos day37.Demo1` 将上述程序比较完整的字节码信息打印出来。

```java
Classfile /Users/liusilong/Dev/Java/java_workspace/JVM_Learn/out/production/JVM_Learn/day37/Demo1.class
  Last modified 2019-5-4; size 451 bytes
  MD5 checksum 0a6803c63e6e12bf6b47e9fdbe2887d6
  Compiled from "Demo1.java"
public class day37.Demo1
  minor version: 0
  major version: 52
  flags: ACC_PUBLIC, ACC_SUPER
Constant pool:
   #1 = Methodref          #4.#20         // java/lang/Object."<init>":()V
   #2 = Fieldref           #3.#21         // day37/Demo1.a:I
   #3 = Class              #22            // day37/Demo1
   #4 = Class              #23            // java/lang/Object
   #5 = Utf8               a
   #6 = Utf8               I
   #7 = Utf8               <init>
   #8 = Utf8               ()V
   #9 = Utf8               Code
  #10 = Utf8               LineNumberTable
  #11 = Utf8               LocalVariableTable
  #12 = Utf8               this
  #13 = Utf8               Lday37/Demo1;
  #14 = Utf8               getA
  #15 = Utf8               ()I
  #16 = Utf8               setA
  #17 = Utf8               (I)V
  #18 = Utf8               SourceFile
  #19 = Utf8               Demo1.java
  #20 = NameAndType        #7:#8          // "<init>":()V
  #21 = NameAndType        #5:#6          // a:I
  #22 = Utf8               day37/Demo1
  #23 = Utf8               java/lang/Object
{
  public day37.Demo1();
    descriptor: ()V
    flags: ACC_PUBLIC
    Code:
      stack=2, locals=1, args_size=1
         0: aload_0
         1: invokespecial #1                  // Method java/lang/Object."<init>":()V
         4: aload_0
         5: iconst_1
         6: putfield      #2                  // Field a:I
         9: return
      LineNumberTable:
        line 3: 0
        line 5: 4
      LocalVariableTable:
        Start  Length  Slot  Name   Signature
            0      10     0  this   Lday37/Demo1;

  public int getA();
    descriptor: ()I
    flags: ACC_PUBLIC
    Code:
      stack=1, locals=1, args_size=1
         0: aload_0
         1: getfield      #2                  // Field a:I
         4: ireturn
      LineNumberTable:
        line 8: 0
      LocalVariableTable:
        Start  Length  Slot  Name   Signature
            0       5     0  this   Lday37/Demo1;

  public void setA(int);
    descriptor: (I)V
    flags: ACC_PUBLIC
    Code:
      stack=2, locals=2, args_size=2
         0: aload_0
         1: iload_1
         2: putfield      #2                  // Field a:I
         5: return
      LineNumberTable:
        line 12: 0
        line 13: 5
      LocalVariableTable:
        Start  Length  Slot  Name   Signature
            0       6     0  this   Lday37/Demo1;
            0       6     1     a   I
}
SourceFile: "Demo1.java"
```

接着我们使用  [Hex Fiend](https://ridiculousfish.com/hexfiend/) 工具打开 `day37.Demo1` 的 class 文件如下：
我们设置的是单个字节显示 （ Views -> Byte Grouping -> 1 ），下面的内容为**十六进制**

![](https://user-gold-cdn.xitu.io/2019/5/3/16a7e3618893dc41?w=2054&h=580&f=png&s=244446)

#### 魔数
**所有的 .class 字节码文件的前 4 个字节都是魔数**，魔数值为固定值：0xCAFEBABE

CAFEBABE 顾名思义 *cafe babe* 就是 *咖啡宝贝* 的意思，正好 Java 的 logo 就是一个咖啡图案

#### 版本信息
**魔数之后的 4 个字节为版本信息**，前两个字节表示 `minor version`（次版本号），后两个字节表示 `major version` （主版本号）。

图片中前两个字节 0000 转换成 十进制还是 0 ，后两个字节 0034 转换成十进制为 52 （3 * 16 + 4）
在使用 javap -verbose 命令输出的字节码文件中，我们也会看到如下输出：

```shell
  minor version: 0
  major version: 52
``` 
这个 52 表示的是 jdk8，对应的 51 表示 jdk7，50 表示 jdk6， 49 表示 jdk5
所以该文件的版本号为：1.8.0
1.8 为主版本号，0 为次版本号

#### 常量池（constant pool）
**紧接着主版本号之后的就是常量池入口**。一个 Java 类中定义的很多信息都是由常量池来维护和描述的。可以将常量池看作是 Class 文件的资源仓库，比如说 Java 类中定义的方法与变量信息，都是存储在常量池中。常量池中主要存储两类常量：字面量与符号引用。字面量如文本字符串，Java 中声明的 final 的常量值等；而符号引用如类和接口的全局限定名（包名 + 类名），字段的名称和描述符，方法的名称和描述符等。

常量池的总体结构：Java 类所对应的常量池主要由常量池数量与常量池数组（常量表）这两部分共同构成。**常量池数量紧跟在常量主版本后面，占据 2 个字节；常量池数组则紧跟在常量池数量之后。**常量池数组中不同的元素类型、结构都是不同的，长度当然也就不同；但是，每一种元素的第一数据都是一个 u1 类型，该字节是个标志位，占据 1 个字节。JVM 在解析常量池时，会根据这个 u1 类型来获取元素的具体类型。值得注意的是，**常量池数组中元素的个数 = 常量池数 - 1（其中 0 暂时不使用）**，目的是满足某些常量池索引值的数据在特定情况下需要表达**不引用任何一个常量池**的含义；根本原因在于，索引为 0 也是一个常量（保留常量），只不过它不位于常量表中，这个常量就对应 null 值；所以，常量池的索引从 1 而非 0 开始。

在上面的字节码图片中可以看到常量池数为 `00 18` 转换为十进制的就是 24 ，也就是有 24 个常量池数，根据公式 **常量池数组中元素的个数 = 常量池数 - 1（其中 0 暂时不使用）**，字节码中的常量池数量应该是 23 ，这个我们可以使用 `javap -verbos` 这个名利反编译来看，部分如下：

```java
Constant pool:
   #1 = Methodref          #4.#20         // java/lang/Object."<init>":()V
   #2 = Fieldref           #3.#21         // day37/Demo1.a:I
   #3 = Class              #22            // day37/Demo1
   #4 = Class              #23            // java/lang/Object
   #5 = Utf8               a
   #6 = Utf8               I
   #7 = Utf8               <init>
   #8 = Utf8               ()V
   #9 = Utf8               Code
  #10 = Utf8               LineNumberTable
  #11 = Utf8               LocalVariableTable
  #12 = Utf8               this
  #13 = Utf8               Lday37/Demo1;
  #14 = Utf8               getA
  #15 = Utf8               ()I
  #16 = Utf8               setA
  #17 = Utf8               (I)V
  #18 = Utf8               SourceFile
  #19 = Utf8               Demo1.java
  #20 = NameAndType        #7:#8          // "<init>":()V
  #21 = NameAndType        #5:#6          // a:I
  #22 = Utf8               day37/Demo1
  #23 = Utf8               java/lang/Object
```

可以看出，程序中的常量池数量数 23 个。

**Java 字节码表如下：**

![](https://user-gold-cdn.xitu.io/2019/5/4/16a80c41079fb433?w=1310&h=890&f=png&s=1033184)

往下分析之前，先熟悉下进制转换表（这里主要是十六进制和十进制之间的相互转换）

| 十进制 | 二进制 | 八进制 | 十六进制 |
| :----: | :----: | :----: | :------: |
|   8    |  1000  |   10   |    8     |
|   9    |  1001  |   11   |    9     |
|   10   |  1010  |   12   |    A     |
|   11   |  1011  |   13   |    B     |
|   12   |  1100  |   14   |    C     |
|   13   |  1101  |   15   |    D     |
|   14   |  1110  |   16   |    E     |
|   15   |  1111  |   17   |    F     |


如上述字节码中的十六进制转换成十进制就是 ： 
> 0A = 0 * 16 + 10 = 10
> 4C = 4 * 16 + 12 = 76
> 3B = 3 * 16 + 11 = 59

在 JVM规范中，每个变量/字段都有描述信息，描述信息主要的作用是描述字段的数据类型、方法的参数列表（包括数量、类型与顺序）与返回值。根据描述规则，基本数据类型和代表无返回值的 `void` 类型都用一个**大写字符**来表示，对象类型则使用字符 **L** 加对象的**全限定名称**来表示。为了压缩字节码文件的体积，对于基本数据类型，JVM 都只使用一个大写字母来表示，对应关系如下所示：

| 字节码描述信息 | 数据类型 |
| :------------: | :------: |
|       B        |   byte   |
|       C        |   char   |
|       D        |  double  |
|       F        |  float   |
|       I        |   int    |
|       J        |   long   |
|       S        |  short   |
|       Z        | boolean  |
|       V        |   void   |
|       L        | 对象类型 |

如 `String` 对象的描述信息就是 `Ljava/lang/String;`

**对于数组类型来说**，每一个维度使用一个前置的 `[` 来表示 ，如 `int[]` 被记录为 `[I` ，`String[][]`  被记录为 `[[Ljava/lang/String;`

**用描述符描述方法时**，按照**先参数列表**，**后返回值的顺序**来描述。**参数列表按照参数的严格顺序放在一组** `()` **之内**，如方法：`String getRealnameByIdAndNickname(int id, Stirng name)` 的描述符为：

    (ILjava/lang/String;)Ljava/lang/String;

再来一个例子 `public boolean getName(int id, String name, boolean flag)` ，该方法的描述信息如下：

    (ILjava/lang/String;Z)Z

 
 再如：`public void method()` ，该方法的描述信息为： `()V`

注意方法描述符不用带方法名称，方法名称是放在常量池当中的。

JVM 在分析字节码中的常量池的时候怎么分析呢？首先我们知道这个字节码文件中的常量数，接着根据 Java 字节码表（就是上面的第一张图片）里面的规则来分析。

**Java 字节码整体结构：**

![](https://user-gold-cdn.xitu.io/2019/5/4/16a814f39396fe0a?w=1638&h=1164&f=png&s=206273)


|      类型      |               名称                |          数量           |
| :------------: | :-------------------------------: | :---------------------: |
| u4 (4 个字节)  |            magic(魔数)            |            1            |
| u2 (2 个字节)  |     minor_version (次版本号)      |            1            |
| u2 (2 个字节)  |      major_version (主版本号)      |            1            |
| u2 (2 个字节)  |  constant_pool_count (常量个数)   |            1            |
|    cp_info     |     Constant_pool (常量池表)      | constant_pool_count - 1 |
| u2 (2 个字节)  |  access_flags (类的访问控制权限)  |            1            |
| u2 (2 个字节)  |         this_class (类名)         |            1            |
| u2 (2 个字节)  |       super_class (父类名)        |            1            |
| u2 (2 个字节)  |    interfaces_count (接口个数)    |            1            |
| u2 (2 个字节)  |        interfaces (接口名)        |    interfaces_count     |
| u2 (2 个字节)  |       fields_count (域个数)       |            1            |
|   field_info   |          fields (域的表)          |      fields_count       |
| u2 (2 个字节)  |    methods_count (方法的个数)     |            1            |
|  method_info   |         methods (方法表)          |      methods_count      |
| u2 (2 个字节)  | attributes_count (附加属性的个数) |            1            |
| attribute_info |     attributes (附加属性的表)     |    attributes_count     |


![](https://user-gold-cdn.xitu.io/2019/5/5/16a86b77e0345fb8?w=1112&h=580&f=png&s=219508)


Class 字节码中有两种数据类型：
- 字节数据直接量：这是基本的数据类型。共细分为 `u1`, `u2`, `u4`, `u8` 四种，分别代表**连续的** 1 个字节，2 个字节，4 个字节，8 个字节组成的整体数据。
- 表（数组）：表是由多个基本数据或其他表，按照既定顺序组成的大的数据集合。表是有结构的，它的结构体现在：组成表的成分所在的位置和顺序都是已经严格定义好的。


#### Access Flag （访问标志）

访问标志信息包括该 Class 文件是类还是接口，是否被定义成 public，是否是 abstract，如果是类，是否被声明成 final。通过上面的源代码，我们知道该文件是类并且是 public。  

![](https://user-gold-cdn.xitu.io/2019/5/5/16a86c79afe93b16?w=896&h=538&f=png&s=402083)

在我们案例中的字节码中，**access_flags** 为 `00 21` ，其实 `0x0021` 是 `0x0020` 和 `0x0001` 的并集，表示 **ACC_PUBLIC** 与 **ACC_SUPER** 。这些信息在上面使用 `javap -verbos` 命令编译出来的内容里面也有所体现：

    flags: ACC_PUBLIC, ACC_SUPER

#### This class name （当前类的名字）
在我们案例中的字节码中，**this_class** 的值为 `00 03` ，这表示一个 index，位于常量池值得 index，当前的 index 为 03，这就会定位到常量池中的如些信息：

```java
 #3 = Class              #22            // day37/Demo1
```

即当前类的完整名称。

#### Super class name （父类的名字）
在我们案例中的字节码中，**super_class** 的值为 `00 04`，同样的，常量池中的 04 位置如下：

```java
#4 = Class              #23            // java/lang/Object
```

即当前类的父类为 Object 类。

#### Interface count （接口数量）
在我们案例中的字节码中，**interfaces_count** 的值为 `00 00`，表示当前类没有实现任何接口。

#### Interfaces （接口表）
由于当前类没有实现任何接口，即 `interfaces_count = 0`，所以在字节码中就不会出现 Interfaces （接口表）的信息。

#### Fields Count  （成员变量数量）
 字段表用于描述类和接口中声明的变量。这里的字段包含了类级别变量以及实力变量，但是不包括方法内部声明的局部变量。
 
在我们案例中的字节码中，**fields_count** 的值为 `00 01`，表示当前类存在一个成员变量。我们实例类中的唯一一个成员变量是 `a`

#### Fields （字段表）

字段表结构如下:

|      类型      |       名称       |       数量       |
| :------------: | :--------------: | :--------------: |
|       u2       |   access_flags   |        1         |
|       u2       |    name_index    |        1         |
|       u2       | descriptor_index |        1         |
|       u2       | attributes_count |        1         |
| attribute_info |    attributes    | attributes_count |

**access_flags**:
在我们案例中的字节码中，**access_flags** 对应的值为 `00 02` ，这代表的访问修饰符为 `private` ，在上述的 **Access Flag** 表中没有列出来。体现在实例程序中的代码就是 

```java
private int a = 1;
```
**name_index**:
在我们案例中的字节码中，**name_index** 对应的值为 `00 05` ，也就是常量池中索引为 05 的值 ：

```java
 #5 = Utf8               a
```

**descriptor_index**:

在我们案例中的字节码中，**descriptor_index** 对应的值为 `00 06` ，也就是常量池中索引为 06 的值 ：

```java
#6 = Utf8               I
```

综合上面三个属性的值，可以得出当前变量的访问修饰符为 `private`，名称为 `a` 描述信息为 `I`，也就是一个**私有的整型变量**。

**attributes_count**：
在我们案例中的字节码中，**attributes_count** 对应的值为 `00 00` ，也就是当前变量没有任何属性：

**attribute_info**：
由于 `attributes_count = 0`，所以 **attribute_info** 在字节码中就不会出现。


#### Lesson 42
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTUzNjgyODI3N119
-->