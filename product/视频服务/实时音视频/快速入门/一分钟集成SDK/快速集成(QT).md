本文主要介绍如何快速地将腾讯云 TRTC SDK（QT 的 Mac 和 Windows 版本）集成到您的项目中，只要按照如下步骤进行配置，就可以快速完成 SDK 的集成工作。

## Mac 端集成
### 开发环境要求

- 操作系统：Mac10.10及以上版本。
- 开发环境：Qt Creator 4.10.3及以上版本，推荐使用 Qt Creator 4.13.3及以上。
- 开发框架：Based on Qt 5.10及以上。

### 操作步骤
本节以从0创建一个简单的 QTTest 项目为例，介绍如何在 Qt Creator 工程中集成 C++ 跨平台 SDK。

[](id:mac_step1)
1. **下载 C++ 跨平台 SDK**
	1. 下载 [SDK](https://liteav.sdk.qcloud.com/download/latest/TXLiteAVSDK_TRTC_Mac_latest.tar.bz2)，解压并打开文件。
	2. 在您的 QTTest 同级目录下新建一个空的 SDK 文件夹，将 SDK 中的`TXLiteAVSDKTRTCMacx.x.x/SDK/TXLiteAVSDKTRTC_Mac.framework` 拷贝到与您 QTTest 工程目录同级目录的 SDK 文件夹下。
2. **配置 QTTest.pro**[](id:mac_step2)
打开 QTTest 工程目录，使用一任意文本编辑器打开 `QTTest.pro` 文件，然后添加 SDK 相关引用：
```MAC
INCLUDEPATH += $$PWD/.
DEPENDPATH += $$PWD/.

LIBS += "-F$$PWD/base/util/mac/usersig"
LIBS += "-F$$PWD/../SDK"
LIBS += -framework TXLiteAVSDK_TRTC_Mac
LIBS += -framework Accelerate
LIBS += -framework AudioUnit

INCLUDEPATH += $$PWD/../SDK/TXLiteAVSDK_TRTC_Mac.framework/Headers/cpp_interface

INCLUDEPATH += $$PWD/base/util/mac/usersig/include
DEPENDPATH += $$PWD/base/util/mac/usersig/include
```
3. **授权摄像头和麦克风使用权限**[](id:mac_step3)
因为 SDK 会使用您的摄像头和麦克风，所以您需要在对应的 `Info.plist` 添加对应的权限申请说明：
```none
NSMicrophoneUsageDescription : 申请使用麦克风
NSCameraUsageDescription : 申请使用摄像头
```
如下图所示：
![](https://main.qcloudimg.com/raw/eac2455042c11db8ce3de49920fa18c1.png)
4. **引用 TRTC SDK**[](id:mac_step4)
	- 您可以通过头文件 `#include "ITRTCCloud.h"` 直接引用。
	- 使用命名空间：C++ 全平台接口的方法、类型等均定义在 trtc 命名空间中，为了让代码更加简洁，建议您直接使用 trtc 命名空间。

>? 至此您的集成工作已经完成，可以编译运行您的项目了。关于更多跨平台 SDK 的 API 使用 Demo，请下载 [QTDemo](https://github.com/tencentyun/TRTCSDK/tree/master/Mac) 详细参考。

## Windows 端集成
### 开发环境要求
- 操作系统：Windows 7及以上版本。
- 开发环境：Visual Studio 2015及以上版本，推荐使用 Visual Studio 2015，前提是您已经配置好 VS 相关的 QT 开发环境。
>? 如果您不熟悉配置 VS 相关 QT 开发环境的步骤，请参见 [README](https://github.com/tencentyun/TRTCSDK/blob/master/Windows/QTDemo/README.md) 中的操作步骤的步骤4相关内容。

### 操作步骤
本节以创建一个简单的 QTTest 项目为例，介绍如何在 Visual Studio 工程中集成 C++ 跨平台SDK。

1. **下载 C++ 跨平台 SDK**[](id:win_step1)
	1. 下载 [SDK](https://liteav.sdk.qcloud.com/download/latest/TXLiteAVSDK_TRTC_Win_latest.zip)，解压并打开文件。
	2. 您的 QTTest 同级目录下新建一个空的 SDK 文件夹，将 SDK 中的 `TXLiteAVSDKTRTCWin_latest/SDK/CPlusPlus` 拷贝到与您 QTTest 工程目录同级目录的 SDK 文件夹下。
2. **配置 QTTest 工程依赖环境**[](id:win_step2)
<dx-tabs>
:::  场景一：使用QtCreator配置依赖环境
打开 QTTest工程目录，使用一款任意文本编辑器（推荐 [Sublime Text](http://www.sublimetext.com/3)）打开 `QTTest.pro`（使用 Qt Creator 创建）文件，然后添加 SDK 相关引用：
<dx-codeblock>
::: windows
INCLUDEPATH += $$PWD/.
               $$PWD/../SDK/CPlusPlus/Win32/include \
               $$PWD/../SDK/CPlusPlus/Win32/include/TRTC

DEPENDPATH += $$PWD/.
               $$PWD/../SDK/CPlusPlus/Win32/include \
               $$PWD/../SDK/CPlusPlus/Win32/include/TRTC

CONFIG += opengl
CONFIG += debug_and_release

debug {
	contains(QT_ARCH,i386) {
		LIBS += -L$$PWD/../SDK/CPlusPlus/Win32/lib -lliteav
	} else {
		LIBS += -L$$PWD/../SDK/CPlusPlus/Win64/lib -lliteav
	}
}

release {
	contains(QT_ARCH,i386) {
		LIBS += -L$$PWD/../SDK/CPlusPlus/Win32/lib -lliteav
	} else {
		LIBS += -L$$PWD/../SDK/CPlusPlus/Win64/lib -lliteav
	}
}
:::
</dx-codeblock>
:::
::: 场景二：使用 VS 配置依赖环境
如果您的工程已经是一个成熟的 VS 项目：
1. 您也可以在 VS 中的工程属性**链接器** > **输入** > **附加依赖项**中添加 SDK 库文件 `liteav.lib`。
2. 在**链接器** > **常规** > **附加库目录**配置 SDK 库路径。以64位为例：`$(ProjectDir)SDK\CPlusPlus\Win64\lib`。
3. 同时在 **C/C++** > **常规** > **附件包含目录**设置好 SDK 的头文件路径 `$(ProjectDir)SDK\CPlusPlus\Win64\include` 和 `$(ProjectDir)SDK\CPlusPlus\Win64\include\TRTC`。
>?如果为32位，则可将以上路径中的 Win64 变为 Win32 即可。

:::
</dx-tabs>
3. **拷贝文件**[](id:win_step3)
	- 当使用 QtCreator 打开 `QTTest.pro` 工程并调试程序时，会自动生成相关的 `debug/release` 文件夹，您需要将 `SDK/CPlusPlus/Win64/lib` （如果为32位，则拷贝 `SDK/CPlusPlus/Win32/lib` ）下的所有的 `.dll` 文件分别拷贝到工程目录下的 `debug/release` 文件夹下。
	- 当使用VS进行调试时，可以手动将 `SDK/CPlusPlus/Win64/lib` 下的所有 `.dll` 文件拷贝到程序的输出运行目录下，也可以在**生成事件** > **后期生成事件** > **命令行**，添加拷贝命令 `copy /Y $(ProjectDir)SDK\CPlusPlus\Win64\lib\*.dll  $(OutDir)`，能够在编译完成后，自动将 SDK 的 .dll 文件拷贝到程序的运行目录下。
>?如果为32位，则添加拷贝命令为 `copy /Y $(ProjectDir)SDK\CPlusPlus\Win32\lib\*.dll  $(OutDir)` 。
4. **引用 TRTC SDK**[](id:win_step4)
	- 您可以通过头文件 `#include "ITRTCCloud.h"` 直接引用。
	- 使用命名空间：C++ 全平台接口的方法、类型等均定义在 trtc 命名空间中，为了让代码更加简洁，建议您直接使用 trtc 命名空间。

>? 至此您的集成工作已经完成，可以编译运行您的项目了。关于更多跨平台 SDK 的 API 使用 Demo，请下载 [QTDemo](https://github.com/tencentyun/TRTCSDK/tree/master/Windows) 详细参考。
