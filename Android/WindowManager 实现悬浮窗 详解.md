# WindowManager 实现悬浮窗 详解 #

> 一:对于想直接看效果的,可以看看我的demo app.
> 
> **链接:[http://sj.qq.com/myapp/detail.htm?apkName=com.inno.backdot](http://sj.qq.com/myapp/detail.htm?apkName=com.inno.backdot)**
> 
> **源码:[https://github.com/didikee/BackDot](https://github.com/didikee/BackDot)**

> 二: Android 6.0 关于`SYSTEM_ALERT_WINDOW`权限申明直接报错
> 
	// 设置window type 
        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.KITKAT) {
            mWinParams.type = WindowManager.LayoutParams.TYPE_TOAST;
        } else {
            mWinParams.type = WindowManager.LayoutParams.TYPE_PHONE;
        }
	//原因1:type为"TYPE_TOAST"在sdk19之前不接收事件,之后可以.
	//原因12:type为"TYPE_PHONE"需要"SYSTEM_ALERT_WINDOW"权限.在sdk19之前不可以直接申明使用,之后不能直接申明使用.

> 三:用到的技术知识点:
>
	1. OnTouch()的事件处理
	2. WindowManager类及其LayoutParams的常见属性的理解
	3. Handler更新UI
	4. 定时器(Timer + TimerTask)


## 1. OnTouch事件处理 ##

这个网上的资料很多,这里说一些注意点:

1.获取坐标

* `event.getRawX()`:获取相对屏幕的坐标X(获取Y的坐标同理)
* `event.getX()`:获取相对于容器的坐标X(获取Y的坐标同理)

2.返回值

* `return true`:表示事件不往下传递了
* `return false`:表示继续传递事件

## 2. WindowManager类 ##

获取方式:

    mWmManager = (WindowManager) context.getSystemService(Context.WINDOW_SERVICE);


### WindowManager.LayoutParams类 ###

	this.mWinParams = new WindowManager.LayoutParams();
		// 设置图片格式，效果为背景透明
        mWinParams.format = PixelFormat.RGBA_8888;
        // 设置浮动窗口不可聚焦（实现操作除浮动窗口外的其他可见窗口的操作）
        mWinParams.flags = WindowManager.LayoutParams.FLAG_NOT_FOCUSABLE;
        // 调整悬浮窗显示的停靠位置为左侧置�?
        mWinParams.gravity= Gravity.LEFT | Gravity.TOP;
        mScreenHeight = mWmManager.getDefaultDisplay().getHeight();

        // 以屏幕左上角为原点，设置x、y初始值，相对于gravity
        mWinParams.x = mScreenWidth/4;
        mWinParams.y = mScreenHeight/4;

        // 设置悬浮窗口长宽数据
        mWinParams.width = FrameLayout.LayoutParams.WRAP_CONTENT;
        mWinParams.height = FrameLayout.LayoutParams.WRAP_CONTENT;

其中需要注意的是其**`Gravity`**属性:

注意:**Gravity不是说你添加到WindowManager中的View相对屏幕的几种放置,而是说你可以设置你的 参 考 系 !**

例如:`mWinParams.gravity= Gravity.LEFT | Gravity.TOP;`意思是以屏幕左上角为参考系,那么屏幕左上角的坐标就是(0,0),这是你后面摆放View位置的唯一依据.当你设置为`mWinParams.gravity = Gravity.CENTER;`那么你的屏幕中心为参考系,坐标(0,0).一般我们用屏幕左上角为参考系.

#### 设置WindowManager中的View的透明度 ####

使用:`LayoutParams.alpha`属性(0.0f ~ 1.0f),1.0f不透明,0.0f全透明,源码如下:

		/**
         * An alpha value to apply to this entire window.
         * An alpha of 1.0 means fully opaque and 0.0 means fully transparent
         */
        public float alpha = 1.0f;

#### Handler更新UI(略) ####
#### 定时器 ####
	
	TimerTask timerTask = new TimerTask(){其实就是一个Runnable};
	看他的类:
	public abstract class TimerTask implements Runnable{...}

	Timer mtimer=new Timer();

	使用的时候:
	mtimer.schedule(timerTask,0,3);//参数1:执行的任务;参数2:延迟0毫米执行;参数3:每隔3毫秒执行一次任务;




