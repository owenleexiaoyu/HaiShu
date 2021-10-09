# SeaTree

一个普通 Android 开发的成长路线。

中文名为海树。取自《笠翁对韵》中的 “山花对海树，赤日对苍穹。”做一山花易，成为海树难。需将树根扎到深深的海底，需面对无穷的海浪，需抵挡鱼虾的骚扰。。。编程一路也是如此。

### Android 基础
- Android 第一行代码
    - Android 系统架构
    - Android 版本变更
    - 搭建开发环境
    - 运行 Hello World 项目
    - 分析 Android 程序结构
- 编程语言基础
    - Java 语法基础
    - Kotlin 语法基础
- 看的见的组件—Activity
    - Activity 是什么
    - Activity 的基本用法
    - 使用 Intent 在 Activity 之间传递数据
    - Activity 的生命周期
    - Activity 的启动模式
- 基础 UI 控件与布局
    - 如何编写 Android 程序界面
    - 基础控件（TextView、Button、EditText、ImageView、ProgressBar、AlertDialog）
    - 基础布局（FrameLayout、LinearLayout、RelativeLayout、ConstraintLayout）
    - 列表控件（ListView、RecyclerView、ViewPager）
    - 其他控件（Toast、ScrollView、CheckBox、RadioButton、TimePicker、Spinner等）
- 界面复用 — Fragment
    - Fragment 是什么
    - Fragment 使用方式
    - Fragment 生命周期
- 广播接收器 — BroadcastReceiver
    - 介绍与分类
    - 接受广播（静态注册和动态注册）
    - 发送广播
- 数据持久化
    - 文件存储 file
    - SharedPreferences 存储键值对 sp
    - 数据库存储 sql
- 跨程序共享数据的组件 ContentProvider
    - Android 权限、动态权限申请
    - ContentResolver 访问其他程序数据
    - 创建 ContentProvider
- 多媒体操作
    - 通知 Notification 的用法
    - 调用摄像头和相册
    - 播放多媒体文件（音频、视频）
- 后台服务 Service
    - Android 异步编程
    - Handler 消息机制
    - AsyncTask 用法
    - Service 简介
    - Service 基本用法
    - Service 生命周期
    - 前台 Service 和 IntentService
- 网络请求
    - WebView 用法
    - HttpUrlConnection
    - OkHttp 入门使用
    - 解析 Json（Xml、Pb等）
    - 使用 Retrofit
- Material Design
    - Material Design 介绍与设计理念
    - Toolbar、DrawerLayout、NavigationView
    - FloatingActionButton、SnackBar、CoordinatorLayout
    - MaterialCardView
    - AppBarLayout
    - SwipeRefreshLayout
    - CollapsingToolbarLayout

### Android进阶
- Activity 深入理解
    - 生命周期
    - 启动模式
    - IntentFilter 匹配规则
- View 工作原理和自定义 View
    - Activity、Windows、View 的关系
    - Window 和 WindowManager
    - View 体系
    - Android 坐标系
    - View 的滑动
    - 理解 MeasureSpec
    - View 的 measure、layout、draw 的过程
    - View 事件分发机制
    - 自定义 View
    - 动画
    - Drawable
- Android 多线程与异步
    - 线程基础
    - 同步
    - 阻塞队列
    - 线程池
    - Handler 消息机制分析（Handler、MessageQueue、Looper）
    - ThreadLocal 工作原理
    - AsyncTask 原理
    - 主线程消息循环
- 四大组件工作过程
    - 四大组件运行状态
    - Activity 工作过程
    - Service 工作过程
    - BroadcastReceiver 工作过程
    - ContentProvider 工作过程
- 图片、Bitmap 和 Cache
    - 图片加载框架 - Glide、Fresco
    - Bitmap 高效加载
    - Android 缓存策略（LruCache、DiskLruCache）
    - ImageLoader 实现
- Android 资源管理与适配
    - 多分辨率适配
    - AssetManager
    - 资源加载机制
    - Style 和 Theme
    - 状态栏导航栏适配
    - 屏幕适配
- 网络进阶
    - 计算机网络基础（详见计算机网络专题）
        - 网络分层
        - TCP 与 UDP
        - Http 与 Https
    - OkHttp
        - OkHttp 使用
        - 拦截器（责任链模式）
        - 超时重传、重定向
        - Http 缓存
        - Socket 连接池复用
    - Volley 使用和经典设计思想
    - Retrofit
        - Retrofit 使用
        - 动态代理
        - 运行时注解
    - Gson 框架
