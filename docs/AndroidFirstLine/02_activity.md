# 🤖 应用界面 Activity
上一篇 Android 启程 的文章中，Android Studio 在创建项目时自动创建了一个 MainActivity，界面上展示的 Hello World 文案就是在 MainActivity 中。这篇文章将更加详细地介绍 Activity 的相关知识。

## Activity 是什么
首先第一个问题是「 Activity 是什么？」先来看一段 [官网](https://developer.android.com/reference/android/app/Activity) 上对于 Activity 的描述：
> An activity is a single, focused thing that the user can do. Almost all activities interact with the user, so the Activity class takes care of creating a window for you in which you can place your UI.

我们可以翻译并简化一下：**Activity 是一个包含用户界面的系统组件，主要作用是展示界面、处理用户交互**。它是 Android 的四大组件之一，Activity 这个单词包含两种含义：第一，特指 Android SDK 中提供的 `Activity` 这个类；第二，泛指 Activity 这一类组件，它们通常代表应用的各个页面，直接或间接继承 Activity SDK。
## Activity 的基本用法
### 创建一个 Activity
在项目 `src/<packagename>` 目录下，创建一个类，比如命名为 `FirstActivity`，继承自 `AppcompatActvity`。
> Activity 的命名是任意的，通常会按照业务名 + Activity 后缀来进行命名，比如 ProfileActivity，表示是用户资料界面的 Activity。

```java
class FirstActivity: AppCompatActivity() {
    
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
    }
}
```
AppCompatActivity 是 Activity 类的子类，但不包含在 Android SDK 中，而是需要在 build.gradle 中额外引入：
```java
dependencies {
    implementation 'androidx.appcompat:appcompat:1.4.1'
}
```
`compat` 一词是兼容的意思，AppCompatActivity 是为了兼容各个 Android SDK 版本的 Activity，提供一致的行为，通常项目中的 Activity 都会继承自 AppCompatActivity 而不是直接继承自 SDK 中 Activity 类。另外上面代码中，重写了父类的 `onCreate` 方法，它是 Activity 的一个生命周期回调方法（后面小节会介绍），在 Activity 创建时被调用并且只会被调用一次，是每个 Activity 都会重写的方法，通常在这个方法中加载界面、加载数据。
### 创建并加载布局
Android 的 UI 布局写在 XML 文件中，然后在 Activity 中进行引用。在 `app/src/main/res/layout` 目录下右键，选择「New」-> 「Layout Resource File」，会弹出新建布局资源文件的窗口，将这个布局文件命名为 first_layout，根元素默认选择为 LinearLayout。完成后，会在 layout 目录下生成一个 first_layout.xml 文件。打开该文件看下内容（Android Studio 需要切换到 Code 标签页下）：
```groovy
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

</LinearLayout>
```
`LinearLayout` 表示一个线性布局，其中的 UI 元素（Android 中称为控件，下文中都会以控件代替 UI 元素）会按水平或垂直方向依次排列。在 LinearLayout 中添加一个按钮。
```kotlin
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
    <Button
		android:id="@+id/button1"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Button 1" />
</LinearLayout>
```
添加了 `<Button/>` 控件，并在内部添加了几个属性：

- `android:id` 是当前控件的唯一标识符，之后可以通过它在 Kotlin 代码中找到这个控件并对其进行操作。它的值 `@+id/button1` 表示定义一个 id，XML 中引用一个 id 的写法是 `@id/button1`，在 Kotlin 代码中则是 `R.id.button1`。
- `android:layout_width` 指定了控件的宽度，`match_parent` 表示和父控件一样宽。
- `android:layout_height` 指定了控件的高度，`wrap_content` 表示高度刚好包含里面的内容。
- `android:text` 指定了按钮中显示的文字。

可以通过切换到 Split/Design 标签页查看预览效果：

![image.png](https://s2.loli.net/2024/01/11/plYUxJRNycECmi9.png)

重新回到 FirstActivity，在 onCreate 中加入如下代码：
```kotlin
class FirstActivity: AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.first_layout)
    }
}
```
在第 4 行新增了一行 `setContentView()`，调用这个方法来给当前的 Activity 增加一个布局，这里引用了 `R.layout.first_layout`，表示引用 `first_layout.xml` 这个布局资源。
### 注册 Activity
所有的 Activity 都需要在 `AndroidManifest.xml` 中注册才可以生效，否则在打开时，会发生应用崩溃。崩溃堆栈如下，也会提示我们需要在 Manifest 中注册（第 3 行）。
```kotlin
FATAL EXCEPTION: main
Process: life.lixiaoyu.androidfirstlineofcode, PID: 28695
android.content.ActivityNotFoundException: Unable to find explicit activity class {life.lixiaoyu.androidfirstlineofcode/life.lixiaoyu.androidfirstlineofcode.chapter3.FirstActivity}; have you declared this activity in your AndroidManifest.xml?
	at android.app.Instrumentation.checkStartActivityResult(Instrumentation.java:2085)
    at android.app.Instrumentation.execStartActivity(Instrumentation.java:1747)
	at android.app.Activity.startActivityForResult(Activity.java:5533)
    at androidx.activity.ComponentActivity.startActivityForResult(ComponentActivity.java:728)
    at android.app.Activity.startActivityForResult(Activity.java:5486)
    at androidx.activity.ComponentActivity.startActivityForResult(ComponentActivity.java:709)
    at android.app.Activity.startActivity(Activity.java:5892)
    at android.app.Activity.startActivity(Activity.java:5845)
```
打开 AndroidManifest 文件，里面的内容如下：
```kotlin
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools">
    <application
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/Theme.AndroidFirstLineOfCode">
        <activity
            android:name=".MainActivity"
            android:exported="true">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>
</manifest>
```
可以看到 `<manifest>` 标签中有个 `<application>` 标签，这里可以设置一些应用全局的属性，比如图标、主题等。`<application>` 标签下有 `<activity>` 标签，这就是用来注册 Activity 组件，Android Studio 创建项目时默认生成的 MainActivity 已经注册了。来看看 `<activity>` 标签下的属性：

- `android:name`：填写 Activity 类的全路径，这里 `.MainActivity` 是个缩写，实际上是 `项目 <package-name> + 相对路径`
- `android:exported` ：表示这个 Activity 是否可以被其他 App 调起，因为 MainActivity 是应用的入口 Activity，会通过桌面 Launcher 启动，所以需要设置为 true

`<activity>` 下还有 ` <intent-filter>` 标签，里面的配置表示这个 Activity 是应用的主 Activity（入口 Activity）。
```xml
<intent-filter>
  <action android:name="android.intent.action.MAIN" />
  <category android:name="android.intent.category.LAUNCHER" />
</intent-filter>
```
将 MainActivity 替换成我们自己创建的 FirstActivity：
```kotlin
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools">
    <application
        ...>
        <activity
            android:name=".FirstActivity"
            android:exported="true">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>
</manifest>
```
这样点击桌面应用图标打开的 Activity 就是 FirstActivity 了。
>  注意：如果应用没有声明任何一个 Activity 作为主 Activity，这个应用还是可以正常安装，但无法在桌面上看到或打开这个应用，这种应用通常是作为第三方服务供其他应用在内部调用的。

![FirstActivity 展示](https://s2.loli.net/2024/01/11/dSHDWZfb9qB5uxy.png)

### 在 Activity 中使用 Toast
前面介绍了 Activity 用于展示 UI 界面的作用，接下来看看在 Activity 处理用户的交互。当用户点击屏幕上的按钮，弹出一个 Toast 给用户，Toast 是 Android 系统提供的一种提醒方式，使用 Toast 可以将一些短小的信息通知给用户，这些信息会在一段时间后自动消失，不会占用任何屏幕空间。
```kotlin
class FirstActivity: AppCompatActivity() {

    private lateinit var button1: Button
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.first_layout)
    	// 注释 1
        button1 = findViewById(R.id.button1)
        // 注释 2
        button1.text = "Show Toast"
        // 注释 3
        button1.setOnClickListener(object: OnClickListener {
            override fun onClick(v: View?) {
                Toast.makeText(this@FirstActivity, "Click Button 1", Toast.LENGTH_SHORT).show()
            }
        })
    }
}
```
上面代码展示了如何在 Activity 里处理 UI 逻辑：

- 注释 1 处通过 `findViewById` 方法获取到布局文件里 id 为 `button1` 的 按钮控件，它返回一个 View 的子类，将其赋值给 button1 实例，这里 Kotlin 无法自动推断出它是 Button 还是其他控件类型，所以需要显式声明 button1 的类型。button1 成为这个按钮的一个句柄，通过这个句柄可以对控件进行操作，比如修改按钮的文字、设置点击监听器等。
- 注释 2 处通过调用 `setText` 方法，修改了按钮上的文字。
- 注释 3 处通过调用 `setOnClickListener()` 方法给 button1 设置了一个点击监听器，setOnClickListener 接收一个 `OnClickListener` 类型的参数，这是一个接口，在 `onClick` 回调方法中执行点击按钮的逻辑：
```java
public interface OnClickListener {
    /**
	* Called when a view has been clicked.
	*
	* @param v The view that was clicked.
	*/
    void onClick(View v);
}
```
在 Kotlin 中，单方法的匿名内部类可以用 Lambda 来简化书写，所以最终写出来是这样的：
```java
class FirstActivity: AppCompatActivity() {

    private lateinit var button1: Button
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.first_layout)
    	// 注释 1
        button1 = findViewById(R.id.button1)
        // 注释 2
        button1.text = "Show Toast"
        // 注释 3
        button1.setOnClickListener {
            Toast.makeText(this@FirstActivity, "Click Button 1", Toast.LENGTH_SHORT).show()
        }
    }
}
```
在点击按钮时，会调用 `Toast.makeText().show()` 方法，在屏幕上展示一个 Toast。
表现：

![展示 Toast](https://s2.loli.net/2024/01/11/bok2G9vjEwBx7Qu.png)

### 销毁 Activity
只需要点击一下系统返回键，如果是全面屏手势滑动，则手势侧滑，就可以销毁当前的 Activity。如果希望在代码里销毁 Activity，则可以调用 Activity 的 `finish()` 方法。
```kotlin
button1.setOnClickListener {
    finish()
}
```
## Activity 切换
应用中一般会有多个 Activity 实现的界面，下面来看下如何从一个 Activity 跳转到另一个 Activity。
### 使用显式 Intent
首先和 FirstActivity 一样，创建一个 SecondActivity，并在 AndroidManifest 中注册。
Activity 间的跳转需要使用 Intent。Intent 是 Android 程序中各组件之间交互的重要方式，它指定当前组件想要执行的动作，还可以传递数据，可以用于启动 Activity，启动 Service，发送广播等场景。Service 和广播等概念还未接触，可以先忽略，后面的文章会进行介绍。
Intent 可以分为两种，显式 Intent 和隐式 Intent。先来看下显式 Intent 的使用：
通过 `Intent(Context packageContext, Class<?> cls)` 这个 Intent 的构造函数来创建一个显式 Intent，这个构造函数接收两个参数：第一个参数 Context 是启动 Activity 的上下文，第二个参数 Class 指定要跳转的目标 Activity。然后调用 Activity 提供的 `startActivity(Intent intent)` 方法来跳转。
```java
button2.setOnClickListener { 
    startActivity(Intent(this, SecondActivity::class.java))
}
```
这种启动 Activity 的方式，明确指定了目标 Activity，Intent 的「意图」非常明显，所以被称为显式 Intent。
### 使用隐式 Intent
相比于显式 Intent，隐式 Intent 不明确指出要启动哪个 Activity，而是指定了一系列更为抽象的 action 和 category 信息，交给系统匹配可以响应这个隐式 Intent 的 Activity 来启动。
新建一个 ThirdActivity，在 AndroidManifest 中，给 ThirdActivity 的的 `<activity>` 添加 `<intent-filter>` 标签，指定当前 Activity 能够响应的 action 和 category：
```xml
<activity android:name=".chapter3.ThirdActivity" android:exported="true">
  <intent-filter>
    <action android:name="life.lixiaoyu.androidfirstlineofcode.ACTION_START" />
    <category android:name="android.intent.category.DEFAULT" />
  </intent-filter>
</activity>
```

- `<activity>` 标签中有个 `android:exported="true"` 属性，从 Android 12 开始，所有设置了 intent-filter 的 Activity 都需要显式申明是否要对外暴露。
- `<action>` 标签指定这个 Activity 可以响应 `life.lixiaoyu.androidfirstlineofcode.ACTION_START` 这个 action。
- `<category>` 标签包含一些附加信息，更精确指定能够响应的 Intent 还可能带有的 category，只有 `<action>` 和 `<categor/>` 中的内容同时匹配 Intent 中指定的 action 和 category 时，这个 Activity 才能响应这个 Intent。

在 FirstActivity 中添加一个按钮，点击事件如下：
```kotlin
button3.setOnClickListener {
    val intent = Intent("life.lixiaoyu.androidfirstlineofcode.ACTION_START")
    startActivity(intent)
}
```
使用了另一个 Intent 的构造函数，传入 action 的字符串，表示想要启动能够响应 `life.lixiaoyu.androidfirstlineofcode.ACTION_START` 这个 action 的 Activity，这里没有指定 category，startActivity 时会自动将一个默认的 category（`android.intent.category.DEFAULT`）添加到 Intent 中。点击按钮，可以正常跳转到 ThirdActivity。
每个 Intent 中只能指定一个 action，但能指定多个 category。修改下点击事件中的代码：
```kotlin
button3.setOnClickListener {
    val intent = Intent("life.lixiaoyu.androidfirstlineofcode.ACTION_START")
    intent.addCategory("life.lixiaoyu.androidfirstlineofcode.MY_CATEGORY")
    startActivity(intent)
}
```
重新运行程序，点击按钮，没有顺利跳转到 ThirdActivity，而是程序崩溃了，崩溃堆栈如下：
```
FATAL EXCEPTION: main
Process: life.lixiaoyu.androidfirstlineofcode, PID: 24965
android.content.ActivityNotFoundException: No Activity found to handle Intent { act=life.lixiaoyu.androidfirstlineofcode.ACTION_START cat=[life.lixiaoyu.androidfirstlineofcode.MY_CATEGORY] }
	at android.app.Instrumentation.checkStartActivityResult(Instrumentation.java:2087)
	at android.app.Instrumentation.execStartActivity(Instrumentation.java:1747)
	at android.app.Activity.startActivityForResult(Activity.java:5533)
	at androidx.activity.ComponentActivity.startActivityForResult(ComponentActivity.java:728)
	at android.app.Activity.startActivityForResult(Activity.java:5486)
	at androidx.activity.ComponentActivity.startActivityForResult(ComponentActivity.java:709)
	at android.app.Activity.startActivity(Activity.java:5892)
```
堆栈中的错误信息告诉我们，没有任何一个 Activity 可以响应这个 Intent，这是因为在 Intent 中添加 category，但没有在 AndroidManifest 中声明。在 AndroidManifest 中修改一下，给 ThirdActivity 的 intent-filter 添加一个 `life.lixiaoyu.androidfirstlineofcode.MY_CATEGORY` 的 category，重新运行程序，就可以正常跳转了。
```xml
<activity android:name=".chapter3.ThirdActivity" android:exported="true">
  <intent-filter>
    <action android:name="life.lixiaoyu.androidfirstlineofcode.ACTION_START" />
    <category android:name="android.intent.category.DEFAULT" />
    <category android:name="life.lixiaoyu.androidfirstlineofcode.MY_CATEGORY" />
  </intent-filter>
</activity>
```
### 隐式 Intent 的更多用法
使用隐式 Intent，不仅可以启动自己程序内的 Activity，还可以启动其他程序的 Activity，这使得可以在多个应用程序间共享功能。比如应用中需要展示一个网页，这时没有必要自己来实现一个浏览器，只需要调用系统的浏览器来打开这个网页即可。
#### Intent.Action_VIEW
`Intent.Action_VIEW` 这个 action 可以用来打开网页
```kotlin
button4.setOnClickListener {
    val intent = Intent(Intent.ACTION_VIEW)
    intent.data = Uri.parse("https://www.baidu.com")
    startActivity(intent)
}
```

- 指定了 Intent 的 action 是 Intent.ACTION_VIEW，这是个 Android 系统内置的 action，常量值为 `android.intent.action.VIEW`。
-  通过 `Uri.parse()` 方法将一个网址字符串解析成 Uri 对象，再调用 Intent 的 `setData` 方法将 Uri 对象传递给 Intent。

运行程序，在 FirstActivity 界面上点击按钮就可以看到打开了系统浏览器，如下图所示：

![使用系统浏览器 Chrome 打开网页](https://s2.loli.net/2024/01/11/HMVeqyBUdaZ6pnJ.png)

上面的代码中，调用了 Intent 的 `setData()` 方法，这个方法接收一个 Uri 对象，用于指定当前 Intent 操作的数据，这些数据通常是以字符串形式传入 `Uri.parse()` 方法中解析产生。
与此对应，还可以在 `<intent-filter>` 标签中再配置一个 `<data>` 标签，用于更精确地指定当前 Activity 能响应的数据。`<data>` 标签中可以配置以下内容：

| **属性** | **说明** |
| --- | --- |
| `android:schema` | 用于指定数据的协议，比如上面示例中的 "https" 部分 |
| `android:host` | 用于指定数据的主机名，比如上面示例中的 "www.baidu.com" 部分 |
| `android:port` | 用于指定数据的端口，一般紧跟在主机名之后 |
| `android:path` | 用于指定主机名和端口之后的部分，如一段网址中跟在域名之后的内容 |
| `android:mimeType` | 用于指定可以处理的数据类型，可以使用通配符来指定 |

只有当 `<data>` 标签中指定的内容和 Intent 中携带的 Data 完全一致时，当前 Activity 才能够响应这个 Intent。不过，`<data>` 标签中一般不会指定过多的内容。例如在上面的浏览器示例中，其实只需要指定 android:scheme 为 https，就可以响应所有 https 协议的 Intent 了。
下面来创建一个 Activity，使它也能响应打开网页的 Intent。
我们创建一个 BrowserActivity，其中有个 WebView 控件，WebView 是系统提供的用于在 App 内展示网页的控件，后续的文章会学习它的用法，这里只需要了解它的作用即可。
```kotlin
class BrowserActivity: AppCompatActivity() {

    private lateinit var webView: WebView

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_browser)
        webView = findViewById(R.id.webview)
        webView.webViewClient = WebViewClient()
        val uri = intent.data
        if (uri == null) {
            finish()
        }
        webView.loadUrl(uri.toString())
    }
}
```
```kotlin
// activity_browser.xml
<?xml version="1.0" encoding="utf-8"?>
<FrameLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
    <WebView
        android:id="@+id/webview"
        android:layout_width="match_parent"
        android:layout_height="match_parent" />
</FrameLayout>
```
在 AndroidManifest.xml 中设置 BrowserActivity 的 IntentFilter。
```xml
<activity android:name=".chapter3.BrowserActivity" android:exported="true">
  <intent-filter>
    <action android:name="android.intent.action.VIEW" />
    <category android:name="android.intent.category.BROWSABLE" />
    <category android:name="android.intent.category.DEFAULT" />
    <data android:scheme="https" android:host="www.baidu.com"/>
    <data android:scheme="http" />
  </intent-filter>
</activity>
```
在 FirstActivity 中按钮点击事件的代码没有变：
```xml
button4.setOnClickListener {
    val intent = Intent(Intent.ACTION_VIEW)
    intent.data = Uri.parse("https://www.baidu.com")
    startActivity(intent)
}
```
点击按钮后，系统弹出了一个列表，显示了目前能响应这个 Intent 的所有应用。选择 「Open with WebView Shell（在模拟器上）」会在系统浏览器中打开，如果选择「AndroidFirstLineOfCode」就是我们自己的应用，会使用 BrowserActivity 打开。Just once 表示只是这次使用选择的应用打开，Always 表示以后都使用选择的应用打开。用 BrowserActivity 打开网页的结果如下图所示。

![image.png](https://s2.loli.net/2024/01/11/zoG6MmZAFYsJHUK.png)
![使用 BrowserActivity 打开网页](https://s2.loli.net/2024/01/11/ZI1FlaUQ465GAKJ.png)

除了 `https` 协议外，还可以指定很多其他的协议，比如 `geo` 表示地理位置，`tel` 表示拨打电话。下面代码展示了如何在应用中调起系统拨号界面：
```kotlin
button.setOnClickListener {
    val intent = Intent(Intent.ACTION_DIAL)
    intent.data = Uri.parse("tel:10086")
    startActivity(intent)
}
```
上面代码首先制定了 Intent 的 action 是 `Intent.ACTION_DIAL`，这也是一个 Android 系统内置的 Action，然后在 data 部分指定了协议是 `tel`，号码是 10086。运行结果如图所示：

![使用隐式 Intent 打开系统拨号界面](https://s2.loli.net/2024/01/11/T6ZODx4KUhoR9ql.png)

### 向下一个 Activity 传递数据
Intent 不仅可以用于启动 Activity，还可以在启动 Activity 时传递数据。
Intent 提供了一系列 `putExtra()` 方法，只需要把想要传给下一个 Activity 的数据存到 Intent 中，在下一个 Activity 中从 Intent 将数据取出即可，数据在 Intent 中以键值对的形式存储。
比如 FristActivity 中有个字符串数据需要传到 SecondActivity 中，可以这么写：
```kotlin
button.setOnClickListener {
    val data = "Hello SecondActivity"
    val intent = Intent(this, SecondActivity::class.java)
    intent.putExtra("extra_data", data)
    startActivity(intent)
}
```
`putExtra()` 方法需要给数据一个字符串类型的 key，用于数据存储和读取。
在 SecondActivity 中可以这么接收数据：
```kotlin
class SecondActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?)
    setContentView(R.layout.activity_second)
    val extraData = intent.getStringExtra("extra_data")
    Log.d("SecondActivity", "extra data is $extraData")
}
```
这里的 `intent` 实际是 `getIntent()` 方法，可以获取启动这个 Activity 的 Intent，然后通过 `getXXXExtra()` 传入 key 取出对应的数据。
Intent 中支持的数据类型有：

- 基本数据类型（boolean、char、byte、short、int、long、float、double）
- 基本数据类型的数组（boolean[]、char[]、byte[]、short[]、int[]、long[]、float[]、double[]）
- String、CharSequence 及 String、CharSequence  的数组和 List（String[]、CharSequence[]、ArraryList<String>、ArraryList<CharSequence>）
- Serializable（序列化） 类型
- Parcelable（包裹化）类型和 Parcelable 的数组（Parcelable[]）和 List（ArrayList<Parcelable>）

每种数据类型都有对应的 putExtra 和 getXXXExtra 方法。
### 返回数据给上一个 Activity
除了从上一个 Activity 传递数据给下一个 Activity，还可以从下一个 Activity 返回数据给上一个 Activity。
如果上一个 Activity 希望接收下一个 Activity 返回的结果，不能使用 `startActivity()` 来启动下一个 Activity，而是需要使用 `startActivityForResult()` 方法，它接受两个参数，第一个参数还是 Intent，第二个参数是请求码，用于在之后的回调中判断数据的来源。看下示例代码：
```kotlin
button.setOnClickListener {
    val intent = Intent(this, SecondActivity::class.java)
    startActivityForResult(intent, 1)
}
```
请求码只要是个唯一值即可，这里传入了 1。
接下来看看 SecondActivity 中如何返回数据：
```kotlin
class SecondActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?)
    setContentView(R.layout.activity_second)
    button.setOnClickListener {
        val intent = Intent()
        intent.putExtra("return_data", "Hello FirstActivity")
        setResult(RESULT_OK, intent)
        finish()
    }
}
```
上面代码中在按钮的点击事件中，构建了一个 Intent，这个 Intent 只用来传递数据，它没有指定任何的「意图」。把要返回的数据放在 Intent 中，然后调用 `setResult()` 方法，这个方法专门用于向上一个 Activity 返回数据。它接收两个参数，第一个参数是结果码，一般是 `RESULT_OK` 和 `RESULT_CANCELED` 两个值，第二个参数则是带有数据的 Intent。最后调用了 `finish()` 方法。
如果是点击返回键来返回上一个 Activity，想要传递数据的话，就需要重写 `onBackPressed()` 方法，在这个方法中传入要返回的数据。
```kotlin
class SecondActivity : AppCompatActivity() {
    override fun onBackPressed() {
        val intent = Intent()
        intent.putExtra("return_data", "Hello FirstActivity")
        setResult(RESULT_OK, intent)
        finish()
    }
}
```

使用 `startActivityForResult()` 启动下个 Activity，在下一个 Activity 销毁后，上一个 Activity 会回调 `onActivityResult()` 方法，需要重写这个方法来获取返回的数据。它有三个参数：

- 第一个参数是 `requestCode`，对应 `startActivityForResult()` 方法中的 requestCode，因为在一个 Activity 中可能调用 `startActivityForResult()` 启动多个不同的 Activity，每个 Activity 返回的数据都会回调到这个方法中，所以需要通过 requestCode 判断数据来源。
- 第二个参数是 `resultCode`，对应下一个 Activity 调用 `setResult()` 设置的结果码，用于判断处理结果是否成功。
- 第三个参数 data 是携带了数据的 Intent，可以通过 `getXXXExtra()` 从 Intent 中取出对应的数据。

示例代码如下：
```kotlin
class FirstActivity : AppCompatActivity() {
    override fun onActivityResult(requestCode: Int, resultCode: Int, data: Intent?) {
        super.onActivityResult(requestCode, resultCode, data)
        when (requestCode) {
            1 -> if (resultCode == RESULT_OK) {
                val returnData = data?.getStringExtra("return_data")
                Log.d("FirstActivity", "returned data is $returnData")
            }
        }
    }
}
```
## Activity 生命周期
Activity 有生命周期，给开发者提供了前面提到的 `onCreate()` 等生命周期回调，掌握 Activity 生命周期可以让我们能对 Activity 的使用有更深的理解。
### 返回栈
Activity 是可以重叠的，每启动一个新的 Activity，就会覆盖在原来的 Activity 上，点击返回键则会销毁最上面的 Activity，下面一个 Activity 会重新显示出来。这是一种很典型的栈结构。
Android 使用任务（task）来管理 Activity，一个任务就是一组放在栈里的 Activity 集合，这个栈被称为返回栈（back stack）。栈是一种后进先出的数据结构，默认情况下，每当启动一个新的 Activity，它会被压入返回栈中，并处在栈顶位置。每当按下返回键，栈顶的 Activity 会出栈，前一个入栈的 Activity 会重新处于栈顶的位置，系统总是会显示处于栈顶的 Activity 给用户。

![yuque_mind.jpeg](https://s2.loli.net/2024/01/11/KRC67t4vZ2E1TUn.jpg)

### Activity 状态
每个 Activity 在它生命周期中可能有四种状态：

- **运行状态（RESUMED）**

当一个 Activity 处于返回栈的栈顶并且正在展示给用户、与用户交互，这个 Activity 就处于运行状态。此时 Activity 不会被系统回收。

- **暂停状态（STARTED）**

当一个 Activity 不再处于栈顶但仍然可见时，Activity 处于暂停状态。比如一个 Activity 打开了另一个对话框形式的 Activity 或者一个半透明的 Activity。暂停状态的 Activity 仍然完全存活，只有在内存极低的情况下，才可能会被系统回收。

- **停止状态（CREATED）**

当一个 Activity 不再处于栈顶并且完全不可见时，Activity 处于停止状态。系统仍然会为这种 Activity 保存相应的状态和成员变量，当内存不足时，停止状态的 Activity 可能会被系统回收。

- **销毁状态（DESTROYED）**

当一个 Activity 从返回栈中移除，比如点击返回键或调用 `finish()` 方法，Activity 会变成销毁状态。销毁状态的 Activity 会被系统回收。
> 注意：这四种状态和 AndroidX 中 Lifecycle 定义的 `RESUMED`、`STARTED`、`CREATED`、`DESTROYED` 是一一对应的。除此之外，Lifecycle 中还有个 `INITIALIZED` 状态，表示 Activity 刚被创建，Activity 的创建是系统实现的，这个状态一般开发者获取不到。关于 Lifecycle 的知识，后面的文章会进行讲解。

### Activity 生命周期回调
Activity 类中定义了 7 个生命周期的回调方法，开发者可以在这些生命周期方法中做一些业务逻辑，比如在 `onCreate()` 中初始化控件，在 `onDestroy()` 中释放资源等。

| **生命周期回调** | **说明** |
| --- | --- |
| `onCreate()` | 这个方法会在 Activity 第一次被创建时调用。应该在这个方法中完成 Activity 的初始化，比如加载布局，初始化控件，绑定事件等。 |
| `onStart()` | 这个方法在 Activity 由不可见变为可见的时候调用。 |
| `onResume()` | 这个方法在 Activity 准备好和用户交互时调用。 |
| `onPause()` | 这个方法在系统准备启动或恢复另一个 Activity 时调用。通常会在这个方法中奖一些消耗 CPU 的资源释放掉，以及保存一些关键数据。但这个方法的执行速度一定要快，否则会影响新的 Activity 的使用。 |
| `onStop()` | 这个方法在 Activity 完全不可见时调用，它和 `onPause()` 方法的主要区别是，如果启动的新 Activity 是个对话框式的 Activity（不是完全不透明），那 `onPause()` 方法会被执行，`onStop()` 不会执行。 |
| `onDestroy()` | 这个方法在 Activity 被销毁前调用，之后 Activity 的状态变为销毁状态。 |
| `onRestart()` | 这个方法在 Activity 由停止状态变为运行状态之前调用，也就是 Activity 被重新启动了。 |

上面 7 个方法除了 `onRestart()` 方法，其他是两两相对的，所以又可以将 Activity 分为以下 3 种生存期：

- **完整生存期**：Activity 在 `onCreate()` 方法和 `onDestroy()` 方法之间所经历的就是完整生存期。一般情况下，一个 Activity 会在 `onCreate()` 方法中完成各种初始化操作，在 `onDestroy()` 方法中完成释放内存的操作。
- **可见生存期**：Activity 在 `onStart()` 方法和 `onStop()` 方法之间所经历的就是可见生存期。在可见生存期内，Activity 对于用户是可见的，但无法和用户进行交互。可以通过这两个方法合理地管理那些对用户可见的资源。比如在 `onStart()` 方法中对资源进行加载，而在 `onStop()` 方法中对资源进行释放，从而保证处于停止状态的 Activity 不会占用过多内存。
- **前台生存期**：Activity 在 `onResume()` 方法和 `onPause()` 方法之间所经历的就是前台生存期。在前台生存期内，Activity 总是处于运行状态，可以和用户进行交互。

官方提供了一张 Activity 生命周期的示意图，如图所示：

![Activity 生命周期](https://s2.loli.net/2024/01/11/2x6pdfE3O4JFuHR.png)

下面通过示例代码来体验 Activity 的生命周期。
我们创建三个 Activity，MainActivity、NormalActivity 和 DialogActivity，其中 MainActivity 是应用的入口 Activity，NormalActivity 是个普通的全屏 Activity，DialogActivity 是一个主题为 `Theme.AppCompat.Dialog` 的对话框式的 Activity。MainActivity 中有两个按钮，一个用于跳转 NormalActivity，一个用于跳转 DialogActivity。我们在每个 Activity 的 7 个生命周期回调方法中加入 Log，方便观察生命周期的变化。
MainActivity 代码：
```kotlin
class MainActivity : AppCompatActivity() {
    private val tag = "MainActivity"

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        Log.d(tag, "onCreate")
        setContentView(R.layout.activity_main)

        val button1 = findViewById(R.id.button1)
        button1.setOnClickListener {
            startActivity(Intent(this, NormalActivity::class.java))
        }
        val button2 = findViewById(R.id.button2)
        button2.setOnClickListener {
            startActivity(Intent(this, DialogActivity::class.java))
        }
    }

    override fun onStart() {
        super.onStart()
        Log.d(tag, "onStart")
    }

    override fun onResume() {
        super.onResume()
        Log.d(tag, "onResume")
    }

    override fun onPause() {
        super.onPause()
        Log.d(tag, "onPause")
    }

    override fun onStop() {
        super.onStop()
        Log.d(tag, "onStop")
    }

    override fun onDestroy() {
        super.onDestroy()
        Log.d(tag, "onDestroy")
    }

    override fun onRestart() {
        super.onRestart()
        Log.d(tag, "onRestart")
    }
}
```
activity_main.xml：
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
    <Button
        android:id="@+id/button1"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Open NormalActivity" />
    <Button
        android:id="@+id/button2"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Open DialogActivity" />
</LinearLayout>
```
NormalActivity 的代码：
```kotlin
class NormalActivity: AppCompatActivity() {

    private val tag = "NormalActivity"

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        Log.d(tag, "onCreate")
        setContentView(R.layout.activity_normal)
    }

    override fun onStart() {
        super.onStart()
        Log.d(tag, "onStart")
    }

    override fun onResume() {
        super.onResume()
        Log.d(tag, "onResume")
    }

    override fun onPause() {
        super.onPause()
        Log.d(tag, "onPause")
    }

    override fun onStop() {
        super.onStop()
        Log.d(tag, "onStop")
    }

    override fun onDestroy() {
        super.onDestroy()
        Log.d(tag, "onDestroy")
    }

    override fun onRestart() {
        super.onRestart()
        Log.d(tag, "onRestart")
    }
}
```
activity_normal.xml：
```kotlin
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:gravity="center">
    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="This is a normal Activity" />
</LinearLayout>
```
DialogActivity 的代码：
```kotlin
class DialogActivity: AppCompatActivity() {

    private val tag = "DialogActivity"

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        Log.d(tag, "onCreate")
        setContentView(R.layout.activity_dialog)
    }

    override fun onStart() {
        super.onStart()
        Log.d(tag, "onStart")
    }

    override fun onResume() {
        super.onResume()
        Log.d(tag, "onResume")
    }

    override fun onPause() {
        super.onPause()
        Log.d(tag, "onPause")
    }

    override fun onStop() {
        super.onStop()
        Log.d(tag, "onStop")
    }

    override fun onDestroy() {
        super.onDestroy()
        Log.d(tag, "onDestroy")
    }

    override fun onRestart() {
        super.onRestart()
        Log.d(tag, "onRestart")
    }
}
```
activity_dialog.xml：
```kotlin
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
    <TextView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="This is a Dialog Activity" />
</LinearLayout>
```
在 AndroidManifest 文件中指定 DialogActivity 的主题为 Dialog：
```xml
<activity
  android:name=".MainActivity"
  android:exported="true">
  <intent-filter>
    <action android:name="android.intent.action.MAIN" />
    <category android:name="android.intent.category.LAUNCHER" />
  </intent-filter>
</activity>
<activity android:name=".chapter3.NormalActivity" />
<activity
  android:name=".chapter3.DialogActivity"
  android:theme="@style/Theme.AppCompat.Dialog" />
```

- 当点击应用图标，打开 MainActivity，打印的日志如下：
```xml
MainActivity            onCreate
MainActivity            onStart
MainActivity            onResume
```
可以看到，当 MainActivity 被第一次创建时，会依次执行 `onCreate()`、`onStart()`、`onResume()` 方法。

- 当点击「Open NormalActivity」按钮时，打开 NormalActivity，打印的日志如下：
```xml
MainActivity            onPause
NormalActivity          onCreate
NormalActivity          onStart
NormalActivity          onResume
MainActivity            onStop
```
可以看到，对于 MainActivity，会依次调用 `onPause()`、`onStop()`方法，对于 NormalActivity，则会依次调用它的 `onCreate()`、`onStart()`、`onResume()`，这里需要注意的是，NormalActivity 的 `onCreate()` 在 MainActivity 的 `onPause()` 之后回调，这就是前面说到为什么 `onPause()` 中的逻辑必须轻量，执行速度必须快，否则会影响下一个 Activity 的展示。

- 当点击返回键，回到 MainActivity，打印的日志如下：
```xml
NormalActivity          onPause
MainActivity            onRestart
MainActivity            onStart
MainActivity            onResume
NormalActivity          onStop
NormalActivity          onDestroy
```
可以看到，从 NormalActivity 返回 MainActivity，会先回调 NormalActivity 的 `onPause()` 方法，再执行 MainActivity 的恢复逻辑，依次调用 MainActivity 的 `onRestart()`、`onStart()`、`onResume()`，然后执行 NormalActivity 的销毁流程，依次调用它的 `onStop()`、`onDestroy()`。

- 当点击「Open DialogActivity」，打开 Dialog 主题的 DialogActivity，打印的日志如下：
```xml
MainActivity            onPause
DialogActivity          onCreate
DialogActivity          onStart
DialogActivity          onResume
```
可以看到，与打开 NormalActivity 不同的是，MainActivity 只调用了 `onPause()` 方法，后面没有再调用 `onStop()` 方法。

- 当此时按下电源键，设备息屏，打印的日志如下：
```xml
DialogActivity          onPause
MainActivity            onStop
DialogActivity          onStop
```
可以看到这种情况下，DialogActivity 会依次调用 `onPause()`、`onStop()` 方法，而下层 MainActivity 也会从可见变为不可见，调用它的 `onStop()` 方法。

- 接着解锁设备，应用重新展示出来，打印的日志如下：
```xml
MainActivity            onRestart
MainActivity            onStart
DialogActivity          onRestart
DialogActivity          onStart
DialogActivity          onResume
```
重新亮屏时，MainActivity 会恢复到可见但不可交互的状态，调用 `onRestart()`、`onStart()`， DialogActivity 会恢复到前台，依次调用 `onRestart()`、`onStart()` 和 `onResume()`。

- 当点击返回键，返回 MainActivity，打印的日志如下：
```xml
DialogActivity          onPause
MainActivity            onResume
DialogActivity          onStop
DialogActivity          onDestroy
```
可以看到，从 DialogActivity 返回到 MainActivity 时，和 NormalActivity 返回的一个区别是 MainActivity 只会调用 `onResume()` 方法，DialogActivity 则会正常执行 `onPause()`、`onStop()`、`onDestroy()`。

- 当点击返回键，返回到桌面，打印的日志如下：
```xml
MainActivity            onPause
MainActivity            onStop
MainActivity            onDestroy
```
可以看到返回桌面时，MainActivity 会依次调用 `onPause()`、`onStop()` 和 `onDestroy()`，这意味着 MainActivity 走完了完整的销毁流程，从任务栈中移除。
这是 Android 12 之前系统的表现，如果是 Android 12 及以上的系统， 情况有所不同，打印的日志如下：
```xml
MainActivity            onPause
MainActivity            onStop
```
也就是说，从 Android 12 开始，在入口 Activity 点击返回键回到桌面，**不会 **调用 Activity 的 `onDestroy()` 方法，只会调用 `onPause()` 和 `onStop()`。如前面所说，入口 Activity 就是在 AndroidManifest 文件中配置了 IntentFilter action 为 MAIN，category 为 LAUNCHER 的 Activity，一般指 MainActivity。

### Activity 被异常回收
当一个 Activity 进入停止状态，内存不足时，可能会被系统回收。我们可以通过开启「开发者工具」中的「不保留活动」来验证这个情况。设想这样一个场景，有个 Activity A，打开了 Activity B，在不保留活动的情况下，Activity A 会被系统回收，此时按下返回键，返回 Activity A。Activity A 会重新创建，再次展示出来。
这个过程中打印的日志如下：
```xml
// MainActivity 打开 NormalActivity
MainActivity             onPause
NormalActivity           onCreate
NormalActivity           onStart
NormalActivity           onResume
MainActivity             onStop
MainActivity             onDestroy

// 点击返回键回到 MainActivity
NormalActivity           onPause
MainActivity             onCreate
MainActivity             onStart
MainActivity             onResume
NormalActivity           onStop
NormalActivity           onDestroy
```
可以看到，在不保留活动的情况下，打开 NormalActivity，MainActivity 除了会调用到 `onStop()` 外，还会调用 `onDestroy()`，这意味着 MainActivity 被系统回收了。而在返回到 MainActivity 时，MainActivity 重新执行了创建流程中的生命周期方法 `onCreate()`、`onStart()`、`onResume()`，这意味着 MainActivity 被系统重建了。
这个情况下，Activity 中的临时数据或状态，会因为被回收、重建而丢失，比如 Activity 中有个文本输入框，输入了一段文字，在重建后这些文字就没了，可能会影响用户体验。所以有些关键的数据和状态，需要能在回收重建的场景下保存。Activity 提供了 `onSaveInstanceState()` 回调方法，这个方法可以保证在 Activity 被回收前一定被调用，可以在这个方法中保存一些数据。 
`onSaveInstanceState()` 方法有个 Bundle 类型的参数 `outState`，可以调用 outState 的 `putXXX()` 方法以键值对的形式保存数据，比如使用 `putString()` 方法保存字符串，使用 `putInt()` 方法保存整型数据。
```kotlin
override fun onSaveInstanceState(outState: Bundle) {
    super.onSaveInstanceState(outState)
    val tempData = "Text user inputed"
    outState.putString("data_key", tempData)
}
```
现在我们把想要保存的数据保存下来了，那如何恢复数据呢？`onCreate()`方法中有个 Bundle? 类型的参数 `savedInstanceState`，这个参数在正常启动 Activity 时为 null，如果在 Activity 被回收前通过 onSaveInstanceState 保存了数据，Activity 被重建时，会带有之前保存的所有数据，可以通过对应的取值方法 `getXXX()` 传入对应的 key 来取出保存的数据。
```kotlin
override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    setContentView(R.layout.activity_main)
    if (savedInstanceState != null) {
        val tempData = savedInstanceState.getString("data_key")
        Log.d(tag, "tempData is: $tempData")
    }
}
```
另外，系统配置变更也会导致 Activity 的重建，比如最常见的横竖屏切换场景。这种情况下，Activity 中的数据也会丢失，这个问题也可以通过 `onSaveInstanceState()` 方法来解决。但 Jetpack 提供了 ViewModel 来更优雅地解决这个问题，Jetpack 和 ViewModel 的相关知识会在后面的文章进行讲解。

## Activity 启动模式
除了 Activity 的生命周期，Activity 的启动模式也是一个非常重要的话题，实际项目中应该根据特定的需求来为每个 Activity 指定合适的启动模式。Activity 的启动模式一共有 4 种，分别是：`standard`、`singleTop`、`singleTask` 和 `singleInstance`。可以在 AndroidManifest.xml 中给 `<activity>` 标签指定 `android:launchMode` 属性来指定启动模式。下面挨个进行介绍：

### standard
standard（标准模式）是 Activity 默认的启动模式，在不显式指定的情况下，都会用这个模式。在 standard 模式下，每当启动一个新的 Activity，它会在返回栈中入栈，并处于栈顶。不管这个 Activity 是否已经在返回栈中存在，都会创建一个新的 Activity 实例。
看下图中的三种情况：

- 图中左边 ①：初始情况下，任务栈中有 A -> B -> C 三个 Activity，然后启动 D，D Activity 不在返回栈中，会创建一个 D Activity 的实例并压入返回栈中。
- 图中中间 ②：初始情况下，任务栈中有 A -> B -> C 三个 Activity，然后启动 C，C Activity 已经处于返回栈栈顶，但还是会创建一个新的 C Activity 的实例并压入返回栈中。
- 图中右边 ③：初始情况下，任务栈中有 A -> B -> C 三个 Activity，然后启动 B，B Activity 已经处于返回栈中，但不在栈顶，还是会创建一个新的 B Activity 的实例并压入返回栈中。

![standard](https://s2.loli.net/2024/01/11/FaAiSNHZzLCdXyb.jpg)

### singleTop
singleTop 被称为「栈顶复用」模式。如果某个 Activity 的启动模式是 singleTop，当启动这个 Activity 时，如果这个 Activity 已经处于返回栈的栈顶，则会复用这个 Activity 实例并回调 `onNewIntent()` 方法，不会创建新的 Activity 实例。如果这个 Activity 已经处于返回栈中，但不在栈顶，那还是会创建新的 Activity 实例并入栈。
看下图中的三种情况：

- 图中左边 ①：初始情况下，任务栈中有 A -> B -> C 三个 Activity，然后启动 D，D Activity 不在返回栈中，会创建一个 D Activity 的实例并压入返回栈中。
- 图中中间 ②：初始情况下，任务栈中有 A -> B -> C 三个 Activity，然后启动 C，C Activity 已经处于返回栈栈顶，此时 **不会创建新的 C Activity 的实例，而是直接复用栈顶 C Activity 的实例，回调它的 `onNewIntent()` 方法**。
- 图中右边 ③：初始情况下，任务栈中有 A -> B -> C 三个 Activity，然后启动 B，B Activity 已经处于返回栈中，但不在栈顶，还是 **会创建一个新的 B Activity 的实例并压入返回栈中**。

![singleTop](https://s2.loli.net/2024/01/11/Brc8g4tjToP6iIF.jpg)

### singleTask
singleTask 被称为「栈内复用」模式。如果某个 Activity 的启动模式是 singleTask，当启动这个 Activity 时，如果这个 Activity 已经处于返回栈中，则会复用这个 Activity 实例并回调 `onNewIntent()` 方法，不会创建新的 Activity 实例。同时，如果这个 Activity 不在栈顶，还会把这个 Activity 之上的所有 Activity 出栈（clearTop）。
看下图中的三种情况：

- 图中左边 ①：初始情况下，任务栈中有 A -> B -> C 三个 Activity，然后启动 D，D Activity 不在返回栈中，会创建一个 D Activity 的实例并压入返回栈中。
- 图中中间 ②：初始情况下，任务栈中有 A -> B -> C 三个 Activity，然后启动 C，C Activity 已经处于返回栈栈顶，此时 **不会创建新的 C Activity 的实例，而是直接复用 C Activity 的实例，回调它的 `onNewIntent()` 方法**。
- 图中右边 ③：初始情况下，任务栈中有 A -> B -> C 三个 Activity，然后启动 B，B Activity 已经处于返回栈中且不在栈顶，此时 **不会创建新的 C Activity 的实例，而是直接复用 C Activity 的实例，回调它的 `onNewIntent()` 方法，并且将 B 之上的 C 出栈**。

![singleTask](https://s2.loli.net/2024/01/11/fwpqtsRakNjJzK7.jpg)

### singleInstance
singleInstance 启动模式是四种启动模式中最特殊、最复杂的一个。如果某个 Activity 的启动模式是 singleInstance，当启动这个 Activity 时，会启用一个新的返回栈来管理这个 Activity。
singleInstance 的使用场景通常和跨应用启动 Activity 有关，假设应用中有个 Activity 允许其他应用调用，如果想实现其他应用和我们自己的应用可以共享这个 Activity 实例，那前三种启动模式是做不到的，因为每个应用都有自己的返回栈，同一个 Activity 在不同的返回栈中入栈时肯定创建了不同的示例。而 singleInstance 则可以解决这个问题，在这种模式下，有一个单独的返回栈来管理这个 Activity，所有应用来访问这个 Activity，都共用同一个返回栈，就解决了共享 Activity 实例的问题。
> 如果 singleTask 模式指定了不同的 taskAffinity，也会启用一个新的返回栈。关于 taskAffinity，在这里不详细展开。

看下图的过程来理解下 singleInstance 启动模式：

- ① 是初始状态，返回栈 1 中有个 A Activity，在 A 之上启动 B，B 的启动模式是 singleInstance，所以会启用一个新的返回栈来管理 B，B 被压入返回栈 2 中，此时 B 展示给用户，是前台 Activity。
-  在 ② 中，在 B 之上又启动了一个 C，C 的启动模式是 standard，它不会加到 B 所在的返回栈 2，而是会加入到 A 所在的返回栈 1，此时 C 展示给用户，是前台 Activity。
- 在 ③ 中，按下返回键，此时会发现，并不是返回到 B，而是返回到了 A，也就是说此时前台 Activity 是 A。这是因为 A 和 C 处于同一个返回栈中，当按下返回键时，C 出栈，A 成为返回栈 1 的栈顶 Activity 展示给用户。
- 在 ④ 中，再次按下返回键，会回到 B，B 成为前台 Activity 展示给用户。这是因为当 A 出栈时，返回栈 1 空了，就展示另一个返回栈的栈顶 Activity，也就是返回栈 2 中的 B。

![singleInstance](https://s2.loli.net/2024/01/11/m9egI3hZ8qX1KY6.jpg)

## 参考资料

- [《Android 第一行代码（第三版）》郭霖著](https://www.ituring.com.cn/book/2744)
- [有关 Android12 中 Activity 生命周期的变化](https://blog.csdn.net/vitaviva/article/details/125074388)
