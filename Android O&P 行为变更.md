行为变更大致分两类： 
 
a. 针对所有 API 等级开发的应用；   
b. 针对指定 API 等级开发的应用；

# Android O(8.0)行为变更

[官方文档](https://developer.android.google.cn/about/versions/oreo/android-8.0-changes)

## 针对所有 API 级别的应用

## 针对 Android 8.0 的应用

### 1. Android O(8.0)运行时权限策略变化

> 在 Android 8.0 之前，如果应用在运行时请求权限并且被授予该权限，系统会错误地将属于同一权限组并且在清单中注册的其他权限也一起授予应用。

> 对于针对 Android 8.0 的应用，此行为已被纠正。系统只会授予应用明确请求的权限。然而，一旦用户为应用授予某个权限，则所有后续对该权限组中权限的请求都将被自动批准。
 
建议：同组权限一起申请。当我们申请权限时。申请同组的多个权限时，也只会弹出一次申请框。所以不如一起申请。


### 2. Android O(8.0)推送通知的变化
[视频](https://v.qq.com/x/page/v0515i5pcjc.html)
  
[通知文档](https://developer.android.google.cn/guide/topics/ui/notifiers/notifications.html)

[通知范例](https://github.com/googlesamples/android-NotificationChannels)

### 3. Android O(8.0)安装Apk所需权限变化
> Android 8.0去除“允许未知来源”选项，需手动确认。如果我们的App具备安装App的功能，那么AndroidManifest文件添加android.permission.REQUEST_INSTALL_PACKAGES权限，未声明此权限的应用将无法安装其他应用。

### 4. Android O(8.0)自适应图标

[视频](https://v.qq.com/x/page/q0515bbfjyo.html)  

[官方文档](https://developer.android.google.cn/guide/practices/ui_guidelines/icon_design_adaptive)


# Android P(9.0)行为变更

[官方文档](https://developer.android.google.cn/preview/behavior-changes)

## 针对所有运行在 Android P 上的应用

### 强制性 FLAG_ACTIVITY_NEW_TASK 要求
> 在 Android P 中，除非使用了 FLAG_ACTIVITY_NEW_TASK  intent flag，您不能在非 Activity 场景下启动 Activity。若您未使用这个 intent flag 就尝试启动 Activity，系统无法正常开启活 Activity，并会在日志中留下相应信息。

注意：FLAG_ACTIVITY_NEW_TASK 标签的要求属为预期行为，并且从 Android N 就对此作了强制要求。但是 Android N 系统中的错误暂时导致此要求没有被强制执行。

### 屏幕旋转变更
> Android O 用户可在快速设定或屏幕设定界面进行操作，在自动旋转和固定竖屏模式之间进行选择。Android P 对竖屏旋转模式引入几大重要变更。竖屏模式更名为 “旋转锁定”，并且只有当自动旋转模式关闭情况下，该模式才能被激活。自动旋转模式暂无变更。

> 当设备处于旋转锁定模式时，用户可以将屏幕锁定在顶层可见 Activity 支持的任何方向。Activity 不能假定屏幕永远处于竖屏状态。如果顶层 Activity 在自动旋转模式下支持多个方向，旋转锁定模式也应给予相同选项，但可根据 Activity 的 screenOrientation 设置规定列外情况 (详情见下表)。

> 请求特定方向 (如，screenOrientation=landscape) 的 Activity 会忽略用户锁定偏好，并与 Android O 行为一致。


### 从 Android P 开始，系统会限制非 SDK 接口的使用

1. 将会有哪些行为?
  
	> 从 Android P 版本开始，我们会限制使用非 SDK 方法与字段：您将无法对其进行访问 —— 无论通过直接调用、反射还是 JNI 等方式。如果加以尝试，您会收到 NoSuchFieldException 或者 NoSuchMethodException 等错误提示。

	> 最初，此项限制主要针对使用频次较低或未使用的接口。在计划与设计工作当中，我们在解决用户及设备制造商反馈的 app 稳定性问题方面，会尊重广大开发者群体意见，同时尽可能降低变更幅度。如果您在迁移至 SDK 方法过程中，遇到技术方面的挑战，我们容许您在 app 更新至最新 API 之前继续使用原有方法。我们计划在今后的 Android 版本中进一步落实这些限制，同时提前通知各位开发人员，确保给予各位充分的时间迁移，并在此期间收集与所需 SDK 接口相关的反馈意见。我们一直强调使用非 SDK 接口带来的风险 —— 在任何版本的 Android 中重构代码以添加功能或修复 bug 时，非 SDK 接口都可能发生变化。因此，如果您的应用程序目前仍依赖于非 SDK 接口，则应尽快开始向 SDK 接口进行迁移。

	> 由于 Java 语言与 C++ 的特性差异，因此该限制将与之前的符号限制略有区别。您不可访问不属于公开 SDK 的类别，同时请确保只使用各个类中被归入官方文档的部分。尤其需要注意的是，当您通过反射等语义与某个类进行交互时，不可访问未在 SDK 中明确列出的方法或字段。

2. 如果缺少 SDK 接口，该如何应对？

	> 我们了解到并不是所有 app 使用的非 SDK 接口都有可替代的 SDK 接口。我们重视您就公共 API 扩展及改进问题提出的反馈意见。如果您觉得在 SDK API 得到对应扩展之前无法停止使用非 SDK 接口，请尽快通过错误追踪器 (bug tracker) 向我们反馈。及时获取这些反馈意见对我们来说非常重要，我们需要利用这些信息调整限制黑名单，从而最大程度减少对开发人员产生的影响，同时着手为今后推出的 Android 平台开发必要的 SDK 接口。

3. 下一步是什么？

	> 在 Android P 开发者预览版当中，您将能够运行现有 app，并在使用非 SDK 接口时收到警告提示 —— 该接口会被列入最终版本的黑名单或灰名单。最好确保您的 app 在开发者预览版中能流畅运行；如果您担心自己的 app 可能受到影响，请特别留意接口兼容性警告信息。

	> 在新开发者预览版发布以及错误追踪器分类条目更新之际，我们会继续监控非 SDK 接口的使用情况。如果官方 SDK 接口已经存在，我们将发布官方指南文档帮助您从常用的非 SDK 接口迁移至 SDK 接口。

## 针对 Android P 开发的应用

### 前台服务
> 针对 Android P 或更高平台开发的应用必须请求 FOREGROUND_SERVICE 权限才能使用前台服务。 FOREGROUND_SERVICE 属于普通级别请求，因此提出请求后，系统会自动授予。

若应用未提出 FOREGROUND_SERVICE 请求就试图创建前台服务，系统会抛出 SecurityException 异常。

### 设备序列号访问限制
> Android 8.0  (API等级26) 已弃用 Build.SERIAL 字段；从 Android P 开始，Build.SERIAL 始终设定为 "UNKNOWN"。此项变更能够保护用户隐私。

> 若您的应用须要访问设备硬件序列号，您应该请求 READ_PHONE_STATE 权限，然后调用 getSerial()。
