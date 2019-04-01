## 阅读对象
本文档面向所有使用京东云直播SDK的开发、测试人员等, 要求读者具有一定的iOS编程开发经验。
## 运行环境
**支持平台**  
SDK支持iOS 8.0以上系统，硬件CPU支持ARM64  
**开发环境**  
SDK编译环境Xcode 8.0及以上版本，Xcode运行环境OS X 10.10 及以上版本  
## 版本说明  
**迭代**  
当前版本为1.0.0    

## 快速集成  
本章节提供一个快速集成京东云推流SDK基础功能的示例。具体可以参考demo工程中的相应文件。  
**下载工程**   
iOS端推流SDK&播放器SDK请<a href="https://sdk-publish.oss.cn-north-1.jcloudcs.com/sdk/jdcloud_streamer_ios.zip">点击下载</a>
**工程目录结构**  
推流SDK的工程目录如下：包含头文件Headers。  
![](https://github.com/jdcloudcom/cn/blob/cn-Video-on-Demand/image/live-video/ios%E6%8E%A8%E6%B5%811.png)  
**配置项目**  
1.手动导入SDK。从Demo中拷贝jdcloud_live_sdk_ios.framework，导入拖入您的Xcode工程中    
![](https://github.com/jdcloudcom/cn/blob/cn-Video-on-Demand/image/live-video/ios%E6%8E%A8%E6%B5%812.png)      
2.在 Building Setting > Valid Architectures 删除armv7、armv7s，只保留arm64。    
![](https://github.com/jdcloudcom/cn/blob/cn-Video-on-Demand/image/live-video/ios%E6%8E%A8%E6%B5%813.png)      
3.在 Building Setting > Enable Bitcode 修改为 NO。  
![](https://github.com/jdcloudcom/cn/blob/cn-Video-on-Demand/image/live-video/ios%E6%8E%A8%E6%B5%814.png)   
4.在 Building Setting > Other Linker Flags增加-all_load。  
![](https://github.com/jdcloudcom/cn/blob/cn-Video-on-Demand/image/live-video/ios%E6%8E%A8%E6%B5%815.png)   
5.在 Info.plist 文件中添加麦克风和摄像头权限Privacy - Microphone Usage Description、Privacy - Camera Usage Description。    
![](https://github.com/jdcloudcom/cn/blob/cn-Video-on-Demand/image/live-video/ios%E6%8E%A8%E6%B5%816.png)   
6.若新建工程，且使用cocoapods，(未使用cocoapods请自动忽略该配置)。发生如下错误：  
```
Target 'jdcloud_live_demo' (project 'jdcloud_live_demo') has process command with output   '/Users/pengdandan1/Library/Developer/Xcode/DerivedData/jdcloud_live_demo-fudhfrujrnpeaicbkuwkjszgdbsq/Build/Products/Debug-iphoneos/jdcloud_live_demo.app/Info.plist'
```
![](https://github.com/jdcloudcom/cn/blob/cn-Video-on-Demand/image/live-video/ios%E6%8E%A8%E6%B5%817.png)   
解决方案：  
可进行如下操作：在Xcode菜单栏 -> File -> Workspace Setting，将build system修改为legacy build system，然后clean后编译  
请参考网址：https://www.jianshu.com/p/8a8444acdca5  
![](https://github.com/jdcloudcom/cn/blob/cn-Video-on-Demand/image/live-video/ios%E6%8E%A8%E6%B5%818.png)   
