## Android EditText悬浮在输入法之上 ##

> **使用 ` android:windowSoftInputMode="adjustResize" ` 会让界面整体被顶上去,很多时候我们不需要这样的情况出现,这里给出另一个方案.**

> **思路:监听输入法的状态,然后动态的滚动 ` EditText ` 所在的 ` ViewGroup ` 或者` View ` **

#### 1. Android Manifest.xml ####

	<activity
            android:name=".InputActivity"
            android:theme="@style/Theme.AppCompat.Light.NoActionBar"
            android:configChanges="keyboard|keyboardHidden|orientation"
            android:screenOrientation="portrait"
            android:windowSoftInputMode="adjustPan"> //非adjustResize
     </activity>

#### 2. 布局文件 ####

	<RelativeLayout
	    xmlns:android="http://schemas.android.com/apk/res/android"
	    xmlns:tools="http://schemas.android.com/tools"
	    android:id="@+id/root"
	    android:layout_width="match_parent"
	    android:layout_height="match_parent"
	    android:background="@drawable/bg_saber_q"
	    tools:context="didikee.com.demoapk.InputActivity">
	    <LinearLayout
	        android:id="@+id/rl_inputdlg_view"
	        android:layout_width="match_parent"
	        android:layout_height="wrap_content"
	        android:layout_alignParentBottom="true"
	        android:background="#00000000"
	        android:orientation="horizontal">
	
	        <EditText
	            android:id="@+id/input_message"
	            android:layout_width="0dp"
	            android:layout_height="@dimen/dp30"
	            android:layout_gravity="center|left"
	            android:layout_marginLeft="@dimen/dp12"
	            android:paddingLeft="@dimen/dp2"
	            android:paddingRight="@dimen/dp2"
	            android:layout_weight="1"
	            android:background="@drawable/shape_cricle_gray_solid_white"
	            android:focusable="true"
	            android:focusableInTouchMode="true"
	            android:gravity="left"
	            android:imeOptions="actionSend"
	            android:maxLength="32"
	            android:singleLine="true"
	            android:text=""
	            android:textColor="@color/dark_text"
	            android:textSize="20sp"/>
	
	        <TextView
	            android:id="@+id/confrim_btn"
	            android:layout_width="@dimen/dp50"
	            android:layout_height="@dimen/dp30"
	            android:layout_gravity="center|right"
	            android:layout_marginLeft="@dimen/dp11"
	            android:layout_marginRight="@dimen/dp12"
	            android:background="@drawable/shape_cricle_solid_orange"
	            android:gravity="center"
	            android:text="发送"
	            android:textColor="@color/white"/>
	    </LinearLayout>
	
	</RelativeLayout>

#### 3. Activity里设置监听,滚动 input 视图 ####

	public class InputActivity extends AppCompatActivity {
	
	    private RelativeLayout rLayout;
	    private View mInputLayout;
	
	    @Override
	    protected void onCreate(Bundle savedInstanceState) {
	        super.onCreate(savedInstanceState);
	        setContentView(R.layout.activity_input);
	        mInputLayout = findViewById(R.id.rl_inputdlg_view);
	
	        rLayout = ((RelativeLayout) findViewById(R.id.root));
	        //输入法到底部的间距(按需求设置)
	        final int paddingBottom = DisplayUtil.dp2px(this, 5);
	
	        rLayout.getViewTreeObserver().addOnGlobalLayoutListener(new ViewTreeObserver.OnGlobalLayoutListener() {
	
	            @Override
	            public void onGlobalLayout() {
	                Rect r = new Rect();
	                rLayout.getWindowVisibleDisplayFrame(r);
	                //r.top 是状态栏高度
	                int screenHeight = rLayout.getRootView().getHeight();
	                int softHeight = screenHeight - r.bottom ;
	                Log.e("test","screenHeight:"+screenHeight);
	                Log.e("test","top:"+r.top);
	                Log.e("test","bottom:"+r.bottom);
	                Log.e("test", "Size: " + softHeight);
	                if (softHeight>100){//当输入法高度大于100判定为输入法打开了
	                    rLayout.scrollTo(0, softHeight+paddingBottom);
	                }else {//否则判断为输入法隐藏了
	                    rLayout.scrollTo(0, paddingBottom);
	                }
	            }
	        });
	    }
	}

#### 效果图: ####

**1. 不做处理前:**
![](http://oahzrw11n.bkt.clouddn.com//pic/20160804input_2.png)

**2. 处理后的效果:**
![](http://oahzrw11n.bkt.clouddn.com//pic/20160804input_3.png)

#### 补充: ####

> 在我的手机上(三星s7)输入法的高度在1000px左右(我的输入法高度是1040px),理论上你只要把这个值放在 0~~1040直接就可以检测到输入法的展开和隐藏.

> 在代码中写的尺寸都是**px**单位的,100px也只有50dp不到,一般输入法的高度不可能比它小.当然你不喜欢可以设其他的,但是不宜过大.=.=

