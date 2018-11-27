


> Written with [StackEdit](https://stackedit.io/).
## MappedByteBuffer

### 内存映射文件
通过MappedByteBuffer 可以将文件的某一部分或者全部映射到内存中，这样我们在写程序的时候只需要和内存进行数据交互，而不需要和磁盘交互，可以提升读写速递。

我们可以通过Channel 的 map方法来获取一个 MappedByteBuffer ， 案例如下：

```java
public class NIOTest6 {  
    public static void main(String[] args) throws Exception{
	    // 
        RandomAccessFile randomAccessFile = new RandomAccessFile("NioTest6.txt", "rw");  
        FileChannel fileChannel = randomAccessFile.getChannel();  
        MappedByteBuffer mappedByteBuffer = fileChannel.map(FileChannel.MapMode.READ_WRITE, 0, 5);  
  
        mappedByteBuffer.put(0, (byte) 'a');  
        mappedByteBuffer.put(3, (byte) 'b');  
  
        randomAccessFile.close();  
    }  
}
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEwOTE4Mzk0NTMsNjI2MDk0ODk2XX0=
-->