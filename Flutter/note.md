


> Written with [StackEdit](https://stackedit.io/).

1. 执行`flutter ...` 相关的命令是，可能会出现如下提示
	```shell
	 Waiting for another flutter command to release the startup lock...
	```
	解决方法如下：
	```shell
	rm ./flutter/bin/cache/lockfile
	```
这是我在 offLine 的时候写的文字
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTQyNjM2MDI1MywyMTI2NTg3MzYxXX0=
-->