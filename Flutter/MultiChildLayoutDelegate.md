


> Written with [StackEdit](https://stackedit.io/).
 A delegate that controls the layout of multiple children.

 Delegates must be idempotent. Specifically, if two delegates are equal, then
 they must produce the same layout. To change the layout, replace the
 delegate with a different instance whose [shouldRelayout] returns true when
 given the previous instance.

 Override [getSize] to control the overall size of the layout. The size of
 the layout cannot depend on layout properties of the children.

 Override [performLayout] to size and position the children. An
 implementation of [performLayout] must call [layoutChild] exactly once for
 each child, but it may call [layoutChild] on children in an arbitrary order.
 Typically a delegate will use the size returned from [layoutChild] on one
 child to determine the constraints for [performLayout] on another child or
 to determine the offset for [positionChild] for that child or another child.

 Override [shouldRelayout] to determine when the layout of the children needs
 to be recomputed when the delegate changes.

 Used with [CustomMultiChildLayout], the widget for the
 [RenderCustomMultiChildLayoutBox] render object.

 Each child must be wrapped in a [LayoutId] widget to assign the id that
 identifies it to the delegate. The [LayoutId.id] needs to be unique among
 the children that the [CustomMultiChildLayout] manages.

 {@tool sample}

 Below is an example implementation of [performLayout] that causes one widget
 (the follower) to be the same size as another (the leader):

 ```dart
 // Define your own slot numbers, depending upon the id assigned by LayoutId.
 // Typical usage is to define an enum like the one below, and use those
 // values as the ids.
 enum _Slot {
   leader,
   follower,
 }

 class FollowTheLeader extends MultiChildLayoutDelegate {
   @override
   void performLayout(Size size) {
     Size leaderSize = Size.zero;

     if (hasChild(_Slot.leader)) {
       leaderSize = layoutChild(_Slot.leader, BoxConstraints.loose(size));
       positionChild(_Slot.leader, Offset.zero);
     }

     if (hasChild(_Slot.follower)) {
       layoutChild(_Slot.follower, BoxConstraints.tight(leaderSize));
       positionChild(_Slot.follower, Offset(size.width - leaderSize.width,
           size.height - leaderSize.height));
     }
   }

   @override
   bool shouldRelayout(MultiChildLayoutDelegate oldDelegate) => false;
 }
 ```
 {@end-tool}

 The delegate gives the leader widget loose constraints, which means the
 child determines what size to be (subject to fitting within the given size).
 The delegate then remembers the size of that child and places it in the
 upper left corner.

 The delegate then gives the follower widget tight constraints, forcing it to
 match the size of the leader widget. The delegate then places the follower
 widget in the bottom right corner.

 The leader and follower widget will paint in the order they appear in the
 child list, regardless of the order in which [layoutChild] is called on
 them.
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTQ3MjU0Njg4MV19
-->