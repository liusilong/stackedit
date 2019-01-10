


> Written with [StackEdit](https://stackedit.io/).
## Checked vs Unchecked Exceptions in Java

在Java中，有两种类型的异常：

### Checked Exceptions
Checked	Exceptions 会在程序编译期检查的异常。如果方法中的某些代码抛出 Checked Exception，则该方法必须处理异常或者使用 throws 关键字指定异常。

举例来说，考虑如下代码，代码的意思是打开 "c:\test\a.txt"位置上的文件然后输出该文件的前三行。这个程序不能编译，因为 main()  方法使用了 FileReader() 而 FileReader() 会抛出一个 checked exception 为 FileNotFoundException。程序中还使用了 readLine() 和 close() 方法，并且这些方法也会抛出 checked exception 为 IOException。

```java
import java.io.BufferedReader;  
import java.io.FileReader;  
public class Demo1 {  
    public static void main(String[] args) {  
        // throw java.io.FileNotFoundException  
		FileReader file = new FileReader("/home/liusilong/workspace/test.txt");  
        BufferedReader fileInput = new BufferedReader(file);  
  
        for (int counter = 0; counter < 3; counter++) {  
            // throw java.io.IOException  
			System.out.println(fileInput.readLine());  
        }  
        // throw java.io.IOException  
	    fileInput.close();  
    }  
}
``` 

上面的程序是编译不通过的。

要修复上述的程序，我们可以使用 throws 来抛出指定的异常，或者使用 try-catch 代码块开捕获异常。

在下面的程序中，我们使用 throws 开捕获异常，由于 FileNotException 是 IOException 的子类，所以我们只需指定 IOException 就可以使上述程序没有在编译器的时候没有错误。


```java
import java.io.BufferedReader;  
import java.io.FileReader;  
import java.io.IOException;  
  
public class Demo1 {  
    public static void main(String[] args) throws IOException {  // changed 
        // throw java.io.FileNotFoundException  
        FileReader file = new FileReader("/home/liusilong/workspace/test.txt");  
        BufferedReader fileInput = new BufferedReader(file);  
  
        for (int counter = 0; counter < 3; counter++) {  
            // throw java.io.IOException  
            System.out.println(fileInput.readLine());  
        }  
        // throw java.io.IOException  
       fileInput.close();  
    }  
}
```

Output:

```java
hello world
null
null
```

### Unchecked Exception
Unchecked Exceptions 在程序的编译期间是不会检查的。在 C++ 中，所有的异常都是 unchecked 的，因此编译器不会强制处理或者指定异常。程序员应该自己指定和捕获异常。

在Java中，`java.lang.Error` 和 `java.lang.RuntimeException` 下的子类都是属于 **unchecked exception**

直接属于 `java.lang.Throwable` 下的都属于 **checked exception**

### 举例

#### Checked Exception
常见的 **Throwable** 有 `FileNotFoundException`

#### UnChecked Exception
常见的 **RuntimeException** 有 `ClassCastException`，`IllegalFormatException`， `NullPointerException` 等

常见的 **Error** 有 `NoSuchFieldError`， `OutOfMemoryError`， `StackOverflowError`


看看下面的例子，在编译器的时候是通过的，但是它在运行的时候会抛出 ArithmeticException，编译器允许它编译，因为  ArithmeticException 是一个 unchecked exception

```java
public class Demo2 {  
    public static void main(String[] args) {  
        int x = 0;  
        int y = 10;  
        int z = y / x;  
    }  
}
```

Output

```java
Exception in thread "main" java.lang.ArithmeticException: / by zero
	at com.lsl.exception.Demo2.main(Demo2.java:7)
```
我们可以使用 try-catch 代码块来捕获这个异常，如下：

```java
public class Demo2 {  
    public static void main(String[] args) {  
        try{  
            int x = 0;  
            int y = 10;  
            int z = y / x;  
        }catch (ArithmeticException e){  
            System.out.println("exception message: "+ e.getMessage());  
        }  
    }  
}
```

Output

```java
exception message: / by zero 
```

### Why two types?

See [Unchecked Exceptions — The Controversy](http://docs.oracle.com/javase/tutorial/essential/exceptions/runtime.html) for details.

### Should we make our exceptions checked or unchecked?
Following is the bottom line from [Java documents](http://docs.oracle.com/javase/tutorial/essential/exceptions/runtime.html)
 *如果客户端可以从异常中恢复，则可以将其设置为 checked exception，反之，如果程序无法从异常中恢复，则应该将其设置为 unchecked exception*




```java
                   +-----------+
           | Throwable |
                   +-----------+
                    /         \
           /           \
          +-------+          +-----------+
          | Error |          | Exception |
          +-------+          +-----------+
       /  |  \           / |        \
         \________/      \______/         \
                            +------------------+
    unchecked     checked    | RuntimeException |
                    +------------------+
                      /   |    |      \
                     \_________________/
                       
                       unchecked
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTIxMjU3MzIzODgsLTYyODMzMDU4MCwyMT
E1OTA5NTYyLC0xNzg1NDA3MTIsODczNjgzNjcwLC05NjIwMTU1
OCwtNjk4ODQ0MjkxXX0=
-->