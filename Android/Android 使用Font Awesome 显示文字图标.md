# Android 使用Font Awesome 显示文字图标 #

> 简单几步就可以完成

简单的效果图:

![](http://oahzrw11n.bkt.clouddn.com//pic/20160720/font.png)

## 1. 创建 assets 文件夹 ##

在Android Studio 上的创建步骤为:

在 `src/main`上右键 --> `New` --> `Folder` --> `Assets Folder`.

将FontAwesome 字体文件copy到assets指定的路径,这里我放在`assets/font/fontawesome-webfont.ttf`.

## 2. 编写资源文件与代码 ##

#### /values/strings.xml ####

	<string name="fa_car">&#xf1b9;</string>
    <string name="fa_apple">&#xf179;</string>
    <string name="fa_android">&#xf17b;</string>

#### activity_layout.xml ####

	//...
	<TextView
        android:id="@+id/tv_1"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="@string/fa_car"
        android:textSize="20sp"
        android:textColor="@color/cardview_shadow_start_color"
        />
    <TextView
        android:id="@+id/tv_2"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="@string/fa_apple"
        android:textSize="24sp"
        android:textColor="@color/colorPrimaryDark"
        />
    <TextView
        android:id="@+id/tv_3"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="@string/fa_android"
        android:textSize="48sp"
        android:textColor="@color/colorAccent"
        />
	//...

#### Activity类 ####

		TextView tv_1 = (TextView)findViewById(R.id.tv_1);
        TextView tv_2 = (TextView)findViewById(R.id.tv_2);
        TextView tv_3 = (TextView)findViewById(R.id.tv_3);

        //获取assets文件夹里的字体文件
        Typeface font = Typeface.createFromAsset(getAssets(), "font/fontawesome-webfont.ttf");

        //给指定的TextView加载字体
        tv_1.setTypeface(font);
        tv_2.setTypeface(font);
        tv_3.setTypeface(font);

## 3. 最后附上字体下载链接 ##

字体:**[http://fontawesome.io/](http://fontawesome.io/)**

对照表:**[http://fontawesome.io/cheatsheet/](http://fontawesome.io/cheatsheet/)**

