


> Written with [StackEdit](https://stackedit.io/).

## Scroller

Scroller 是弹性滑动对象，当我们在使用 View 的 scrollTo/scrollBy 方法进行滑动的时候，其过程都是瞬间完成的，是没有过渡效果的。这个时候就可以使用 Scroller 来实现有过渡效果的滑动。

Scroller 本身无法让 View 滑动，他需要和 View 的 computeScroll 方法配合使用才能共同完成这个功能。

我们先来看看 View 的 scrollTo/s

<!--stackedit_data:
eyJoaXN0b3J5IjpbMTUzODAzNTQ0OF19
-->