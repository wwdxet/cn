## 阅读对象
本文档面向所有使用京东云直播SDK的开发、测试人员等, 要求读者具有一定的Android编程开发经验，读者具备阅读 wiki的习惯更佳。  

## 运行环境
•	最低支持版本为Android 5.0 (API level 21)
•	支持的CPU架构： arm64-v8a, armeabi-v7a, x86
软硬编部分功能对Android系统版本的要求及相关限制 
<table>
<tr>
    <td>功能</td>
    <td>软编</td>
    <td>硬编</td>
    <td>软编兼容模式</td>
</tr>
<tr>
    <td>基础推流</td>
    <td>5.0(21)</td>
    <td>5.0(21)</td>
    <td>5.0(21)</td>
</tr>
<tr>
    <td>网络自适应</td>
    <td>5.0(21)</td>
    <td>5.0(21)</td>
    <td>5.0(21)</td>
</tr>
 <tr>
    <td>内置美颜滤镜</td>
    <td>所有</td>
    <td>所有</td>
    <td>仅Denoise</td>
</tr> 
</table>  

## 版本说明
* **迭代**  
当前版本为1.0.0  

* **体系结构**  
当前推流 JDCloud_Live Android SDK支持以下体系结构:
armeabi-v7a
arm64-v8a
x86
为了节省apk size，如果没有特殊缘由，请只集成armeabi-v7a版本。   
只集成armeabi-v7a版本，会导致ARMv5 ARMv6 设备不能运行。如果APP需要适配这两类设备，需要额外集成armebi版本。  
ARMv5 ARMv6 设备计算性能较差，京东云不保证该芯片设备上的直播体验。不推荐直播APP视频适配该两款芯片设备。  

## 快速集成
本章节提供一个快速集成京东云推流SDK基础功能的示例。
具体可以参考demo工程中的相应文件。

* **获取SDK和demo**  


* **工程目录结构**  
demo: 示例工程，演示本SDK主要接口功能的使用    
libs: 集成SDK需要的所有库文件    

jniLibs/[armeabi-v7a|arm64-v8a|x86]: 各平台的so库    
libs/jdc_live.jar: SDK jar包    

* **配置项目**    
引入目标库, 将libs目录下的库文件引入到目标工程中并添加依赖。
可参考下述配置方式（以Android Studio为例）：  
1)导入SDK
手动集成：
将libs目录copy到目标工程的libs下；
将jniLibs目录拷贝到目标工程的src\main\jniLibs;

2)修改proguard(混淆)文件，添加如下规则：
```
-keep class com.jdcloud.media.live.** {
  *;
} 
```

3)在AndroidManifest.xml文件中申请相应权限
```
<!-- 使用权限 -->
<uses-permission android:name="android.permission.READ_PHONE_STATE" />
<uses-permission android:name="android.permission.SYSTEM_ALERT_WINDOW" />
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
<uses-permission android:name="android.permission.READ_PHONE_SINTERNETWIFI_STATE" />
<uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
<uses-permission android:name="android.permission.CAMERA" />
<uses-permission android:name="android.permission.RECORD_AUDIO" />
<uses-permission android:name="android.permission.FLASHLIGHT" />
<uses-permission android:name="android.permission.VIBRATE" />
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
<uses-permission android:name="android.permission.MODIFY_AUDIO_SETTINGS" />
<!-- 硬件特性 -->
<uses-feature android:name="android.hardware.camera" />
<uses-feature android:name="android.hardware.camera.autofocus" />  
```

4）将demo assets目录下的所有文件copy到目标工程的assets目录下。  

## 简单推流示例    

具体可参考demo工程中的 com.jdcloud.media.demo.push.ui.PushActivity 类.  
1.在布局文件中加入预览View, 当前支持GLSurfaceView和TextureView：  
```
<android.opengl.GLSurfaceView
            android:id="@+id/gl_live_push"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:layout_alignParentBottom="true"
            android:layout_alignParentTop="true" />
```

2.利用ButterKnife初始化预览View：  
```
    @BindView(R.id.gl_live_push)
GLSurfaceView mGLSurface;
``` 

