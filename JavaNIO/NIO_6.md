


> Written with [StackEdit](https://stackedit.io/).
## MappedByteBuffer

### 内存映射文件
通过MappedByteBuffer 可以将文件的某一部分或者全部映射到内存中，这样我们在写程序的时候只需要和内存进行数据交互，而不需要和磁盘交互，可以提升读写速递。

我们可以通过Channel 的 map方法来获取一个 MappedByteBuffer ， 案例如下：

```java
public class NIOTest6 {  
    public static void main(String[] args) throws Exception{
	    // 第二个参数为 RandomAccessFile 的可读写权限
        RandomAccessFile randomAccessFile = new RandomAccessFile("NioTest6.txt", "rw");  
        // 获取 Channel
        FileChannel fileChannel = randomAccessFile.getChannel();  
        // 获取 MappedByteBuffer
        MappedByteBuffer mappedByteBuffer = fileChannel.map(FileChannel.MapMode.READ_WRITE, 0, 5);  
		// 通过 mappedByteBuffer 修改数据
        mappedByteBuffer.put(0, (byte) 'a');  
        mappedByteBuffer.put(3, (byte) 'b');  
		// 关闭 RandomAccessFile
        randomAccessFile.close();  
    }  
}
```

 `fileChannel.map` 的三个参数的含义分别如下：
 -  **MapMode**： 
	 - READ_ONLY ： 只读
	 - READ_WRITE： 可读可写
	 
  - **position**: 起始位置
  
  - **size**: 长度

上述案例中 的 `NioTest6.txt` 文件中的初始内容为
```txt
000000000000000
```
程序运行之后的内容为
```txt
a00b00000000000
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTU5NzgwNjc5Niw2MjYwOTQ4OTZdfQ==
-->