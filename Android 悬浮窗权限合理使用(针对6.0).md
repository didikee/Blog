## Android 悬浮窗权限合理使用(针对6.0) ##

#### Android 悬浮窗权限 ####

> 关于Android 6.0 申明`SYSTEM_ALERT_WINDOW`时默认拒绝的应对方案


	<!-- 悬浮窗口 所需权限 -->
    <!-- Android 4.4及以下用此权限-->
    <uses-permission android:name="android.permission.SYSTEM_ALERT_WINDOW"/>
    <!--Android 4.4以上用此权限-->
    <uses-permission android:name="android.permission.SYSTEM_OVERLAY_WINDOW" />

#### 在代码中的应用 ####





