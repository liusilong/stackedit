


> Written with [StackEdit](https://stackedit.io/).

```java
public class MyLinearLayout2 extends LinearLayout {  
  private static final String TAG = "MyLinearLayout2";  
 private boolean isShow;  
  
 private View headView;  
  
 private int minDownAreaPosition;  
 private int maxDownAreaPosition;  
  
 private boolean canScroll;  
  
 private float mLastY;  
  
 private int maxY = DensityUtil.dip2px(getContext(), 300);  
 private int minY = 0;  
  
  
 private boolean scrollUp;  
  
 private Scroller scroller;  
  
  
 public MyLinearLayout2(Context context) {  
  super(context);  
  }  
  
  public MyLinearLayout2(Context context, @Nullable AttributeSet attrs) {  
  super(context, attrs);  
  minDownAreaPosition =  
                DensityUtil.dip2px(context, 300);  
  maxDownAreaPosition = minDownAreaPosition +  
                DensityUtil.dip2px(context, 20);  
  scroller = new Scroller(context);  
  Log.e(TAG, "MyLinearLayout2: 导航栏： " + DensityUtil.getDaoHangHeight(context));  
  }  
  
  
  @Override  
  public boolean dispatchTouchEvent(MotionEvent ev) {  
  float y = ev.getY();  
 float dy = 0f;  
 switch (ev.getAction()) {  
  case MotionEvent.ACTION_DOWN:  
//                if (y > minDownAreaPosition && y < maxDownAreaPosition) {  
//                    Log.e(TAG, "dispatchTouchEvent: 000");  
//                    canScroll = true;  
//                    mLastY = y;  
//                } else {  
//                    Log.e(TAG, "dispatchTouchEvent: 111");  
//                }  
//                mLastY = y;  
  break;  
 case MotionEvent.ACTION_MOVE:  
                if (y < maxDownAreaPosition) {  
  if (!canScroll) {  
  mLastY = y;  
  canScroll = true;  
  }  
  dy = mLastY - y;  
 if (dy > 0) { // 上滑  
  scrollUp = true;  
  } else if (dy < 0) {  
  scrollUp = false; // 下滑  
  }  
  scrollBy(0, (int) (dy + 0.5));  
  mLastY = y;  
  }  
  
  break;  
 case MotionEvent.ACTION_UP:  
                int scrollY1 = getScrollY();  
 int dy1 = maxY - scrollY1;  
 if (scrollUp) {  
  Log.e(TAG, "dispatchTouchEvent: 上滑");  
  scroller.startScroll(0, scrollY1, 0, dy1);  
  } else {  
  Log.e(TAG, "dispatchTouchEvent: 下滑");  
  scroller.startScroll(0, scrollY1, 0, -getScrollY());  
  }  
  invalidate();  
  canScroll = false;  
 break;  
  }  
  return super.dispatchTouchEvent(ev);  
  }  
  
  @Override  
  public void computeScroll() {  
  if (scroller.computeScrollOffset()) {  
  scrollTo(scroller.getCurrX(), scroller.getCurrY());  
  postInvalidate();  
  }  
 }  
  @Override  
  public void scrollBy(int x, int y) {  
  int scrollY = getScrollY();  
 int toY = scrollY + y;  
 if (toY >= maxY) {  
  toY = maxY;  
  } else if (toY <= minY) {  
  toY = minY;  
  }  
  y = toY - scrollY;  
 super.scrollBy(x, y);  
  
  }  
  
  @Override  
  public void scrollTo(int x, int y) {  
  if (y >= maxY) {  
  y = maxY;  
  } else if (y <= minY) {  
  y = minY;  
  }  
  super.scrollTo(x, y);  
  }  
  
  @Override  
  protected void onFinishInflate() {  
  super.onFinishInflate();  
  }  
}
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTkxMDY3MTMwMl19
-->