## Android RatingBar 自定义样式 ##

#### 1.先定义Style: ####

	<style name="RadingStyle" parent="@android:style/Widget.RatingBar">
        <!-- 定义星星图片 -->
        <item name="android:progressDrawable">@drawable/layer_live_rating_bar</item>
        <!-- 根据自定义星星图片的大小,设置相应的值,否则可能显示不全 -->
        <item name="android:numColumns">5</item>
		//这里放一些你觉得公共的属性(你可以在控件里覆盖这里的属性)
    </style>

#### 2. Drawable里的layer_live_rating_bar.xml: ####

	<?xml version="1.0" encoding="utf-8"?>
	<layer-list xmlns:android="http://schemas.android.com/apk/res/android">
	    <item
	        android:id="@+android:id/background"
	        android:drawable="@drawable/ic_rate_stroke">
	    </item>
	    <item
	        android:id="@+android:id/secondaryProgress"
	        android:drawable="@drawable/ic_rate_stroke">
	    </item>
	    <item
	        android:id="@+android:id/progress"
	        android:drawable="@drawable/ic_rate_solid">
	    </item>
	
	</layer-list>

#### 3. 在布局文件里使用RatingBar: ####

	//........
	<RatingBar
        android:id="@+id/rb"
        style="@style/RadingStyle"
        android:layout_width="wrap_content"//宽度一般都是自适应吧
        android:layout_height="wrap_content"
        android:maxHeight="15dp"//两个都写就能限制高度
        android:minHeight="15dp"//两个都写就能限制高度
        android:rating="3"//默认的评分
        android:stepSize="0.5"//评分最小单位
        android:clickable="true"
        android:isIndicator="false"//是否只是展示,展示就不可点击
        />
	//........

#### 4. 最终效果: ####
![](pic/rate.png)

#### 5.注意点: ####

**这两个属性同时写才能确定高度,不知道还有没其他方式**

	android:maxHeight="15dp"//两个都写就能限制高度
    android:minHeight="15dp"//两个都写就能限制高度

**`isIndicator`属性确定评分条是否可以点击评分,false就是只是展示而已**

	android:isIndicator="false"//是否只是展示,展示就不可点击