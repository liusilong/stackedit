


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
        FileReader file = new FileReader("/home/xxx/test.txt");  
        BufferedReader fileInput = new BufferedReader(file);  
  
        for (int counter = 0; counter < 3; counter++) {  
            System.out.println(fileInput.readLine());  
        }  
          
        fileInput.close();  
    }  
}
``` 

上面的程序是编译不通过的。

要修复上述的程序，我们可以使用 throws 来paoc
<!--stackedit_data:
eyJoaXN0b3J5IjpbNjI1MjQ2NjAsLTk2MjAxNTU4LC02OTg4ND
QyOTFdfQ==
-->