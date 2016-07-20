## 状态栏 a.getBoolean(1, false) 报错 ##

这个错误在编译运行时候并不会出现，但是当需要**编译打包**的时候，就会报出这个异常。

	TypedArray a = mContext.obtainStyledAttributes(attrs);
	boolean hasBottomLine = a.getBoolean(0, false);
	boolean hasTopLine = a.getBoolean(1, false);//AS会在"1"下显示错误红线.

## 解决方案: ##
在该方法上添加` @SuppressWarnings("ResourceType") `，这样即可过滤该警告，可以正常通过签名编译。

参考:**[http://stackoverflow.com/questions/26360954/obtainstyledattributes-annotated-with-styleableres-suppress-warnings](http://stackoverflow.com/questions/26360954/obtainstyledattributes-annotated-with-styleableres-suppress-warnings)**


