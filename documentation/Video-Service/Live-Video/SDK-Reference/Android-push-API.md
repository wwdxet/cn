* **静音设置**  
```
mStreamer.setMuteAudio(true);//设置静音，false为非静音模式  
```
* **推流过程中截屏**
 ```
mStreamer.requestScreenShot(new GLRender.ScreenShotListener() {
    @Override
    public void onBitmapAvailable(Bitmap bitmap) {
        Date date = new Date();
        SimpleDateFormat dateFormat = new SimpleDateFormat("yyyy_MM_dd_HH_mm_ss",
                Locale.getDefault());
        String path = Environment.getExternalStorageDirectory().getPath() +
                "/jdc_live_" + dateFormat.format(date) + ".jpg";
        CommonUtils.saveBitmap(bitmap, path);
        mUIHandler.obtainMessage(MSG_SHOW_IMAGE_TOAST, path).sendToTarget();
    }
});
```
* **录制推流中视频**  
直播过程中，点击本地录制，可以录制屏幕到本地，录制格式为 MP4  
```
mStreamer.startRecord(Environment.getExternalStorageDirectory().getPath()+"/jdc_live_record.mp4");//参数为输出路径
```
* **纯音频推流**  
直播场景是声音直播时，可以调用以下方法启动纯音频推流。  
```
mStreamer.setAudioOnly(true);
```
* **水印**  
直播视频可以支持打上水印，原理是在SDK内部进行视频编码前把水印图片加上。水印类型支持图片、文字、时间等设置。
```
mStreamer.showWaterMarkLogo(bitmap, x, y, w, h, alphaVal);//图片
mStreamer.showWaterMarkLogo(path, x, y, w, h, alphaVal);//指定图片的位置
mStreamer.showWaterMarkTime(x, y, w,colorVal, alphaVal);//时间
mStreamer.showWaterMarkText(text, y, w, mWaterMarkColorVal, mWaterMarkAlphaVal);//文字
```
* **音频调用**  
在直播时可以设置自己喜欢的本地音乐加入直播中
```
mStreamer.getAudioPlayerCapture().setVolume(0.4f);//设置音量
mStreamer.setEnableAudioMix(true);
String audioPath = "file://" + Environment.getExternalStorageDirectory().getPath() + "/jdc_audio/" + titleView.getText().toString() + ".mp3";
mStreamer.startBgm(audioPath, true);// audioPath为本地音乐的url
```
* **音频升降调**  
升降调是主播唱歌时伴奏的特效处理，即从低沉到高亢的一个过程，更适用于较专业的唱歌直播，我们可以支持7[-3~3]个等级，负数为降调，正数为声调，0为原始声调。
```
AudioEffectFilter audioEffect = new AudioEffectFilter(
                        AudioEffectFilter.AUDIO_EFFECT_TYPE_PITCH);
audioEffect.setPitchLevel(mVoiceRiseFallLevel);
               mStreamer.getAudioPlayerCapture().getAudioFilterMgt().setFilter(audioEffect);
```
* **耳返**  
指的是当主播带上耳机来唱歌时，耳机中要能实时反馈主播的声音，这是由于主播自己的声音是通过头部骨骼（固体）传入耳朵，而观众听到声音最终是通过空气介质传入耳朵，这两种方式听的声音是有很大差异的，因此主播有需求直接听到观众端的效果。
如下打开耳返：   
```
mStreamer.setEnableAudioPreview(true);
```
* **旋转锁定**  
屏幕未锁定时候，旋转手机直播画面旋转，推流也要调用下面的方法进行旋转相应的角度。
```
mStreamer.setRotateDegrees(rotation);
```
* **自动对焦**  
手动点击屏幕，会根据手指聚焦的坐标进行自动对焦。
```
CameraTouchHelper cameraTouchHelper = new CameraTouchHelper();
cameraTouchHelper.setCameraCapture(mStreamer.getCameraCapture());
mGLSurface.setOnTouchListener(cameraTouchHelper);
cameraTouchHelper.setCameraHintView(mCameraHintView);// mCameraHintView为焦点上显示的view
```
* **音频降噪**  
在直播过程中，可以对周边噪音进行处理,以保证主播的声音更加清晰。
```
mStreamer.setEnableAudioNS(true);//打开音频降噪
```
* **镜像推流**  
镜像实际上直播端自己看自己永远是正的，切换的时候自己的镜头不动，但是观众的镜头看着是反转的。打开方式如下：
```
mStreamer.setFrontCameraMirror(on);
```
* **变声**  
变声，将主播声音变为搞怪的特效处理，能够增强互动时的趣味性，可支持大叔、萝莉、庄重、机器人等 效果.四种变声效果分别用：
```
AudioEffectFilter.AUDIO_EFFECT_TYPE_MALE
AudioEffectFilter.AUDIO_EFFECT_TYPE_FEMALE
AudioEffectFilter.AUDIO_EFFECT_TYPE_HEROIC
AudioEffectFilter.AUDIO_EFFECT_TYPE_ROBOT
四个参数控制，设置方式如下：
List<AudioFilterBase> filters = new LinkedList<>();
AudioEffectFilter audioEffect = new AudioEffectFilter(
                        AudioEffectFilter.AUDIO_EFFECT_TYPE_HEROIC);
filters.add(audioEffect);
mStreamer.getAudioFilterMgt().setFilter(filters);
```
* **美颜**  
美颜级别为0-5级，等级越高，美颜效果约明显。设置如下：
```
ImgFilterBase beautyFilter = new ImgBeautyProFilter(mStreamer.getGLRender(), PushActivity.this, 3);
beautyFilter.setGrindRatio(beautyVal);// beautyVal取值[o,1]
beautyFilter.setWhitenRatio(beautyVal);
beautyFilter.setRuddyRatio(2 * beautyVal - 1);
mStreamer.getImgTexFilterMgt().setFilter(filter);  
```
* **滤镜**  
滤镜模式有：清新，日光，风景，人物，日系，HDR几种选择。
```
通过ImgFilterBase filter = new ImgBeautySpecialEffectsFilter(mStreamer.getGLRender(),
                        PushActivity.this, filterIndex - 15);
mStreamer.getImgTexFilterMgt().setFilter(filter);
```
* **混音**  
推流过程中支持背景混音，支持背景音乐和麦克风的音轨混合，同时可以对其音量进行适当调节。
```
//设置背景音量输入比例
mStreamer.getMixerFilter().setInputVolume(0, volumeF); //volumeF取值[0,1]，为浮点型数据。
//设置麦克风音量输入比例
mStreamer.getMixerFilter().setInputVolume(1, volumeF); //volumeF取值[0,1]，为浮点型数据。
```
* **画中画推流**   
底层图片+视频画面+自定义位置
画中画是一种视频内容呈现方式,支持底层图片叠加视频画面，且支持自定义位置。在一部视频全屏播出的同时，于画面的小面积区域上同时播出另一部视频。
如可构造了3个图层叠加的示例:
最底层为背景图片
中间为本地视频播放画面
最上层为摄像头拍摄的画面
实例化JDCloudPIPLive  类，可以显示画中画推流：
```
JDCloudPIPLive  mStreamer = new JDCloudPIPLive(this);
((JDCloudPIPLive) mStreamer).showBgPicture(this, mPIPPicPath);//设置播放背景图
((JDCloudPIPLive) mStreamer).showBgVideo(mPIPVideoPath);//设置播放视频
((JDCloudPIPLive)mStreamer).getMediaPlayerCapture().getMediaPlayer().setVolume(0.4f, 0.4f);
((JDCloudPIPLive) mStreamer).setCameraPreviewRect(0.65f, 0.f, 0.35f, 0.3f);//设置推流视频位置
```
开始推流
```
((JDCloudPIPLive) mStreamer).getMediaPlayerCapture().getMediaPlayer().start();
((JDCloudPIPLive) mStreamer).getPictureCapture().start(this, mPIPPicPath);
```
暂停推流：
```
mStreamer.onPause();
((JDCloudPIPLive) mStreamer).getMediaPlayerCapture().getMediaPlayer().pause();
((JDCloudPIPLive) mStreamer).getPictureCapture().stop();
```
停止推流代码如下：
```
mStreamer.stopPush();
mStreamer.hideBgPicture();//隐藏背景图
mStreamer.hideBgVideo();//隐藏视频
mStreamer.setCameraPreviewRect(0.f, 0.f, 1.f, 1.f);
```
* **画笔推流**  
在直播过程中，主播使用画笔画出任意文字、图形等，通过与摄像头画面合成后，再一起推送给观众端，主播端在页面上涂鸦，观众看到的就是带涂鸦的画面。涂鸦内容于主播画面已经完成了叠加，无需其他传输渠道。无论移动端、flash等播放器，看到的涂鸦画面完全一致。
