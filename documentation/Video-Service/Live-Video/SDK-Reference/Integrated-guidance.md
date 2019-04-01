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

* **直播SDK**  
JDCloud_Live Android SDK集成了 JDCloudLive，具有直播推流sdk的所有功能。  

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
待补充

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
-keep class com.jdcloud.media.live.** {
  *;
}  

3)在AndroidManifest.xml文件中申请相应权限
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

4）将demo assets目录下的所有文件copy到目标工程的assets目录下。
