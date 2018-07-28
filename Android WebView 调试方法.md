> 调试Android WebView中的h5页面，通常就是通过alert和抓包工具来定位问题，效率低且无法直接调试样式或打断点，可谓是事倍功半。通过chrome的开发工具在原生 Android 应用中调试 WebView。

## 前提条件：
> Android4.4+

## 基本原理：
1.在APP中启用 WebView 调试，开启调试后，Chrome DevTools才能对WebView进行远程调试；
> WebView.setWebContentsDebuggingEnabled(true);

2.通过访问 `chrome://inspect/#devices` 访问已启用调试的 WebView 列表；

3.点击inspect进入新开调试页面；页面若长时间空白，请开启翻墙；

## 两个小Tips：

（1）访问chrome://inspect/#devices如果chrome没有检测到Remote Target中的页面，可能需要安装一下chrome的ADB插件；

（2）对于腾讯系的APP，默认采用X5内核，需要将WebViewDebugHook的git目录下的debug.conf文件拷贝到SD卡的根目录下即可。 