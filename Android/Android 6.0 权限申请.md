## Android 6.0 权限申请 ##

#### 1. 以前的权限申请(sdk<23) ####

直接在`AndroidManifest.xml`中申明即可:

	<uses-permission android:name="android.permission.READ_PHONE_STATE"></uses-permission>
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"></uses-permission>

> **`但是,Android sdk>=23(6.0)`申明的权限是直接被拒绝的.需要我们在`运行时`去申请!**

#### 2. 运行时权限申请 ####

	void checkPermission() {
        final List<String> permissionsList = new ArrayList<>();
        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.M) {
            if ((checkSelfPermission(Manifest.permission.READ_PHONE_STATE) != PackageManager.PERMISSION_GRANTED))
                permissionsList.add(Manifest.permission.READ_PHONE_STATE);
            if ((checkSelfPermission(Manifest.permission.WRITE_EXTERNAL_STORAGE) != PackageManager.PERMISSION_GRANTED))
                permissionsList.add(Manifest.permission.WRITE_EXTERNAL_STORAGE);
            if (permissionsList.size() != 0) {
                requestPermissions(permissionsList.toArray(new String[permissionsList.size()]),
                        100);
            }else {
                //已经不是第一次,已经有权限
                Log.e("test","permissionsList.size()==0");
            }
        }
    }

这个`checkPermission()`是`Activity`的方法.

> **所以在Application中是无法申请的,一些第三方库尽量避免在Application中初始化时调用`危险权限`!**

#### 3. 回调 ####

	private int count=0;
    @TargetApi(Build.VERSION_CODES.M)
    @Override
    public void onRequestPermissionsResult(int requestCode, @NonNull String[] permissions,
                                           @NonNull int[] grantResults) {
        count++;
        Log.e("test","onRequestPermissionsResult:"+count);
        if (requestCode==100){
            if (grantResults.length>0 && (checkSelfPermission(Manifest.permission.READ_PHONE_STATE) == PackageManager.PERMISSION_GRANTED) && (checkSelfPermission(Manifest.permission.WRITE_EXTERNAL_STORAGE) == PackageManager.PERMISSION_GRANTED)){
                Toast.makeText(this, "成功", Toast.LENGTH_SHORT).show();
            }else {

            }
        }
    }

在logcat中:


	08-03 20:09:20.545 16735-16735/didikee.com.demoapk E/test: onRequestPermissionsResult:1

> 结论:**回调只会走一次!**

当我们再次运行程序时,logcat中日志:

	08-03 20:10:14.165 16735-16735/didikee.com.demoapk E/test: permissionsList.size()==0
> 结论:**第一次获取到权限就会一直生效(不排除用户自己去去掉给你授予的某些权限 =.= 所以权限的申请还是按照需求来,别什么都不管就把权限列表轮一遍...这是大忌!)**
