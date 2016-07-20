## ScrollView 里的 EditText 与输入法的用例 ##

> 情景是这样的:
> 
> 1. 我希望页面可以滚动,因为长页面,内容多,必须滚动来满足不同手机的显示
> 2. 点击 **EditText** 输入法弹出来,并将布局顶起来,并且**EditText有足够的显示空间**
> 3. 进入页面时,输入法不能自动弹出来.

#### 解决方案: ####
	1. 用 Scrollview 满足情景里的第一个需求
	2. 使用android:windowSoftInputMode="adjustResize"让布局被顶高,满足第二个需求
	3. 在Activity里配置

	<activity
            android:name="com.didikee.Test"
            android:configChanges="keyboard|keyboardHidden"
            android:windowSoftInputMode="stateAlwaysHidden|adjustResize" >
        </activity>
	
	stateAlwaysHidden 与 adjustResize 连用即可实现.

#### 会出现的bug ####

> 输入法弹出来后,取消输入法,输入法的会"占位"(即占用之前位置,可能显示白色或者黑色)

**解决:**

	Scrollview 的宽高 和父Layout的宽高设为 "match_parent"代替 "wrap_content"即可.

**原因:**

	布局里面的如果有listview或者类似的控件,软键盘上移的时候在adjustResize模式下会把根layout的高度修改
	成软键盘之上，这时列表会被挤上去，但是软键盘还原后layout的高度还原的时候列表高度不会改变，就留白了
	这时候把你的layout的高度设置成 match_parent 就好了.
