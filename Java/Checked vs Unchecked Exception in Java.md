


> Written with [StackEdit](https://stackedit.io/).
## Checked vs Unchecked Exceptions in Java

在Java中，有两种类型的异常：

### Checked Exceptions
Checked	Exceptions 会在程序编译期检查的异常。如果方法中的某些代码抛出 Checked Exception，则该方法必须处理异常或者使用 throws 关键字指定异常。

举例来说，考虑如下代码，代码的意思是打开 "c:\test\a.txt"位置上的文件然后输出该文件的前三行。这个程序不能编译，因为 main()  方法使用了 FileReader() 而 FileReader() 会抛出一个 checked exception 为 FileNotFoundException。程序中还使用了 readLine() 和 close() 方法，并且这些方法也会抛出 checked exception 为 IOException。

```java 
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTIwMjE1OTIyNCwtOTYyMDE1NTgsLTY5OD
g0NDI5MV19
-->