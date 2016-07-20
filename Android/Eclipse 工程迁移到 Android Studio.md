## Eclipse 工程迁移到 Android Studio ##

> 目标:迁移成功,并成功正常运行!
> 
> 附加:同步视频在文章后面!

#### 两种方式: ####

**1. 用Gradle导出,在Android Studio中用Gradle导入 (不推荐)**

**2. 用Android Studio 直接导入Eclipse 工程 (推荐)**

> 我以第二种方式.

#### 步骤: ####

1. Eclipse 工程(**主工程**+**依赖的第三方库**)
2. 导入**主工程**(依赖的库不用理会,Studio会自动导入的)
3. 导入后等待**build**(可能会比较慢,推荐SSD)
4. 会出现很多的错误,慢慢排查,一个一个的看日志
5. Error1:重复的资源文件(String),可以全局搜索看看是不是重复了,AS是不让重复定义资源的.
6. Error2:注解**@null**依赖错误,添加`compile 'com.android.support:support-annotations:19.1.0'`
7. ok,正常运行了.
8. Error附加:清单文件报错:
	在根标签的命名空间后添加`xmlns:tools="http://schemas.android.com/tools"`


		<application
		android:name="com.fanwe.app.App"
        android:allowBackup="true"
        android:icon="@drawable/icon"
        android:label="@string/app_name"
        android:theme="@style/FanweTheme"
        tools:replace="icon, label,theme">//Android Studio会引导你使用tools标签来进行设置:tools:replace="icon, label,theme"

#### 结束: ####
其实第二种方式还是很简单的,没什么特别要注意的地方,对了,还有一个文件需要注意,因为有些文件AS没有复制过来而是**忽略**了他们.

在`import-summary.txt`中有`Ignored Files:`里面注明具体哪一类型的文件会被Android Studio忽略.

有篇blog对这个进行了说明,我也放在文章后面:

[Android Studio 忽略文件解释说明-博客园](http://www.cnblogs.com/ct2011/p/4183553.html)

本文章同步的视频操作录像下载地址:[点击下载](http://oahzrw11n.bkt.clouddn.com/vedio/20160718/Eclipse%20%E5%B7%A5%E7%A8%8B%E8%BF%81%E7%A7%BB%E5%88%B0%20Android%20Studio.mp4)
//完了=.=
