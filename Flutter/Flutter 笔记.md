


> Written with [StackEdit](https://stackedit.io/).

# Flutter 

### 判断设备是手机还是 pad

判断方法是获取设备的最短边，来判断这条边的大小，如：
```dart
// 屏幕最短边的大小 / 屏幕像素密度
double shortestSide = window.physicalSize.shortestSide / window.devicePixelRatio;
if(shortestSide > 600){
	// iPad
}else {
	// iPhone
}
```

iPhone 和 iPad 的尺寸如下：

![](https://user-gold-cdn.xitu.io/2019/6/9/16b3ae6f0c71d2cd?w=1720&h=1096&f=png&s=234941)
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTczMjMxNDkzMywyMDcyNzI5NjgwXX0=
-->