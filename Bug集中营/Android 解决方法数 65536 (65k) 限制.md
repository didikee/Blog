## Android 解决方法数 65536 (65k) 限制 ##

#### 可能出现的错误信息: ####

> **`Conversion to Dalvik format failed: Unable to execute dex: method ID not in [0, 0xffff]: 65536`**

> **说明:这个方法是谷歌提供的.链接如下:**
> **[https://developer.android.com/studio/build/multidex.html](https://developer.android.com/studio/build/multidex.html)**


#### 解决步骤: ####

**1.步骤1:**

	android { compileSdkVersion 21 buildToolsVersion "21.1.0"
	
	defaultConfig {
	    ...
	    minSdkVersion 14
	    targetSdkVersion 21
	    ...
	
	    // Enabling multidex support.
	    multiDexEnabled true
	}
	...
	}
	
	dependencies { compile 'com.android.support:multidex:1.0.0' }

**2.步骤2:**

让应用支持多DEX文件。在MultiDexApplication JavaDoc中描述了三种可选方法：

1.在AndroidManifest.xml的application中声明

	android.support.multidex.MultiDexApplication；
2.如果你已经有自己的Application类，让其继承MultiDexApplication；

3.如果你的Application类已经继承自其它类，你不想/能修改它，那么可以重写attachBaseContext()方法：

	@Override 
	protected void attachBaseContext(Context base) {
	    super.attachBaseContext(base); MultiDex.install(this);
	}

#### 附言: ####

Multidex仍有一些限制：

1. DEX文件安装到设备的过程非常复杂，如果第二个DEX文件太大，可能导致应用无响应。此时应该使用ProGuard减小DEX文件的大小。

2. 由于Dalvik linearAlloc的Bug，应用可能无法在Android 4.0之前的版本启动，如果你的应用要支持这些版本就要多执行测试。
 
3. 同样因为Dalvik linearAlloc的限制，如果请求大量内存可能导致崩溃。Dalvik linearAlloc是一个固定大小的缓冲区。在应用的安装过程中，系统会运行一个名为dexopt的程序为该应用在当前机型中运行做准备。dexopt使用LinearAlloc来存储应用的方法信息。Android 2.2和2.3的缓冲区只有5MB，Android 4.x提高到了8MB或16MB。当方法数量过多导致超出缓冲区大小时，会造成dexopt崩溃。
Multidex构建工具还不支持指定哪些类必须包含在首个DEX文件中，因此可能会导致某些类库（例如某个类库需要从原生代码访问Java代码）无法使用。

