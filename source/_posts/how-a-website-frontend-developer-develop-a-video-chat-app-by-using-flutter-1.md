---
layout: '[layout]'
title: how-a-website-frontend-developer-develop-a-video-chat-app-by-using-flutter-1
date: 2023-05-21 12:11:53
tags:
---


一个web前端撸一个视频通话App，是否能做到？当然可以啊，伊隆·马斯克都上天了，开发一个视频通话App能有多难? 为什么别人能做到，我就做不到？ 灵魂拷问自己千百次之后，下定决心，说干就干。

<!-- more -->

# 前期准备

**前端技术栈: Flutter**<br>
前期在技术栈选型方面，徘徊了一段时间，做了多个demo，不是各种各样未知的问题，就是性能上的问题而放弃了 uni-app, react-native, taro, tuari-mobile 这些技术栈，最终选定了 Flutter， Flutter是比较牛X的， 跨平台方面，同时支持 window应用，iOS应用，安卓应用也就只有它了。

**后端技术栈：腾讯云**<br>
就算我会写Java, C#, PHP，Rust， Node.js, Python后端服务，也然并卵，我一个人的团队，开发一个App， 前端和后端都要做, 要搞得猴年马月啊? 有没有现成的serverless CMS平台? 有，而且很成熟，开箱即用，噔噔噔噔，向你们隆重推荐 strapi， 什么？搞错了?不是这个?  是腾讯云开发，为什么是腾讯云开发?（以后我会慢慢解析）。

**需要两台手机**<br>
音频视通信嘛，肯定需要两个终端。

**平台说明**<br>
window还是macOS平台，看自己的条件， 各个平台所遇到的问题都各不相同, 本文主要以macOS为主。<br><br>


# WebRTC 选择?

选择 WebRTC方面，走了一些弯路，最早选择是的 flutter\_webrtc，但发现后端以及管理方面的能力，比如用户管理，流量统计，安全审计等等都是没有的，需要自己开发管理后台，天啊。。。市面上已经很多的很厉害的平台，何不直接拿来用？我应该集中时间和精力去做前端和交互（也就是产品），这也是我选择腾讯云的原因，它是TRTC, 生态完整，技术成熟，站在巨人的肩膀上，让我跑得更快，看得更高，更远。<br><br>

# TRTC App配置与运行

这里我使用带有UI的RTC项目, TuiCallKit项目链接 <https://github.com/tencentyun/TUICallKit.git>

    // clone本地
    git clone https://github.com/tencentyun/TUICallKit.git
    // 具体的路径是  Flutter/example/

除了RTC服务，TUIKit还使用了即时通信IM PaaS 服务，所以也需要开启和使用腾讯的即时通信IM

下面是项目的具体配置和运行说明。

1.即时通信IM控制台 新建一个应用<br>
即时通信IM控制台 <https://console.cloud.tencent.com/im><br>

![image.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/2befc9c6f1204df4b65592eb665833fa~tplv-k3u1fbpfcp-watermark.image?)

![image.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/55b35a78835c4d188e1c23eae65d789a~tplv-k3u1fbpfcp-watermark.image?)

2.拿到 SDKAppID和 secretKey 之后，更改项目配置.

```dart
// flutter/example/lib/debug/generate_test_user_sig.dart
class GenerateTestUserSig {
    static int sdkAppId = 0;   //修改这里
    static String secretKey = ''; // 修改这里
}
```

3.创建IM的用户

即时通信 -> 账号管理
<https://console.cloud.tencent.com/im/account-management>

![image.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/6b1e9785ae96495daf30a9f20632d1b6~tplv-k3u1fbpfcp-watermark.image?)

![image.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/fc2e15f42df743a5ac84e9c4183baa6e~tplv-k3u1fbpfcp-watermark.image?)

注：也可以在客户端临时注册，但呼叫的时候，需要知道对方的userID。

4.安装依赖<br>

    flutter pub get

5.xcode 项目配置<br>
使用xcode 直接打 Flutter\example\ios\Runner.xcworkspace， 点击Setting -> Accounts

setting位置<br>
![image.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/7402d07693744bc3a70b9fc4e0c69558~tplv-k3u1fbpfcp-watermark.image?)

点击 + 添加Apple ID<br>
![image.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/e3eb254b0ceb4f8f912b855f7958a4f6~tplv-k3u1fbpfcp-watermark.image?)

Runner  -> Signing & Capabilities -> Debug<br>
![image.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/43989d26f4da400ebc4690ad04e797b5~tplv-k3u1fbpfcp-watermark.image?)

![image.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/d740a496b29244159983e29308e8f18a~tplv-k3u1fbpfcp-watermark.image?)

team 那里选中你刚才新增的Apple ID的团队。<br>
![image.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/b9a53cb7c8de4ad994a740c5714bf2f7~tplv-k3u1fbpfcp-watermark.image?)

6.运行App<br>

![image.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/7d8eb28cb89e4d5a8927ce8f77b262f3~tplv-k3u1fbpfcp-watermark.image?)

提示失败<br>
![image.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/4194b343b21e484891c51f916f5a62ac~tplv-k3u1fbpfcp-watermark.image?)

需要在iphone设置信任 App，具体 设置 -> VPN与设置管理 -> 开发者App，然后点击信任。<br>

![image.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/e86086002f5745f4b5ad7ee845245da4~tplv-k3u1fbpfcp-watermark.image?)

最后，重新再运行，成功。

以下是运行成功之后的截图。

