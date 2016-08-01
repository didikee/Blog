## 自定义 checkbox 新玩法 ? ##

#### 第一步:selector ####

> 编写 **`drawable/selector_checkbox_voice.xml`**

	<?xml version="1.0" encoding="utf-8"?>
	<selector xmlns:android="http://schemas.android.com/apk/res/android">
	    <item android:drawable="@drawable/ic_voice_off" android:state_checked="true"/>
	    <item android:drawable="@drawable/ic_voice_on" android:state_checked="false"/>
	    <item android:drawable="@drawable/ic_voice_off"/>
	</selector>

#### 第二步:style ####

> **`VoiceCheckboxTheme`**

	<!--自定义的checkbox-->
    <style name="VoiceCheckboxTheme" parent="@android:style/Widget.CompoundButton.CheckBox">
        <item name="android:button">@drawable/selector_checkbox_voice</item>
    </style>

#### 第三步:布局文件里 ####

	<CheckBox
        android:id="@+id/cb_voice"
        style="@style/VoiceCheckboxTheme" //这里使用
        android:layout_width="@dimen/dp21"
        android:layout_height="@dimen/dp28"
        android:gravity="center"
        android:layout_marginLeft="@dimen/dp30"
        />

#### 第四步:效果 ####

> **看左边第二个**

//点击前
![](pic/checkbox_1.png)
//点击后
![](pic/checkbox_2.png)

你可以监听状态:

	//语音是否关闭
        mCb_Voice.setOnCheckedChangeListener(new CompoundButton.OnCheckedChangeListener() {
            @Override
            public void onCheckedChanged(CompoundButton buttonView, boolean isChecked) {
                if (isChecked){
                    //执行关闭语音
                    MGToast.showToast("执行关闭语音");
                }else {
                    //执行开启语音
                    MGToast.showToast("执行开启语音");
                }
            }
        });