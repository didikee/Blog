# SharedPreferences.Editor 的apply()与commit()方法的区别 #

## commit()的文档  ##


官方文档如下:

	Commit your preferences changes back from this Editor to the SharedPreferences object it is editing. 
	This atomically performs the requested modifications, replacing whatever is currently in the SharedPreferences.
	
	Note that when two editors are modifying preferences at the same time, the last one to call commit wins.
	
	If you don't care about the return value and you're using this from your application's main thread,
	consider using apply() instead.


## apply()的文档 ##


官方文档如下:

	Commit your preferences changes back from this Editor to the SharedPreferences object it is editing.
	This atomically performs the requested modifications, replacing whatever is currently in the SharedPreferences.
	
	Note that when two editors are modifying preferences at the same time, the last one to call apply wins.
	
	Unlike commit(), which writes its preferences out to persistent storage synchronously, apply() commits its 
	changes to the in-memory SharedPreferences immediately but starts an asynchronous commit to disk and you won't
	be notified of any failures. If another editor on this SharedPreferences does a regular commit() while a apply() 
	is still outstanding, the commit() will block until all async commits are completed as well as the commit itself.
	
	As SharedPreferences instances are singletons within a process, it's safe to replace any instance of commit()
	with apply() if you were already ignoring the return value.
	
	You don't need to worry about Android component lifecycles and their interaction with apply() writing to disk.
	The framework makes sure in-flight disk writes from apply() complete before switching states.


## 解释说明 ##


1. 需要注意的是`commit()`方法是`
Added in API level 1`的,也就是`sdk1`就已经存在了.
2. `apply()`方法是`Added in API level 9`的.
3. `commit()`有返回值,成功返回`true`,失败返回`false`.`commit()`方法是**同步**提交到硬件磁盘，因此，在多个并发的提交commit的时候，他们会等待正在处理的commit保存到磁盘后在操作，从而降低了效率。
4. `apply()`没有返回值.`apply()`是将修改的数据提交到内存, 而后**异步**真正的提交到硬件磁盘.

## 为什么建议使用apply()替代commit() ? ##

答:**因为Android的设计人员发现,开发人员对commit的返回值不感兴趣,而且在数据并发处理时使用commit要比apply效率低,所以推荐使用apply.**


