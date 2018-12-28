


> Written with [StackEdit](https://stackedit.io/).

```dart
/// An object in the render tree.  
///  
/// The [RenderObject] class hierarchy is the core of the rendering  
/// library's reason for being.  
///  
/// [RenderObject]s have a [parent], and have a slot called [parentData] in  
/// which the parent [RenderObject] can store child-specific data, for example,  
/// the child position. The [RenderObject] class also implements the basic  
/// layout and paint protocols.  
///  
/// The [RenderObject] class, however, does not define a child model (e.g.  
/// whether a node has zero, one, or more children). It also doesn't define a  
/// coordinate system (e.g. whether children are positioned in Cartesian  
/// coordinates, in polar coordinates, etc) or a specific layout protocol (e.g.  
/// whether the layout is width-in-height-out, or constraint-in-size-out, or  
/// whether the parent sets the size and position of the child before or after  
/// the child lays out, etc; or indeed whether the children are allowed to read  
/// their parent's [parentData] slot).  
///  
/// The [RenderBox] subclass introduces the opinion that the layout  
/// system uses Cartesian coordinates.  
///  
/// ## Writing a RenderObject subclass  
///  
/// In most cases, subclassing [RenderObject] itself is overkill, and  
/// [RenderBox] would be a better starting point. However, if a render object  
/// doesn't want to use a Cartesian coordinate system, then it should indeed  
/// inherit from [RenderObject] directly. This allows it to define its own  
/// layout protocol by using a new subclass of [Constraints] rather than using  
/// [BoxConstraints], and by potentially using an entirely new set of objects  
/// and values to represent the result of the output rather than just a [Size].  
/// This increased flexibility comes at the cost of not being able to rely on  
/// the features of [RenderBox]. For example, [RenderBox] implements an  
/// intrinsic sizing protocol that allows you to measure a child without fully  
/// laying it out, in such a way that if that child changes size, the parent  
/// will be laid out again (to take into account the new dimensions of the  
/// child). This is a subtle and bug-prone feature to get right.  
///  
/// Most aspects of writing a [RenderBox] apply to writing a [RenderObject] as  
/// well, and therefore the discussion at [RenderBox] is recommended background  
/// reading. The main differences are around layout and hit testing, since those  
/// are the aspects that [RenderBox] primarily specializes.  
///  
/// ### Layout  
///  
/// A layout protocol begins with a subclass of [Constraints]. See the  
/// discussion at [Constraints] for more information on how to write a  
/// [Constraints] subclass.  
///  
/// The [performLayout] method should take the [constraints], and apply them.  
/// The output of the layout algorithm is fields set on the object that describe  
/// the geometry of the object for the purposes of the parent's layout. For  
/// example, with [RenderBox] the output is the [RenderBox.size] field. This  
/// output should only be read by the parent if the parent specified  
/// `parentUsesSize` as true when calling [layout] on the child.  
///  
/// Anytime anything changes on a render object that would affect the layout of  
/// that object, it should call [markNeedsLayout].  
///  
/// ### Hit Testing  
///  
/// Hit testing is even more open-ended than layout. There is no method to  
/// override, you are expected to provide one.  
///  
/// The general behavior of your hit-testing method should be similar to the  
/// behavior described for [RenderBox]. The main difference is that the input  
/// need not be an [Offset]. You are also allowed to use a different subclass of  
/// [HitTestEntry] when adding entries to the [HitTestResult]. When the  
/// [handleEvent] method is called, the same object that was added to the  
/// [HitTestResult] will be passed in, so it can be used to track information  
/// like the precise coordinate of the hit, in whatever coordinate system is  
/// used by the new layout protocol.  
///  
/// ### Adapting from one protocol to another  
///  
/// In general, the root of a Flutter render object tree is a [RenderView]. This  
/// object has a single child, which must be a [RenderBox]. Thus, if you want to  
/// have a custom [RenderObject] subclass in the render tree, you have two  
/// choices: you either need to replace the [RenderView] itself, or you need to  
/// have a [RenderBox] that has your class as its child. (The latter is the much  
/// more common case.)  
///  
/// This [RenderBox] subclass converts from the box protocol to the protocol of  
/// your class.  
///  
/// In particular, this means that for hit testing it overrides  
/// [RenderBox.hitTest], and calls whatever method you have in your class for  
/// hit testing.  
///  
/// Similarly, it overrides [performLayout] to create a [Constraints] object  
/// appropriate for your class and passes that to the child's [layout] method.  
///  
/// ### Layout interactions between render objects  
///  
/// In general, the layout of a render object should only depend on the output of  
/// its child's layout, and then only if `parentUsesSize` is set to true in the  
/// [layout] call. Furthermore, if it is set to true, the parent must call the  
/// child's [layout] if the child is to be rendered, because otherwise the  
/// parent will not be notified when the child changes its layout outputs.  
///  
/// It is possible to set up render object protocols that transfer additional  
/// information. For example, in the [RenderBox] protocol you can query your  
/// children's intrinsic dimensions and baseline geometry. However, if this is  
/// done then it is imperative that the child call [markNeedsLayout] on the  
/// parent any time that additional information changes, if the parent used it  
/// in the last layout phase. For an example of how to implement this, see the  
/// [RenderBox.markNeedsLayout] method. It overrides  
/// [RenderObject.markNeedsLayout] so that if a parent has queried the intrinsic  
/// or baseline information, it gets marked dirty whenever the child's geometry  
/// changes.

```
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE4MTU4NzA4MTNdfQ==
-->