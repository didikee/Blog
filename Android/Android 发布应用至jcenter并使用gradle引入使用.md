## Android 发布应用至jcenter并使用gradle引入使用

[TOC]
### 1. maven插件配置
在工程根目录的`gradle`下配置
 完整的代码如下:
```
// Top-level build file where you can add configuration options common to all sub-projects/modules.

buildscript {
    repositories {
        jcenter()
    }
    dependencies {
        classpath 'com.android.tools.build:gradle:2.1.3'

        /*发布到仓库*/
        classpath 'com.jfrog.bintray.gradle:gradle-bintray-plugin:1.2'
        classpath 'com.github.dcendents:android-maven-gradle-plugin:1.4.1'

        // NOTE: Do not place your application dependencies here; they belong
        // in the individual module build.gradle files
    }
}

allprojects {
    repositories {
        jcenter()
    }
}
```
### 2. gradle里配置

```
Library(本身也是Module嘛)里配置:(可以先复制粘贴,然后修改)
```

完整代码如下:
```
apply plugin: 'com.android.library'
//添加申请生成插件(下面两行)
apply plugin: 'com.github.dcendents.android-maven'
apply plugin: 'com.jfrog.bintray'
//实际使用的名称 compile 'com.didikee:UILibrary:0.0.4' 就是group + ：+module名字 + ：+version

//------------ 每个项目默认的配置 start
android {
    compileSdkVersion 24
    buildToolsVersion "24.0.0"

    defaultConfig {
	    //applicationId "com.xxx.xx"   library没有applicationId
        multiDexEnabled true
        versionCode 1
        versionName "1.0"
        minSdkVersion 15
        targetSdkVersion 24
    }
    buildTypes {
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
        }
    }
}

dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
    testCompile 'junit:junit:4.12'
    compile 'com.android.support:appcompat-v7:24.1.1'
}
//-------------------每个项目默认的配置 end

//------------------ bintray maven 配置start
ext {
    bintrayRepo = 'maven'  //你在 bintray 上要先建立一个maven类型的仓库,我的仓库就叫maven,你也可以取别的名字
    bintrayName = 'UILibrary'  //

    publishedGroupId = 'com.didikee'  //GroupId 
    libraryName = 'UILibrary'  //module 的名称
    artifact = 'UILibrary'  //module 的名称

    libraryDescription = 'A UI libs on Android'  //描述信息

    siteUrl = 'https://github.com/didikee/AndroidNormalUILibs' //github 项目地址
    gitUrl = 'https://github.com/didikee/AndroidNormalUILibs.git'//git地址

    libraryVersion = '0.0.4' //版本号,更新的时候需要增加,不能减小

    developerId = 'didikee'
    developerName = 'didikee'
    developerEmail = 'didikee@qq.com'

    licenseName = 'The Apache Software License, Version 2.0'
    licenseUrl = 'http://www.apache.org/licenses/LICENSE-2.0.txt'
    allLicenses = ["Apache-2.0"]
}
// 以下部分不需要修改
version = libraryVersion

if (project.hasProperty("android")) { // Android libraries
    task sourcesJar(type: Jar) {
        classifier = 'sources'
        from android.sourceSets.main.java.srcDirs
    }

    task javadoc(type: Javadoc) {
        options.encoding = "utf-8"
        options.charSet = 'UTF-8'
        source = android.sourceSets.main.java.srcDirs
        classpath += project.files(android.getBootClasspath().join(File.pathSeparator))
		failOnError false
    }
} else { // Java libraries
    task sourcesJar(type: Jar, dependsOn: classes) {
        classifier = 'sources'
        from sourceSets.main.allSource
    }
}

task javadocJar(type: Jar, dependsOn: javadoc) {
    classifier = 'javadoc'
    from javadoc.destinationDir
}

artifacts {
    archives javadocJar
    archives sourcesJar
}

// Bintray
Properties properties = new Properties()
properties.load(project.rootProject.file('local.properties').newDataInputStream())

bintray {
    user = properties.getProperty("bintray.user")
    key = properties.getProperty("bintray.apikey")

    configurations = ['archives']
    pkg {
        repo = bintrayRepo
        name = bintrayName
        desc = libraryDescription
        websiteUrl = siteUrl
        vcsUrl = gitUrl
        licenses = allLicenses
        publish = true
        publicDownloadNumbers = true
        version {
            desc = libraryDescription
            gpg {
                sign = true //Determines whether to GPG sign the files. The default is false
                passphrase = properties.getProperty("bintray.gpg.password")
                //Optional. The passphrase for GPG signing'
            }
        }
    }
}

group = publishedGroupId   // Maven Group ID for the artifact

install {
    repositories.mavenInstaller {
        // This generates POM.xml with proper parameters
        pom {
            project {
                packaging 'aar'
                groupId publishedGroupId
                artifactId artifact

                // Add your description here
                name libraryName
                description libraryDescription
                url siteUrl

                // Set your license
                licenses {
                    license {
                        name licenseName
                        url licenseUrl
                    }
                }
                developers {
                    developer {
                        id developerId
                        name developerName
                        email developerEmail
                    }
                }
                scm {
                    connection gitUrl
                    developerConnection gitUrl
                    url siteUrl

                }
            }
        }
    }
}
//------------------ maven 配置end
```

### 3. 配置用户名与密钥
在`local.properties`中添加:

```
//这些信息在bintray网站的个人中心可以看到
bintray.user=didikee //用户id
bintray.apikey=581d2d**********************27ddd2376  //apikey
```

### 4. 编译并上传

打开`AndroidStudio`自带的终端`terminal`:

```
1. `gradlew install` //成功继续上传操作
2. `gradlew bintrayUpload` //上传操作
```

### 5. 添加到jcenter
去`bintray`你的个人中心,找到你刚刚上传的`package`中点击`Add to Jcenter`,执行申请操作.
接着等待就可以了.
通过审核后就可以使用了.

	//在实际项目使用
    compile 'com.didikee:UILibrary:0.0.4'
    收工=.=
 
 