# Kotlin 协程

## 前言

全网能把协程讲明白的，很少，非常少，因为协程对于 Java、Android 工程师来说，还是个新概念，所以想要学好协程，第一步是要理清楚：协程的本质是什么？协程的出现是为了解决什么痛点？有哪些缺点？非它不可吗？RxJava 不香了吗？第二步，上手尝试下协程，是否满足业务场景？有没有什么注意事项？最后，使用协程代替 RxJava，看看性能对比效率对比。

## 资料

[Kotlin 的协程用力瞥一眼 - 学不会协程？很可能因为你看过的教程都是错的](https://kaixue.io/kotlin-coroutines-1/)

[Kotlin 协程的挂起好神奇好难懂？今天我把它的皮给扒了](https://kaixue.io/kotlin-coroutines-2/)

[到底什么是「非阻塞式」挂起？协程真的更轻量级吗？](https://kaixue.io/kotlin-coroutines-3/)

> 【推荐】扔物线朱凯的码上开学系列协程教程，有视频有文章，把协程讲的很透，也很好懂。
> 摘录一下最关键的点：

```
Q: 协程是什么？
A: Kotlin 中的协程本质上是基于线程来实现的一种更上层的工具 API，类似于 Java 自带的 Executor 系列 API 或者 Android 的 Handler 系列 API。Kotlin 协程的底层还是线程。

Q: 挂起是什么？
A: 挂起就是可以自动切回来的切线程

Q: 挂起的非阻塞式是怎么回事?
A: 挂起的非阻塞式指的是它能用看起来阻塞的代码写出非阻塞的操作，就这么简单
```

[Kotlin协程Coroutines入门到实战：（一）理解异步回调的本质](https://blog.csdn.net/NJP_NJP/article/details/103513537)

[Kotlin协程Coroutines入门到实战：（二）Coroutines初体验](https://blog.csdn.net/NJP_NJP/article/details/103513719)

[Kotlin协程Coroutines入门到实战：（三）Coroutines+Retrofit+ViewModel+LiveData实现MVVM客户端架构](https://blog.csdn.net/NJP_NJP/article/details/103524778)

> 【推荐】或许我们很少去思考，为什么我们需要回调？回调的目的到底是什么？有没有其他方式来代替实现这个目的呢？作者从异步回调的本质说起，引出了协程的优势，再来个 MVVM 实战把协程的使用吃透，非常优秀的三系列文。

[Kotlin Coroutines 101 - Android Conference Talks](https://www.youtube.com/watch?v=ZTDXo0-SKuU&t=8s)

> 【推荐】Google 工程师讲解协程的用法，介绍了协程解决什么问题、如何创建和取消协程、如何捕获异常、如何测试协程等知识，非常全面。

## 练习

[Use Kotlin Coroutines in your Android App](https://developer.android.com/codelabs/kotlin-coroutines)

> 【推荐】Google 官方出品的协程 Codelab，其中也包含很多知识点，边学边练。
