


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

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEyMTI4OTUzNzEsLTc4OTAyNjAzMV19
-->