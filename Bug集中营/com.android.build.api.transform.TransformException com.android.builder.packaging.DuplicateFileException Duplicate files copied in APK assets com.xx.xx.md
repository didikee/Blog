## com.android.build.api.transform.TransformException: com.android.builder.packaging.DuplicateFileException: Duplicate files copied in APK assets/com.xx.xx ##

#### 完整的Error 信息(关键部分) ####



> Error:Execution failed for task ':fanwe_o2o_47_mgxz_dingzhi:transformResourcesWithMergeJavaResForDebug'.
> com.android.build.api.transform.TransformException: com.android.builder.packaging.DuplicateFileException: Duplicate files copied in APK assets/com.tencent.plus.logo.png
	File1: E:\TGit\EclispeAS\mgxz_user\fanwe_o2o_47_mgxz_dingzhi\libs\open_sdk.jar
	File2: E:\TGit\EclispeAS\mgxz_user\fanwe_o2o_47_mgxz_dingzhi\build\intermediates\exploded-aar\mgxz_user\library_umeng_share_project\unspecified\jars\classes.jar

#### 解决方案: ####
**在Gradle中配置:**
>     packagingOptions {
> 
        exclude 'assets/com.tencent.plus.logo.png'
    }

#### 原因: ####

	在AS编译打包apk是,资源文件重复了,有两个名称一样的"com.xx.xx",
	配置的意思是保留其中一个,这样就能打包成功.

#### 注意: ####

**上面的解决方案是在确定你没有重复导入jar包,so文件.注意不同module的jar与so文件,他们打包时会合并,所以同样的jar在整个项目中只能含有一份!**


