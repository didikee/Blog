## 拓展 Android 原生 CountDownTimer 倒计时

[TOC]

#### CountDownTimer

> 在系统的`CountDownTimer`上进行的修改,主要是拓展了功能,当然也保留了系统默认的模式.

> 四种模式:
> - Normal模式:  向上取整(我觉得应该是日常中用的最多的)
> - Floor模式:  向下取整
> - System模式:  系统默认的(保留系统原始功能)
> - SystemFix模式:  系统默认会少一个`onTick()`回调,这里只是把缺的这个回调加进去

在Activity中的代码如下:

```
final CountDownTimer timer=new CountDownTimer(10000,1000,CountDownTimer.NORMAL) {
            @Override
            public void onTick(long millisUntilFinished) {
                Log.d("test","millisUntilFinished: "+millisUntilFinished);
                long l = millisUntilFinished / 1000;
                mTs.setText(""+l);
            }

            @Override
            public void onFinish() {
                Log.d("test","onFinish 0");
                mTs.setText("0");
            }
        };

        mBt_start.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                timer.start();
            }
        });
```
 
 **对应模式的Log如下所示:**


#### Normal 模式

```
08-10 09:23:53.595 27628-27628/didikee.com.demoapk D/test: millisUntilFinished: 10000
08-10 09:23:54.595 27628-27628/didikee.com.demoapk D/test: millisUntilFinished: 9000
08-10 09:23:55.605 27628-27628/didikee.com.demoapk D/test: millisUntilFinished: 8000
08-10 09:23:56.605 27628-27628/didikee.com.demoapk D/test: millisUntilFinished: 7000
08-10 09:23:57.605 27628-27628/didikee.com.demoapk D/test: millisUntilFinished: 6000
08-10 09:23:58.605 27628-27628/didikee.com.demoapk D/test: millisUntilFinished: 5000
08-10 09:23:59.605 27628-27628/didikee.com.demoapk D/test: millisUntilFinished: 4000
08-10 09:24:00.605 27628-27628/didikee.com.demoapk D/test: millisUntilFinished: 3000
08-10 09:24:01.605 27628-27628/didikee.com.demoapk D/test: millisUntilFinished: 2000
08-10 09:24:02.615 27628-27628/didikee.com.demoapk D/test: millisUntilFinished: 1000
08-10 09:24:03.605 27628-27628/didikee.com.demoapk D/test: onFinish 0
```

#### Floor 模式

```
08-10 09:26:54.455 27628-27628/didikee.com.demoapk D/test: millisUntilFinished: 9000
08-10 09:26:55.455 27628-27628/didikee.com.demoapk D/test: millisUntilFinished: 8000
08-10 09:26:56.455 27628-27628/didikee.com.demoapk D/test: millisUntilFinished: 7000
08-10 09:26:57.455 27628-27628/didikee.com.demoapk D/test: millisUntilFinished: 6000
08-10 09:26:58.455 27628-27628/didikee.com.demoapk D/test: millisUntilFinished: 5000
08-10 09:26:59.455 27628-27628/didikee.com.demoapk D/test: millisUntilFinished: 4000
08-10 09:27:00.465 27628-27628/didikee.com.demoapk D/test: millisUntilFinished: 3000
08-10 09:27:01.465 27628-27628/didikee.com.demoapk D/test: millisUntilFinished: 2000
08-10 09:27:02.465 27628-27628/didikee.com.demoapk D/test: millisUntilFinished: 1000
08-10 09:27:03.465 27628-27628/didikee.com.demoapk D/test: onFinish 0
```

#### System 模式

```
08-10 09:29:03.035 27628-27628/didikee.com.demoapk D/test: millisUntilFinished: 9999
08-10 09:29:04.035 27628-27628/didikee.com.demoapk D/test: millisUntilFinished: 8998
08-10 09:29:05.035 27628-27628/didikee.com.demoapk D/test: millisUntilFinished: 7996
08-10 09:29:06.035 27628-27628/didikee.com.demoapk D/test: millisUntilFinished: 6995
08-10 09:29:07.045 27628-27628/didikee.com.demoapk D/test: millisUntilFinished: 5993
08-10 09:29:08.045 27628-27628/didikee.com.demoapk D/test: millisUntilFinished: 4992
08-10 09:29:09.045 27628-27628/didikee.com.demoapk D/test: millisUntilFinished: 3990
08-10 09:29:10.045 27628-27628/didikee.com.demoapk D/test: millisUntilFinished: 2989
08-10 09:29:11.045 27628-27628/didikee.com.demoapk D/test: millisUntilFinished: 1987
08-10 09:29:13.035 27628-27628/didikee.com.demoapk D/test: onFinish 0
```

#### SystemFix 模式
```
08-10 09:29:59.795 27628-27628/didikee.com.demoapk D/test: millisUntilFinished: 9999
08-10 09:30:00.795 27628-27628/didikee.com.demoapk D/test: millisUntilFinished: 8998
08-10 09:30:01.805 27628-27628/didikee.com.demoapk D/test: millisUntilFinished: 7997
08-10 09:30:02.795 27628-27628/didikee.com.demoapk D/test: millisUntilFinished: 6997
08-10 09:30:03.795 27628-27628/didikee.com.demoapk D/test: millisUntilFinished: 5996
08-10 09:30:04.805 27628-27628/didikee.com.demoapk D/test: millisUntilFinished: 4994
08-10 09:30:05.805 27628-27628/didikee.com.demoapk D/test: millisUntilFinished: 3992
08-10 09:30:06.805 27628-27628/didikee.com.demoapk D/test: millisUntilFinished: 2990
08-10 09:30:07.805 27628-27628/didikee.com.demoapk D/test: millisUntilFinished: 1989
08-10 09:30:08.805 27628-27628/didikee.com.demoapk D/test: millisUntilFinished: 988
08-10 09:30:09.795 27628-27628/didikee.com.demoapk D/test: onFinish 0
```