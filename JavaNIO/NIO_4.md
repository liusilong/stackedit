


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
public static void main(String[] args) {  
    ByteBuffer buffer = ByteBuffer.allocate(10);  
  
 for (int i = 0; i < buffer.capacity(); i++) {  
        buffer.put((byte) i);  
  }  
  
    // 指定 buffer 的 position 和 limit  
  buffer.position(2);  
  buffer.limit(6);  
  
  // 从原有 buffer 中获取 slice buffer 这里是 [2, 6} 左闭右开区间  
  ByteBuffer sliceBuffer = buffer.slice();  
  
  // 将 slice buffer 中的元素都 乘以 2  
  for (int i = 0; i < sliceBuffer.capacity(); i++) {  
        byte b = sliceBuffer.get(i);  
	  b *= 2;  
	  sliceBuffer.put(i, b);  
	}  
  
    // 重置 buffer 的状态  
  buffer.position(0);  
  buffer.limit(buffer.capacity());  
  
  // 从原有 buffer 中读取数据  
  while (buffer.hasRemaining()) {  
        byte b = buffer.get();  
  System.out.print(b + "\t");  
  }  
  
}
```
输出如下：

```java
0	1	4	6	8	10	6	7	8	9
```
我们可以看到，从第二个到第六个（不包含第六个）的元素都已经乘以了2


### 只读 Buffer

z
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE2ODg4ODE0MTAsLTE2OTc0NTEzNjQsLT
EzOTQ2NDY5NzAsLTExODk2NDY4ODcsNTgyMzE3Mzk3LDIxMjE1
Njk2MjksMzc3MjcxMzk1LC03ODkwMjYwMzFdfQ==
-->