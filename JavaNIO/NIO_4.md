


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

* 只读 Buffer 就是只能执行读取操作，不能执行改写的 Buffer
* 通过调用原有 Buffer（读\写）的 `asReadOnlyBuffer()` 方法将在原有 Buffer 的基础上创建一个新的只读 Buffer
* 新的只读 Buffer 的 `capacity`、`limit`、`position`、`mark` 和原有 Buffer 一致，但是它们两个的 `capacity`、`limit`、`position`、`mark`  又都是独立的
* 新 Buffer 的内容和原有 Buffer 一致，对原有 Buffer 的修改对于新 Buffer 是可见的

下面我们来看一个只读 Buffer 的例子

```java
public static void main(String[] args) {  
    ByteBuffer buffer = ByteBuffer.allocate(10);  
  
  System.out.println(buffer.getClass());  
  
 for (int i = 0; i < buffer.capacity(); i++) {  
        buffer.put((byte) i);  
  }  
  
    // 在原有 Buffer 的基础上创建一个只读 Buffer  
  ByteBuffer readOnlyBuffer = buffer.asReadOnlyBuffer();  
  
  System.out.println(readOnlyBuffer.getClass());  
  
}
```

首先我们创建了一个可读写的 Buffer，然后往里面添加了一些元素

之后调用了 `asReadOnlyBuffer()`  方法生成一个 readOnlyBuffer

我们在上述代码中分别输出了 buffer 的 class 对象和 readOnlyBuffer 的 class 对象，他们的结果如下：

```java
class java.nio.HeapByteBuffer
class java.nio.HeapByteBufferR
```

可以看出：可读写的 Buffer 的 class 对象是 `HeapByteBuffer` ，而只读 Buffer 的 class 对象是 `HeapByteBufferR`

然后我们看看 `HeapByteBuffer` 的 `asReadOnlyBuffer()` 方法，如下：

```java
public ByteBuffer asReadOnlyBuffer() {  
  
    return new HeapByteBufferR(hb,  
		 this.markValue(),  
		 this.position(),  
		 this.limit(),  
		 this.capacity(),  
		  offset);  
}
```

可以看出在 `asReadOnlyBuffer()` 里面是直接 new 出了一个 `HeapByteBufferR` 对象。

那么，`HeapByteBufferR`  是如何保证只读的呢？我们来看看 `HeapByteBuffer`  和 `HeapByteBufferR`  的 `put` 方法就明白了：
**HeapByteBuffer**

<!--stackedit_data:
eyJoaXN0b3J5IjpbNTcyNDU0NzMzLDMwMzQ2MzE0Myw2MTk0NT
cyNzAsLTE2OTc0NTEzNjQsLTEzOTQ2NDY5NzAsLTExODk2NDY4
ODcsNTgyMzE3Mzk3LDIxMjE1Njk2MjksMzc3MjcxMzk1LC03OD
kwMjYwMzFdfQ==
-->