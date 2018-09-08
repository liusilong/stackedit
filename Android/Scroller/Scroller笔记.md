


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
这两个变量分别可以通过 getScrollX() h

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE5Mjg5NzU3MzcsLTIwNzcxODgzNTddfQ
==
-->