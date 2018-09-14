


> Written with [StackEdit](https://stackedit.io/).

## Buffer 深入详解

### 通过 NIO 读取文件涉及到 3 个步骤
1. 从 FileInputStream 获取到 FileChannel 对象
2. 创建 Buffer
3. 将数据从 Channel 读取到 Buffer 中

###  绝对方法与相对方法的含义
1. 相对方法：limit 值与 position 值会在操作时被考虑到
2. 绝对方法：完全忽略掉 limit 值与 position 值

### 案例
> 使用 Buffer 将一个文件中的内容写入到另一个文件中去

```java
FileInputStream inputStream = new FileInputStream("input.txt");  
FileOutputStream outputStream = new FileOutputStream("output.txt");  
  
FileChannel inputChannel = inputStream.getChannel();  
FileChannel outputChannel = outputStream.getChannel();  
  
ByteBuffer buffer = ByteBuffer.allocate(4);  
  
while (true) {  
  // Buffer 重置  
  buffer.clear();  
  int read = inputChannel.read(buffer);  
  System.out.println("read: " + read);  
  
 if (read == -1) {  
        break;  
  }  
  
  buffer.flip();  
  outputChannel.write(buffer);  
}  
  
inputChannel.close();  
outputChannel.close();
```

### 类型化的 Buffer
> ByteBuffer 里面不仅可以放置 byte 类型的数据，还可以放置其他类型的数据
> 在下面的例子中，我们依次在 ByteBuffer 中放置几种类型，然后再**依次**取出来。取出顺序和放置顺序一定要一致

```java
public class Demo5 {  
    public static void main(String[] args) {  
      ByteBuffer buffer = ByteBuffer.allocate(64);  
  
	  buffer.putInt(10);  
	  buffer.putLong(50000000L);  
	  buffer.putDouble(3.1415926);  
	  buffer.putChar('刘');  
	  
	  
	  buffer.flip();  
	  
	 int anInt = buffer.getInt();  
	 long aLong = buffer.getLong();  
	 double aDouble = buffer.getDouble();  
	 char aChar = buffer.getChar();  
	  
	  System.out.println(anInt); 
	  System.out.println(aLong); 
	  System.out.println(aDouble); 
	  System.out.println(aChar); 
  
  }  
}
```

输出如下：

```java
10
50000000
3.1415926
刘
```

### Slice Buffer

> Slice Buffer 与 原有 Buffer 共享相同的底层数组

```java

```



<!--stackedit_data:
eyJoaXN0b3J5IjpbLTExODk2NDY4ODcsNTgyMzE3Mzk3LDIxMj
E1Njk2MjksMzc3MjcxMzk1LC03ODkwMjYwMzFdfQ==
-->