- 事件总线 -EventBus
    - EventBus 使用
    - EventBus 理解原理
- 响应式编程
    - RxJava
        - RxJava 基本使用
        - RxJava 操作符
        - RxJava 线程切换
        - Flowable 背压
        - RxJava + OkHttp + Retrofit
        - RxJava 实现事件总线 RxBus
        - RxJava 原理理解
        - RxAndroid
    - Kotlin Flow
- 注解与依赖注入框架
    - 注解
    - 依赖注入原理
    - ButterKnife 使用和原理
    - Dagger2 使用和原理
    - Koin 使用和原理（Kotlin）
    - Hilt 使用和原理
- 应用架构模式
    - MVC
    - MVP
    - MVVM
    - Android Architecture Components
    - 几种架构模式的横向比较
- JetPack
    - 简介和分类
    - navigation
    - Lifecycle
    - LiveData
    - ViewModel
    - WorkManager
    - DataStore
    - Startup
    - Room
- Gradle 和 Groovy
    - Groovy 语言基础
    - Gradle 插件
- 虚拟机
    - JVM
    - Dalvik
    - ART
- 动态化技术
    - Hook 技术
        - 四大组件的 Hook
    - 热修复
        - ClassLoader
        - 代码插桩
        - 热修复技术原理
        - 热修复框架
    - 插件化
        - 插件化原理
        - 四大组件的插件化
        - 资源的插件化
        - so 的插件化
        - 插件化框架学习
    - Hybrid开发
        - WebView(H5)
        - RN
        - Flutter + Dart
        - JSBridge
- 性能优化
    - View 绘制优化
    - 布局优化，过度绘制优化
    - 内存优化
    - 响应速度优化与 ANR 分析
    - 线程优化
    - OOM 治理
    - 卡顿检测和优化
    - Crash 收集分析
    - 启动加速
    - 埋点监控
    - APM
    - 包体积优化
    - 电量优化
    - 网络优化
- 设计模式
    - 大话设计模式（详见设计模式专题）
    - Android 源码中的设计模式应用
- 组件化
    - 组件化思想
    - 组件化架构设计
    - ARouter 框架
- JNI 和 NDK
    - 调用 JNI 方法
    - 回调 Java 方法
- Android 音视频开发
    - 详见音视频专题
- 打包与持续集成
    - Jenkins
    - 多渠道打包
    - Android 打包流程
- 安全
    - root 原理
    - 经典漏洞研究
    - 逆向

### Kotlin 语法
- 基础语法
    - 基础数据结构
    - 类与对象
    - 分支、循环
    - 函数
    - 空安全
    - 接口
    - 数据类、密封类、枚举类
    - 作用域函数
    - 异常
    - IO 操作
- 委托
- 泛型
- 高阶函数
- Kotlin DSL
- 注解
- 反射
- Kotlin 协程
- Kotlin JS
- Kotlin Server 开发
- Kotlin Native
- Kotlin Mutiplatform
- Kotlin Flow

### Java 进阶


### Git
- Git 基础用法
    - git init
    - git add
    - git commit
    - git log
    - git pull
    - git push
- Git 分支
    - 创建分支
    - 切换分支
    - 分支合并
    - 分支变基
    - 删除分支
- Git 版本回退
- 工作区、暂存区理解
- Git 高级用法

### Android 音视频

### Flutter

### Android 专家

### 设计模式
- 六大原则
    - 单一职责
    - 开放封闭
    - 里式替换
    - 最少知识
    - 接口隔离
    - 依赖倒置

- 创建型设计模式
    - 工厂方法模式
    - 抽象工厂模式
    - 单例模式
    - 建造者模式
    - 原型模式

- 结构型设计模式
    - 适配器模式
    - 装饰器模式
    - 代理模式
    - 外观模式
    - 桥接模式
    - 组合模式
    - 享元模式

- 行为型设计模式
    - 策略模式
    - 模板方法模式
    - 观察者模式
    - 迭代子模式
    - 责任链模式
    - 命令模式
    - 备忘录模式
    - 状态模式
    - 访问者模式
    - 中介者模式
    - 解释器模式

### 其他领域开发
- 其他领域编程语言
    - Python
    - Dart
    - Groovy
    - JavaScript
- 其他领域开发
    - 服务端开发
    - 前端开发
    - 机器学习

### 数据结构与算法
- 数据结构
    - 线性表
    - 树
    - 图
    - 散列表
- 算法
    - 查找算法
    - 排序算法
    - 分治
    - 动态规划
### 计算机网络

### 操作系统

### 编译原理
