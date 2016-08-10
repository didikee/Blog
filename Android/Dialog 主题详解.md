## Dialog 主题详解 ##

> **原文注释**
> 
>  Default theme for dialog windows and activities (on API level 10 and lower),
   which is used by the
     {@link android.app.Dialog} class.  This changes the window to be
     floating (not fill the entire screen), and puts a frame around its
     contents.  You can set this theme on an activity if you would like to
     make an activity that looks like a Dialog. 

> （在API级别10 以及以下）dialog和 Activity 默认使用的主题. 
     "floating":浮动式（不填满整个屏幕） ，并有一个框架围绕dialog内容。
	 您可以设置这个主题上的活动，如果你想使看起来像一个对话的活动。


    <style name="Theme.Dialog">
    //Dialog的windowFrame框为无
    <item name="windowFrame">@null</item> 
    //设置dialog标题的样式
    	--<item name="maxLines">1</item> //单行显示
    	--<item name="scrollHorizontally">true</item> //水平方向
    	--<item name="textAppearance">@style/TextAppearance.DialogWindowTitle</item> //里面只有一个属性:<item name="textSize">18sp</item> 字体大小
    <item name="windowTitleStyle">@style/DialogWindowTitle</item>
    //设置dialog的背景
    <item name="windowBackground">@drawable/panel_background</item>
    //是否浮现在activity之上
    <item name="windowIsFloating">true</item>
    //content上的覆盖的一层drawable(一般为null,不需要覆盖什么)
    <item name="windowContentOverlay">@null</item>
    //默认的dialog进场与出场动画
    	--<item name="windowEnterAnimation">@anim/dialog_enter</item> //进场动画
    	--<item name="windowExitAnimation">@anim/dialog_exit</item> //出场动画
    <item name="windowAnimationStyle">@style/Animation.Dialog</item>
    //输入法模式
    <item name="windowSoftInputMode">stateUnspecified|adjustPan</item>
    //点击窗体外是否关闭,boolean值,true表示点击关闭;false表示点击不关闭;
    	--<bool name="config_closeDialogWhenTouchOutside">true</bool> //默认点击关闭(dialog消失)
    <item name="windowCloseOnTouchOutside">@bool/config_closeDialogWhenTouchOutside</item>
    //弹出时覆盖部分布局  若 false则不符盖，将原有布局下移。
   	[在Activity中会复杂些,参见这篇blog:http://blog.csdn.net/zhaoweixing1989/article/details/7712085]
    <item name="windowActionModeOverlay">true</item>
    //-----------------------------------------------------

	//背景缓存颜色
    <item name="colorBackgroundCacheHint">@null</item>
    //文字样式
	--<style name="TextAppearance">
        <item name="textColor">?textColorPrimary</item> //文字颜色
        <item name="textColorHighlight">?textColorHighlight</item>
        <item name="textColorHint">?textColorHint</item> //hint(提示)文字颜色
        <item name="textColorLink">?textColorLink</item> //链接颜色
        <item name="textSize">16sp</item> //文字字体大小
        <item name="textStyle">normal</item> //字体样式(普通normal,粗体bold,斜体italic)
    --</style>
    <item name="textAppearance">@style/TextAppearance</item>
	//
    <item name="textAppearanceInverse">@style/TextAppearance.Inverse</item>
    
    <item name="textColorPrimary">@color/primary_text_dark</item>
    <item name="textColorSecondary">@color/secondary_text_dark</item>
    <item name="textColorTertiary">@color/tertiary_text_dark</item>
    <item name="textColorPrimaryInverse">@color/primary_text_light</item>
    <item name="textColorSecondaryInverse">@color/secondary_text_light</item>
    <item name="textColorTertiaryInverse">@color/tertiary_text_light</item>
    <item name="textColorPrimaryDisableOnly">@color/primary_text_dark_disable_only</item>
    <item name="textColorPrimaryInverseDisableOnly">@color/primary_text_light_disable_only</item>
    <item name="textColorPrimaryNoDisable">@color/primary_text_dark_nodisable</item>
    <item name="textColorSecondaryNoDisable">@color/secondary_text_dark_nodisable</item>
    <item name="textColorPrimaryInverseNoDisable">@color/primary_text_light_nodisable</item>
    <item name="textColorSecondaryInverseNoDisable">@color/secondary_text_light_nodisable</item>
    <item name="textColorHint">@color/hint_foreground_dark</item>
    <item name="textColorHintInverse">@color/hint_foreground_light</item>
    <item name="textColorSearchUrl">@color/search_url_text</item>
    
    <item name="textAppearanceLarge">@style/TextAppearance.Large</item>
    <item name="textAppearanceMedium">@style/TextAppearance.Medium</item>
    <item name="textAppearanceSmall">@style/TextAppearance.Small</item>
    <item name="textAppearanceLargeInverse">@style/TextAppearance.Large.Inverse</item>
    <item name="textAppearanceMediumInverse">@style/TextAppearance.Medium.Inverse</item>
    <item name="textAppearanceSmallInverse">@style/TextAppearance.Small.Inverse</item>
    
    <item name="listPreferredItemPaddingLeft">10dip</item>
    <item name="listPreferredItemPaddingRight">10dip</item>
    <item name="listPreferredItemPaddingStart">10dip</item>
    <item name="listPreferredItemPaddingEnd">10dip</item>
    
    <item name="preferencePanelStyle">@style/PreferencePanel.Dialog</item>
    </style>