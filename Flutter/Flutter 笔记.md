


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

### FlutterActivity 源码
在 Android 应用中，每创建一个 FlutterActivity 就会创建一个新的 FlutterEngine。这样就会导致 FlutterActivity 在启动的时候会有一个初始化的时间，会导致页面短暂的出现白屏。这种情况会出现在如果APP 中有多个 Activity 的情况且 FlutterActivity 不是作为APP 的第一个页面。

如果 APP 就是一个单页面的应用，那么这个短暂的白屏是感觉不出来的，因为 Flutter 有 SplashView。当 FlutterView 的第一帧绘制完成之后，SplashView 才会消失。

针对上面的第一中情况，Flutter 提供了引擎复用的策略。就是我们先初始化一个 FlutterEngine ，然后在新的 FlutterActivity 中使用我们缓存的那个 Engine，就不会导致白屏。具体使用如下：

创建并预热一个 FlutterEngine

```java
	FlutterEngine flutterEngine = new FlutterEngine(context);
	flutterEngine.getDartExecutor().executeDartEntrypoint(DartEntrypoint.createDefault());
	// 将预热的 FlutterEngine 缓存到 FlutterEngineCache 中
	// Cache the pre-warmed FlutterEngine in the FlutterEngineCache.
	FlutterEngineCache.getInstance().put("my_engine", flutterEngine);
```
然后在需要使用FlutterEngine 的地方使用我们预先缓存的 engine 就好。

```java
FlutterEngineCache.getInstance().get("my_engine");
```

接下来再看看 FlutterActivity 里面干了哪些事情：

```java
  @Override
  protected void onCreate(@Nullable Bundle savedInstanceState) {
	// 切换注意，有 LaunchTheme 切换到 NormalTheme
    switchLaunchThemeForNormalTheme();

    super.onCreate(savedInstanceState);
	// 实例化一个 FlutterActivityAndFragmentDelegate
    delegate = new FlutterActivityAndFragmentDelegate(this);
    delegate.onAttach(this);
    delegate.onRestoreInstanceState(savedInstanceState);
	// 通知 lifecycle 当前的生命周期状态
    lifecycle.handleLifecycleEvent(Lifecycle.Event.ON_CREATE);
    // 配置 window 为透明
    configureWindowForTransparency();
	// 为 FlutterActivity 设置 view ，由 createFlutterView 方法生成
    setContentView(createFlutterView());

    configureStatusBarForFullscreenFlutterExperience();
  }
````

createFlutterView() 源代码如下：

```java
@NonNull
  private View createFlutterView() {
    return delegate.onCreateView(
        /* inflater=*/ null,
        /* container=*/ null,
        /* savedInstanceState=*/ null,
        /*flutterViewId=*/ FLUTTER_VIEW_ID,
        /*shouldDelayFirstAndroidViewDraw=*/ getRenderMode() == RenderMode.surface);
  }
```

还是去使用的 FlutterActivityAndFragmentDelegate 的 onCreateView 方法创建的一个 view 

接下来看看 FlutterActivityAndFragmentDelegate 这个代理的构造方法：

```java
FlutterActivityAndFragmentDelegate(@NonNull Host host) {
    this.host = host;
  }
```

构造方法里面接收了一个 Host 对象，FlutterActivity 实现了 FlutterActivityAndFragmentDelegate.Host 这个接口。

一般在使用默认的方式创建 Flutter 程序的时候，在FlutterEngine 创建的时候，FlutterEngine 就会自动去注册我们在 pubspec.yml 中依赖的插件，如下：

```java
GeneratedPluginRegister.registerGeneratedPlugins(this);
```

源代码如下：

```java
	public static void registerGeneratedPlugins(@NonNull FlutterEngine flutterEngine) {
    try {
      Class<?> generatedPluginRegistrant =
          Class.forName("io.flutter.plugins.GeneratedPluginRegistrant");
      Method registrationMethod =
          generatedPluginRegistrant.getDeclaredMethod("registerWith", FlutterEngine.class);
      registrationMethod.invoke(null, flutterEngine);
    } catch (Exception e) {
      Log.e(
          TAG,
          "Tried to automatically register plugins with FlutterEngine ("
              + flutterEngine
              + ") but could not find or invoke the GeneratedPluginRegistrant.");
      Log.e(TAG, "Received exception while registering", e);
    }
  }
```

会通过反射的方式去调用 Flutter 自动生成的类 GeneratedPluginRegistrant 的 registerWith 方法，如下：

```java
public final class GeneratedPluginRegistrant {
  public static void registerWith(@NonNull FlutterEngine flutterEngine) {
	// Flutter 工程依赖的 Flutter 插件
    flutterEngine.getPlugins().add(new com.flutter.flutter_plugin_test.FlutterPluginTestPlugin());
  }
}
```