3.创建JDCloudLive实例并配置推流相关参数：
```
JDCloudLive mStreamer = new JDCloudLive(this);
//设置推流url
mStreamer.setUrl(“rtmp://pu.jdcloud.com/android/ {streamName}”);
// 设置推流分辨率
mStreamer.setPreviewResolution(BaseConstants.VIDEO_RESOLUTION_360P);
mStreamer.setTargetResolution(BaseConstants.VIDEO_RESOLUTION_360P);
// 设置编码方式（硬编、软编）
mStreamer.setEncodeMethod(BaseConstants.ENCODE_METHOD_HARDWARE);
mStreamer.setVideoEncodeProfile(VideoEncodeFormat.ENCODE_PROFILE_HIGH_PERFORMANCE);
// 设置推流视频码率，三个参数分别为初始码率、最高码率、最低码率
int videoBitrate = 800;
mStreamer.setVideoKBitrate(videoBitrate * 3 / 4, videoBitrate, videoBitrate / 4);
// 设置屏幕的旋转角度
mStreamer.setRotateDegrees(0);
//设置开始预览使用前置还是后置摄像头
mStreamer.setCameraFacing(CameraCapture.FACING_FRONT);
// 设置预览View
mStreamer.setDisplayPreview(mGLSurface);
```
4.创建推流事件监听:  
Note：所有回调均运行在JDCloudLive的创建线程，建议在主线程中进行，开发者可以直接在回调中操作 JDCloudLive的相关接口，但不要在这些回调中做任何耗时的操作。  
```
// 设置Info回调，可以收到相关通知信息
mStreamer.setOnInfoListener(new JDCloudLive.OnInfoListener() {
            @Override
            public void onInfo(int i, int i1, int i2) {
                
            }
        });
// 设置错误回调，收到该回调后，一般是发生了严重错误，比如网络断开等，
// SDK内部会停止推流，APP可以在这里根据回调类型及需求添加重试逻辑。
mStreamer.setOnErrorListener(new JDCloudLive.OnErrorListener() {

            @Override
            public void onError(int i, int i1, int i2) {
                
            }
        });
```
5.开始推流：  
Note：初次开启预览后需要在OnInfoListener回调中收到 BaseConstants.LIVE_CAMERA_INIT_DONE 事件后调用方才有效。  
```
mStreamer.startPush();
```
6.推流开始前及推流过程中可动态设置的常用方法：
```
// 设置屏幕的旋转角度，支持 0, 90, 180, 270
mStreamer.setRotateDegrees(0);
//设置开始预览使用前置还是后置摄像头
mStreamer.setCameraFacing(CameraCapture.FACING_FRONT);
// 开关闪光灯，true为开启，false为关闭
mStreamer.toggleTorch(true);
// 设置美颜滤镜，关于美颜滤镜的具体说明请参见专题说明
ImgFilterBase beautyFilter = new ImgBeautyProFilter(mStreamer.getGLRender(), PushActivity.this, 3);
beautyFilter.setGrindRatio(beautyVal);
beautyFilter.setWhitenRatio(beautyVal);
beautyFilter.setRuddyRatio(2 * beautyVal - 1);
mStreamer.getImgTexFilterMgt().setFilter(filter);
```
7.停止推流：
```
mStreamer.stopPush();
```
8.Activity生命周期的回调处理：
```
public class PushActivity extends Activity {
    @Override
    public void onResume() {
        super.onResume();
mStreamer.onResume();
        mStreamer.stopImageCapture();
// 一般可以在onResume中开启摄像头预览
        mStreamer.startCameraPreview();
//如果onPause中切到了DummyAudio模块，可以在此恢复
        mStreamer.setUseDummyAudioCapture(false);
        }

    @Override
    public void onPause() {
        super.onPause();
        mStreamer.onPause();
        // 一般在这里停止摄像头采集
        mStreamer.stopCameraPreview();
// 如果希望App切后台后，停止录制主播端的声音，可以在此切换为
//DummyAudio采集，该模块会代替mic采集模块产生静音数据，同时释放占用的//mic资源
        mStreamer.setUseDummyAudioCapture(true);
mStreamer.startImageCapture(mBgImagePath);
    }

    @Override
    public void onDestroy() {
        // 清理相关资源
        mStreamer.release();
super.onDestroy();
    }
```

