# Android笔记----Android 开发进阶 从小工到专家 #

## 第 1 章  四大组件 ##

#### 1.1 Activity ####

**生命周期:**

- **`onCreate()`**:通常完成Activity的**`初始化`**操作.(设置布局,初始化试图,绑定事件)
- **`onStart()`**:Activity还是`不可见`.
- **`onResume()`**:Activity请求`AMS`渲染它所管理的试图.
- **`onPause()`**:通常会在这个回调里将一些消耗CPU的`资源释放`掉,以及`保存`一些关键数据.
- **`onStop()`**:Activity此时`完全不可见`.
- **`onDestroy()`**:(`略`)
- **`onRestart()`**:(`略`)

**Activity的构成**

    Activity --> PhoneWindow --> DecorView -->Default Layout -->ViewGroup(mContentParent) --> 用户自己的xml布局

**Activity 的 4 种启动模式**

1. `standard`:标准模式.
2. `SingleTop`:实例如果在**`顶端`**,那么**`复用`**而不是重新创建.否则,和`standard`模式一样.
3. `SingleTask`:一个**`任务桟`**只能有一个该Activity的**`实例`**,没有会创建,有的话就复用,情景:`A --> B --> C --> D --> E --> A`那么 A之间的Activity都会被销毁,跳转到最后的A时会回调该Activity的**`onNewIntent()`**函数.
4. `SingleInstance`:会开启一个新的`任务桟`,并且只会有`一个实例`,如果已经存在,那么跳转时会触发该Activity的**`onNewIntent()`**函数.

#### 1.2 Service ####

1. 普通`service`.
2. `IntentService`:在任务执行完毕之后IntentService会调用`stopSelf`自我销毁.(适用于完成一些短期的耗时任务)用法是直接变成继承`IntentService`.
3. AIDL:略.

#### 1.3 Broadcast ####

1. 普通广播
2. 有序广播
3. 本地广播:`LocalBroadcastManager`
4. sticky广播:sticky广播通过`Context.sendStickyBroadcast()`函数发送,用此函数发送的广播会一直`滞留`,当有匹配此广播的接收器注册后,该广播接收器就会接收到此条广播.使用时需要`BROADCAST_STICKY`权限.需要注意的是,`sendStickyBroadcast()`只保留最后一条广播,并且一直保持下去,这样即使已经有广播接收器处理了该广播,当再有匹配的广播接收器注册时,此广播仍会被接收.当然只想处理一次可以在处理调用`removeStickyBroadcast()`函数移除广播.

#### 1.4 ContentProvider (略) ####

## 第 2 章 View与动画 ##

#### 2.1 View ####
1. Listview 与Gridview
2. RecyclerView
3. ViewPager
4. 自定义**View**
5. 自定义**ViewGroup**

#### 2.2 Scroller 的使用 ####

//TODO p46

#### 2.3 动画 ####

1. 帧动画:`AnimationDrawable`
2. 补间动画:(`tween`动画)`Animation`,`AlphaAnimation`,`ScaleAnimation`,`TranslateAnimation`,`RotateAnimation`,`Interpolator`.
3. 属性动画:`ValueAnimator`,`ObjectAnimator`(重点),`AnimatorSet`,`TypeEvaluator`,`TimeInterpolator`
4. **`Xfermode`**:对两个View的效果处理,很棒很实用,建议了解或者学习.

## 第 3 章 ##

#### 3.1 多线程 ####

1. **Handler**
2. **Looper**
3. **MessageQueue**
4. Thread
5. Runnable
6. **AsyncTask**:Android异步任务,有兴趣的可以看看`Rxjava`,在Android上使用需要其拓展框架`RxAndroid`,基于观察者的异步.

## 第 4 章 ##

HTTP

## 第 5 章 ##
SQLite

## 第 6 章 ##
#### 性能优化 ####

#### 6.1 布局优化 ####

1. `include`标签:复用Layout.
2. `merge`标签:消除无用的节点,减少嵌套,从而减少View的绘制与计算,达到优化的目的.
3. `ViewStub`标签:一个**不可见**的和能**在运行期间延迟加载**目标视图的**宽高**都为**0**的View.
4. 五种基本布局的合理使用,在使用Linearlayout的`weight`属性时可以尝试使用RelativeLayout等等.

#### 6.2 内存优化 ####

1. 珍惜Service资源:不要**贪婪**地使得一个Service持续保留.
2. 当UI隐藏时需要释放内存:`onTrimMemory()`当内存不足时回调.
3. 当内存紧张时释放部分内存.(补充)
4. 避免bitmap的浪费
5. 使用优化的数据容器:使用Android Framework里面优化过的容器类,例如 `SparseArray`,`SparseBooleanArray`,`LongSparseArray`.通常使用HashMap会消耗更多的内存.
6. 注意内存的开销:例如,`Enums`的内存消耗通常是`static constants`的 **2** 倍.
7. 注意代码"抽象":如果**抽象**没有显著的提升效率,应该尽量避免使用它,因为更多的代码会map到内存中.
8. 为序列化的数据使用`nano protobufs`:`Protocol buffers`是由Google为序列化结构数据而设计的.
9. 避免使用依赖注入:框架通过扫描你的代码会执行许多的初始化操作,这会导致你的代码需要的RAM来map代码.
10. 谨慎使用外部库:很多library并不是专为移动平台设计的.
11. lint的使用.
12. 使用`ProGuard`来剔除不需要的代码.
13. 对最终的APK使用zipalign:Android studio 默认是会执行的.
14. 合理使用多进程.

#### 6.3 内存泄漏 ####

1. Memory Monitor: Android studio 自带的内存分析工具.
2. LeakCanary:Square开源的第三方库`LeakCanary`.

#### 6.4 性能优化 ####

1. 过度绘制:手机开发者模式(Developer Options)--> 调试GPU过度绘制 (Debug GPU Overdraw) -->显示过度绘制区域 (show overdraw area).
2. 图形渲染:`Hierarchy Viewer`,在Android studio的"tools"-->"Android"-->"Android Device Monitor" 

## 第 7 章 ##
#### 代码规范 ####

## 第 8 章 ##
#### Git 版本控制 ####

学习通过**GitHub**学习和参与开源项目.
#### 附加 GitHub windows 客户端的安装 ####

## 第 9 章 ##
#### 单元测试 ####

## 第 10 章 ##

#### 10.1 面向对象六大设计原则 ####
1. 单一职责原则
2. 里氏替换原则
3. 依赖倒置原则
4. 开闭原则
5. 接口隔离原则
6. 迪米特原则

#### 10.2 设计模式 ####

1. MVC:
2. MVP:
3. MVVM:

## 第 11 章 ##
#### 重构 ####











