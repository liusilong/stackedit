


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

在Java中，`java.lang.Error` 和 `java.lang.RuntimeException` 下的子类都是属于 unchecked exception

直接属于 `java.lang.Throwable` 下的都属于 checked exception

常见的 **RuntimeException** 有 `ClassCastException`，`IllegalFormatException`， `NullPointerException` 等

常见的 Error 有 `NoSuchFieldError`， ``OutOfMemoryError
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEwMzYxMTM4ODUsODczNjgzNjcwLC05Nj
IwMTU1OCwtNjk4ODQ0MjkxXX0=
-->