| Demo图片                                                                                                                            |                                                                                                                                   |
| --------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------- |
| ![image.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/3c5e80b88fc84cc2ae29951e9808eeb5~tplv-k3u1fbpfcp-watermark.image?) | ![image.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/0e983e185f65434188c8570320bd8756~tplv-k3u1fbpfcp-watermark.image?) |
| ![image.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/297f066a8d71437e8a4ce3e29ab9f898~tplv-k3u1fbpfcp-watermark.image?) | ![image.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/1354fa2987ce46d8a6486f962fe348bb~tplv-k3u1fbpfcp-watermark.image?) |

**腾讯云其他链接**<br>

实时音视频（Tencent RTC）<br>
<https://cloud.tencent.com/product/trtc>

实时音视频控制台<br>
<https://console.cloud.tencent.com/trtc>

TuiCallKit 开发文档<br>
<https://cloud.tencent.com/document/product/647/71930>

## 问题与解决方案

在运行成功之前，可能还会遇到其他问题，以下是我收集的一些问题和解决方案，供你参考。

**问题: window平台 flutter run 很慢**<br>
在window上面跑flutter run , 一直停留在Running Gradle task 'assembleDebug'... ,这个需要修改 android\build.gradle文件，改成阿里的镜像。

    // example\android\build.gradle
    buildscript {
        // 修改这里
        repositories {
            maven { url 'https://maven.aliyun.com/repository/public' } // jcenter
            maven { url 'https://maven.aliyun.com/repository/gradle-plugin' } // gradle-plugin
            maven { url 'https://maven.aliyun.com/repository/central' } // central
            maven { url 'https://maven.aliyun.com/repository/google' } // google
            google()
            mavenCentral()
            maven { url 'https://jitpack.io' }
        }
    }

    allprojects {
        // 修改这里
        repositories {
            maven { url 'https://maven.aliyun.com/repository/public' }
            maven { url 'https://maven.aliyun.com/repository/gradle-plugin' }
            maven { url 'https://maven.aliyun.com/repository/central' }
            maven { url 'https://maven.aliyun.com/repository/google' }
            google()
            mavenCentral()
            maven { url 'https://jitpack.io' }
        }
    }

<br>

**问题: window平台的运行提示No implementation found for method xxx on channel**<br>
解决方案: 据平台解释说 不支持虚拟机运行，需要在真机上运行。
![image.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/383c6bff90e0403b9cdd9288f728becd~tplv-k3u1fbpfcp-watermark.image?)

**问题: macOS平台 flutter run很慢，一直卡 Running pod install...** <br>
解决方案: 需要开启魔法（你懂的），并且复制终端代理命令， 具体操作如下：
![image.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/82b1a8eba0ec4b2da56c909ce14e0869~tplv-k3u1fbpfcp-watermark.image?)

![image.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/72c351db3bef40fbac4c2e5f68ebb723~tplv-k3u1fbpfcp-watermark.image?)

![image.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/45d8b0910b4a46a794dc293ccc2482e1~tplv-k3u1fbpfcp-watermark.image?)

**问题: macOS提示Framework not found TXFFmpeg/TXSoundTouch**<br>
好不容易,运行不卡，也不慢，，但提示 Framework not found TXFFmpeg 的错误。运行Ruuner，点击Build Phases --> Link Binary.. 搜索 TXFFmpeg 或者 TXSoundTouch 添加上去就可以，然后重新跑Flutter run.

**问题: macOS提示CFBundleShortVersionString的问题** <br>
在macOS平台运行时提示 The application's Info.plist does not contain a valid CFBundleShortVersionString<br>
解决方案: xcode打开 Flutter\example\ios\Runner.xcworkspace 在Runnder dashboard 在Build Settings 点击 + 新建添加一个键为  **Marketing Version**  然后搜索 Marketing Version, 在搜索结果 双击编辑 输入 \${FLUTTER\_BUILD\_NAME} 即可

![image.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/84703a0459254597b48877ce0f041345~tplv-k3u1fbpfcp-watermark.image?)

![image.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/59e4452e07ca4adca0543186f2b5340e~tplv-k3u1fbpfcp-watermark.image?)

**问题: App iphone真机上闪退问题**<br>
排除万能，项目终于可以在iphone真机上面运行了，但把数据线拔了之后，App就闪退了，这是致命的Bug?
原来之前跑的都是debug版本，需要插入数据线链上xcode的。<br>
解决方案：设置 Flutter 模块的编译模式为 profile 或 releas， xcode打开 Flutter\example\ios\Runner.xcworkspace， 在Build Settings选项，找到 User-Defined，点击 + 新建，添加一个键为 FLUTTER\_BUILD\_MODE ，值为 release。

![image.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/5ad0f63bb0df48c3b01a7e20618482b7~tplv-k3u1fbpfcp-watermark.image?)

**问题: codesign想要访问您的钥匙串中的密钥access**<br>
macOS 真机运行的时候, 弹窗提示 codesign想要访问您的钥匙串中的密钥access, 这是要输入你的Mac开机密码，如不小心点了拒绝。
解决方案： 打开 钥匙串App， 找到登录 -> 我的证书， 把 iPhone DeveloperXXX删除，然后重启xcode， 重新运行项目，再弹窗提示codesign，输入开机密码 点击 始终运行即可。

![2a0affe902a29490719526ef5cfc753.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/c2dabf546b4d4822b01cb82e425e8f33~tplv-k3u1fbpfcp-watermark.image?)

![image.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/7b408f2b53204189abb003f66af3ebb5~tplv-k3u1fbpfcp-watermark.image?)

# 待处理问题

*   [ ] 用户注册与登录
*   [ ] 微信登录
*   [ ] 离线呼醒功能
*   [ ] 新功能增加
<br><br>

# 其他相关链接
<https://www.jianshu.com/p/e1bb7bd80083><br>
<https://blog.csdn.net/crasowas/article/details/127537248><br>
<https://blog.csdn.net/weixin_27437725/article/details/112273583><br>
<https://www.bilibili.com/read/cv2899754/>
