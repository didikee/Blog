## Android 5.x java.lang.VerifyError: Verifier rejected class ##

> `java.lang.VerifyError: Verifier rejected class com.xxx.xxx`

## 解决: ##

> **compileSdkVersion**不要是20与21(不要是5.x)即可.
> 
> **targetSdkVersion**同上一起修改了吧


	android {

    compileSdkVersion 23
    buildToolsVersion "23.0.3"
    useLibrary 'org.apache.http.legacy'



     defaultConfig {
        applicationId "com.fanwe.o2o.miguo"
        minSdkVersion 19
        targetSdkVersion 23
        compileOptions {
            sourceCompatibility JavaVersion.VERSION_1_7
            targetCompatibility JavaVersion.VERSION_1_7
        }
        // Enabling multidex support.
        multiDexEnabled true
    }



     buildTypes {
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.txt'
        }
    	}
	}

