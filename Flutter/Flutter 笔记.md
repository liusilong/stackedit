


> Written with [StackEdit](https://stackedit.io/).

# Flutter 

### 判断设备是手机还是 pad
判断方法是获取设备的最短边，来判断这条边的大小，如：
```dart
// 屏幕最短边的大小 / 屏幕像素密度
double shortestSide = window.physicalSize.shortestSide / window.devicePixelRatio;
if(shortestSide > 600){
	// pad
}else {
	// phone
}
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbMjA3MjcyOTY4MF19
-->