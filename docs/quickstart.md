# 快速开始
使用`npm`安装
```shell
npm install --save react-native-jiguang-message@latest
```
## 使用RN link命令
推荐使用`react native link`命令，在link过程中请按照提示输入appKey与masterSecret
```shell
 react-native link react-native-jiguang-message
```
## 手动link
手动link适用于自动link失效时，请按照不同的平台遵照以下步骤
### iOS

* 打开你的Xcode工程

* 在`node_modules/react-native-jiguang-message/ios`目录下找到`RCTJMessage.xcodeproj` 把它拖到`Libraries`中

* 选择项目中的"Build Phases"进行配置

* 在`Libraries/RCTJMessage.xcodeproj/Products`目录下找到`libRCTJMessage.a`拖到 "Link Binary With Libraries"中

* 在"Link Binary With Libraries"列表中添加以下库：
`libz.tbd,libsqlite3.tbd,libresolv.tbd,UIKit.framework,Foundation.framework,`
`SystemConfiguration.framework,CoreFoundation.framework,CFNetwork.framework,`
`Security.framework,CoreTelephony.framework`

* 在`../node_modules/react-native-jiguang-message/ios/RCTJMessage`目录下找到`JMessage.framework`，在`../node_modules/react-native-jiguang-message/ios/RCTJMessage/JMessage.framework`目录下找到`jcore-ios-1.1.0.a`，把这两个文件拖到"Link Binary With Libraries"中，在"Build Settings"标签设置的"Framework Search Paths"中添加`$(SRCROOT)/../node_modules/react-native-jiguang-message/ios/RCTJMessage/**`

* 在AppDelegate.m文件中添加以下代码 
```objectiv-c
...
#import <JMessageModule/RCTJMessageModule.h>
```

* 在didFinishLaunchingWithOptions方法中添加以下代码
```objectiv-c
[JMessage registerForRemoteNotificationTypes:(UIUserNotificationTypeBadge | UIUserNotificationTypeSound | UIUserNotificationTypeAlert) categories:nil];
	#ifdef DEBUG
	[RCTJMessageModule setupJMessage:launchOptions apsForProduction:false category:nil];
	#else
	[RCTJMessageModule setupJMessage:launchOptions apsForProduction:true category:nil];
	#endif
```

* 在Info.plist文件中添加JiguangAppKey、JiguangMasterSecret and JiguangAppChannel

### Android

* 在`android/settings.gradle`文件中添加如下代码

    ```gradle
    include ':react-native-jiguang-message'
    project(':react-native-jiguang-message').projectDir = new File(rootProject.projectDir, '../node_modules/react-native-jiguang-message/android')
    ```

* 在`android/app/build.gradle`文件中添加引用

    ```gradle
    ...
    dependencies {
        ...
        compile project(':react-native-jiguang-message')
    }
    ```
* 在 react-native-jiguang-message node_modules的`android/build.gradle`文件中添加 AppKey、AppChannel、MasterSecret

    ```gradle
    ...
    manifestPlaceholders = [
            JIGUANG_APPKEY: ${JIGUANG_APPKEY},
            JGUANG_APPCHANNEL: "developer-default",
            JIGUANG_MASTER_SECRET: ${JIGUANG_MASTER_SECRET}
    ]
    ```
* 在`MainApplication.java`文件中添加以下代码

```java
...
import com.xsdlr.rnjmessage.JMessagePackage;

public class MainApplication extends Application implements ReactApplication {

    private final ReactNativeHost mReactNativeHost = new ReactNativeHost(this) {
        ...

        @Override
        protected List<ReactPackage> getPackages() {
            return Arrays.<ReactPackage>asList(
                new MainReactPackage(),
                new JMessagePackage(BuildConfig.DEBUG)
            );
        }
    };
}
```



