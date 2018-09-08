


> Written with [StackEdit](https://stackedit.io/).

## Scroller

Scroller 是弹性滑动对象，当我们在使用 View 的 scrollTo/scrollBy 方法进行滑动的时候，其过程都是瞬间完成的，是没有过渡效果的。这个时候就可以使用 Scroller 来实现有过渡效果的滑动。

Scroller 本身无法让 View 滑动，他需要和 View 的 computeScroll 方法配合使用才能共同完成这个功能。

我们先来看看 View 的 scrollTo/scrollBy 方法的实现

### scrollTo 、scrollBy
```java
/**  
 * Set the scrolled position of your view. This will cause a call to * {@link #onScrollChanged(int, int,    	 	int, int)} and the view will be  invalidated. 
 * @param x the x position to scroll to  
 * @param y the y position to scroll to  
 */
 public void scrollTo(int x, int y) {  
  if (mScrollX != x || mScrollY != y) {  
  int oldX = mScrollX;  
  int oldY = mScrollY;  
  mScrollX = x;  
  mScrollY = y;  
  invalidateParentCaches();  
  onScrollChanged(mScrollX, mScrollY, oldX, oldY);  
  if (!awakenScrollBars()) {  
  postInvalidateOnAnimation();  
  }  
 }}
```
`scrollTo` 方法方法中接受两个参数，`x` 和 `y` , 这两个参数代表 View 将要滑动到的位置（绝对位置）。

```java
/**  
 * Move the scrolled position of your view. This will cause a call to * {@link #onScrollChanged(int, int, int, int)} and the view will be  
 * invalidated. * @param x the amount of pixels to scroll by horizontally  
 * @param y the amount of pixels to scroll by vertically  
 */
 public void scrollBy(int x, int y) {  
  scrollTo(mScrollX + x, mScrollY + y);  
}
```
scrollBy 方法是相对于当前位置滑动，当前位置为 mScrollX， 该变量可以同个 getScrollX() 方法获取。

#### mScrollX 、mScrollY
这两个变量分别可以通过 `getScrollX()` 和 `getScrollY()` 方法来获取。

**mScrollX** 的值总是等于 View 左边缘和 View 内容左边缘在水平方向的距离。
**mScrollY** 的值总是等于 View 上边缘和 View 内容上边缘在竖直方向的距离。

**scrollTo 和 scrollBy 只能改变 View 内容的位置而不能改变 View 在布局中的位置。**

下面我们以图示的方式来说明 mScrollX 和 mScrollY 这两个属性的变换规则。
> 图中实线框代表 View 而蓝色的虚线框代表 View 的内容
> 比方说 Button 是代表 View 而 Button 上的 text 则代表 View 的内容
> 我们以 V 代表 View 左上角的点，C 代表 View 内容的左上角的点
> Vx、Vy、Cx、Cy 分别代表点 V 和点 C 的横纵坐标点
> 我们假设水平和竖直方向的滑动距离都为 100

mScrollX = 0
mScrollY = 0

![](https://user-gold-cdn.xitu.io/2018/9/8/165b87c9cd0ca944?w=360&h=280&f=png&s=18065)

mScrollX = 100

![](https://user-gold-cdn.xitu.io/2018/9/8/165b888bd0baf18e?w=788&h=572&f=png&s=51185)

mScrollX = -100

![](https://user-gold-cdn.xitu.io/2018/9/8/165b8955be76a869?w=738&h=594&f=png&s=52436)

mScrollY = 100

![](https://user-gold-cdn.xitu.io/2018/9/8/165b8979af09e08d?w=904&h=566&f=png&s=52081)

mScrollY = -100

![](https://user-gold-cdn.xitu.io/2018/9/8/165b89932ef4756a?w=922&h=566&f=png&s=51788)

### Scroller
下面说说 Scroller 的固定用法

```java
Scrller scroller = new Scroller(context);

private void smoothScrollTo(int destX, int destY) {
	int scrollX = getScrollX();
	int delta = destX - scrollX;
	scroller.start(scrollx, 0, delta, 0, 1000);
	invalidate();
}

@Override
public void computeScroll() {
	if(scroller.computeScrollOffset()) {
		scrollTo(sc
	}
}


```
<!--stackedit_data:
eyJoaXN0b3J5IjpbODA4NzQ4MTc5LDczODU4Njg2OSwxNjA0Mj
M0NjUsLTE0MjQ2MDIzOTAsLTU5MDUzMjY2MywtMjA3NzE4ODM1
N119
-->