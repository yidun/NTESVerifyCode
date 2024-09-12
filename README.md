# iOS SDK接入教程

全新人机验证方式，高效拦截机器行为，业务安全第一道防线。搭载风险感知引擎，智能切换验证难度，安全性高，极致用户体验。读屏软件深度适配，视障群体也可轻松使用，符合工信部无障碍适配要求。

目前行为式验证码支持的类型有：[智能无感知](https://dun.163.com/trial/sense)、[滑动拼图](https://dun.163.com/trial/jigsaw)、[文字点选](https://dun.163.com/trial/picture-click)、[图标点选](https://dun.163.com/trial/icon-click)、[推理拼图](https://dun.163.com/trial/inference)、[短信上行](https://dun.163.com/trial/sms)、[语序点选](https://dun.163.com/trial/word-order)、[空间推理](https://dun.163.com/trial/space-inference)、[障碍躲避](https://dun.163.com/trial/avoid)、
[语序选词](https://dun.163.com/trial/word-group)、语音验证码。

验证码类型和验证码ID（captchaId）绑定，可以登录[管理后台](https://dun.163.com/dashboard#/login/)灵活切换。

![验证码示例](https://nos.netease.com/yidun/4479496b96530a851f485eb99b7e93ae.png)

## 平台支持（兼容性）

| 条目     | 说明       |
| -------- | ---------- |
| 适配版本 | iOS8以上   |
| 开发环境 | Xcode 11.4 |

## 环境准备

[CocoaPods 安装教程](https://guides.cocoapods.org/using/getting-started.html)

## 资源引入/集成

### 通过 CocoaPods 自动集成

podfile 里面添加以下代码：

```ruby
 source 'https://github.com/CocoaPods/Specs.git' // 指定下载源
 
# 以下两种版本选择方式示例

# 集成最新版SDK:
pod 'NTESVerifyCode'

# 集成指定SDK，具体版本号可先执行 pod search NTESVerifyCode，根据返回的版本信息自行决定:
pod 'NTESVerifyCode', '~> 3.5.5'
```

* 保存并执行 `pod install` 即可，若未执行 `pod repo update`，请执行 `pod install --repo-update`

### 手动集成
* 添加易盾SDK，将压缩包中所有资源添加到工程中（请勾选Copy items if needed选项）
* 添加资源包，进入主工程 Build Phase，在 Copy Bundle Resources 选项中，添加 NTESVerifyCodeResources.bundle 文件
* 添加依赖库，在项目设置target -> 选项卡General ->Linked Frameworks and Libraries添加如下依赖库，如果已存在如下的系统framework，则忽略： 
	* `SystemConfiguration.framework`
	* `JavaScriptCore.framework`
	* `WebKit.framework`

## 项目开发配置

* 在Xcode中找到`TARGETS-->Build Setting-->Linking-->Other Linker Flags`在这个选项中需要添加 `-ObjC`

## 调用示例

```

// 在项目需要使用SDK的文件中引入 #import <VerifyCode/NTESVerifyCodeManager.h>

@property(nonatomic, strong) NTESVerifyCodeManager *manager;

/// 获取验证码管理对象
self.manager = [NTESVerifyCodeManager getInstance];

// sdk 调用
 self.manager.mode = NTESVerifyCodeNormal;
        
[self.manager configureVerifyCode:请输入易盾业务ID timeout:超时时间];
  
// 显示验证码
[self.manager openVerifyCodeView:nil];
```
更多使用场景请参考 [demo](https://github.com/yidun/captcha-ios-demo)

## SDK 方法说明

### 获取 NTESVerifyCodeManager 实例化对象

在项目需要使用 SDK 的文件中先引入`#import <VerifyCode/NTESVerifyCodeManager.h>` 然后再初始化的 SDK，如下：

#### 代码说明：
```
self.manager = [NTESVerifyCodeManager getInstance];
```

#### 返回值说明：

|类型|描述|
|----|----|
|NSObject|验证码实例化对象|

### 初始化

#### 代码说明：

```
[self.manager configureVerifyCode:请输入易盾业务ID timeout:超时时间];
```

* 入参说明：

    |参数|类型|是否必填|默认值|描述|
    |----|----|--------|------|----|
    | businessId |NSString|是|无|易盾分配的业务id|
	| timeout |NSString|是|无|单位：ms，加载验证码的超时时间。这个时间尽量设置长一些，比如7秒以上（7-12秒）|

### 初始化与定制化UI配置

#### 代码说明：

```
 NTESVerifyCodeStyleConfig *styleConfig = [[NTESVerifyCodeStyleConfig alloc] init];
[self.manager configureVerifyCode:请输入易盾业务ID timeout:超时时间 styleConfig:styleConfig];
```


* 基础入参说明：

    |参数|类型|是否必填|默认值|描述|
    |----|----|--------|------|----|
    | businessId |NSString|是|无|易盾分配的业务id|
	| timeout |NSString|是|无|单位：ms，加载验证码的超时时间。这个时间尽量设置长一些，比如7秒以上（7-12秒）|

* styleConfig入参说明：

    |参数|类型|是否必填|默认值|描述|
    |----|----|--------|------|----|
    | radius |NSUInteger|否|无|验证码圆角|
    | verifyBorderColor |NSString|否|无|验证码弹框边框颜色，想要去掉的话可取 transparent 或者与背景色同色|
    | capBarTextAlign |enum|否|无|NTESCapBarTextAlignLeft 居左显示<br> NTESCapBarTextAlignRight 居右显示 <br> NTESCapBarTextAlignCenter 居中显示|
    | capBarBorderColor |NSString|否|无|弹框头部下边框颜色，想要去掉的话可取 transparent 或者与背景色同色 #fff|
    | capBarTextColor |NSString|否|无|弹框头部标题文字颜色|
    | capBarTextSize |NSUInteger|否|无|弹框头部标题文字字体大小|
    | capBarTextWeight |NSString|否|无|弹框头部标题文字字体体重，可设置粗细，<br>参考：capBarTextWeight: normal、bold、lighter、bolder、100、200、300、400、500、600、700、800、900<br>更多详情参考：https://developer.mozilla.org/en-US/docs/Web/CSS/font-weight|
    | capBarTitleHeight |NSUInteger|否|无|弹框头部标题所在容器高度|
    | capBodyPadding |NSUInteger|否|无|验证码弹框 body 部分的内边距，相当于总体设置 capPaddingTop，capPaddingRight，capPaddingBottom，capPaddingLeft|
    | capPaddingTop |NSString|否|无|验证码弹框 body 部分的【上】内边距，覆盖 capPadding 对于上内边距的设置，单位px|
    | capPaddingRight |NSString|否|无|验证码弹框 body 部分的【右】内边距，覆盖 capPadding 对于右内边距的设置，单位px|
    | capPaddingBottom |NSString|否|无|验证码弹框 body 部分的【底】内边距，覆盖 capPadding 对于底内边距的设置，单位px|
    | capPaddingLeft |NSString|否|无|验证码弹框 body 部分的【左】内边距，覆盖 capPadding 对于左内边距的设置，单位px|
    | paddingTop |NSString|否|无|弹框【上】内边距，实践时候可与 capPaddingTop 配合，单位px|
    | paddingBottom |NSString|否|无|弹框【下】内边距，实践时候可与 capPaddingBottom 配合，单位px|
    | capBorderRadius |NSUInteger|否|无|验证码弹框body圆角|
    | borderColor |NSString|否|无|滑块的边框颜色|
    | background |NSString|否|无|滑块的背景颜色|
    | borderColorMoving |NSString|否|无|滑块的滑动时边框颜色，滑动类型验证码下有效|
    | backgroundMoving |NSString|否|无|滑块的滑动时背景颜色，滑动类型验证码下有效|
    | borderColorSuccess |NSString|否|无| 滑块的成功时边框颜色，此颜色同步了文字成功时文字颜色、滑块背景颜色|
    | backgroundSuccess |NSString|否|无|滑块的成功时背景颜色|
    | backgroundError |NSString|否|无| 滑块的失败时背景颜色|
    | borderColorError |NSString|否|无|失败时边框颜色|
    | slideBackground |NSString|否|无| 滑块的滑块背景颜色|
    | textSize |NSUInteger|否|无|滑块的内容文本大小|
    | textColor |NSString|否|无| 滑块内容文本颜色（滑块滑动前的颜色，失败、成功前的颜色）|
    | height |NSUInteger|否|无|滑块的高度|
    | borderRadius |NSUInteger|否|无| 滑块的圆角|
    | gap |NSString|否|无|滑块与验证码视图之间的距离，单位px|
    | slideTip |NSString|否|向右拖动滑块填充拼图|滑块文案描述。默认：向右拖动滑块填充拼图。|
    | executeBorderRadius |NSUInteger|否|无|  组件圆角大小|
    | executeBackground |NSString|否|无|组件背景色|
    | executeTop |NSString|否|无|组件外层容器距离组件顶部距离，单位px|
    | executeRight |NSString|否|无|组件外层容器距离组件右侧距离，单位px|
    | paddingLeft |NSString|否|无|滑块文字，如果相对于整个 bar 居中提示文字，设为 0|
    | loadBackgroundImage |NSString|否|无|加载中时的背景图片地址，需要填入一个 url|
    | loadBackgroundColor |NSString|否|无|加载中时的背景颜色，支持 CSS 所有颜色值|
	
### 验证码元素配置
配置项可以通过 self.manager 点语法配置 例如 self.manager.mode = NTESVerifyCodeNormal。

| 配置项 |类型|是否必填|默认值|描述|
|----|----|--------|------|----|
| frame | CGRect | 否 | NTESVerifyCodeNormal |验证码控件显示的位置,可以不传递。<br>(1)如果不传递或者传递为CGRectNull(CGRectZero)，则使用默认值：topView的居中显示，宽度为屏幕宽度的4/5，高度：view宽度/2.0 + 65。 <br>(2)如果传递，则frame的宽度至少为270；高度至少为：宽度/2.0 + 65。|
| mode | enum | 否 | NTESVerifyCodeNormal | - NTESVerifyCodeNormal 表示传统验证码<br> - NTESVerifyCodeBind 表示无感知验证码<br>（3.5.4及以上版本不需要配置）|
| protocol | enum | 否 |NTESVerifyCodeProtocolHttps | - NTESVerifyCodeProtocolHttps 表示 HTTPS 协议<br> - NTESVerifyCodeProtocolHttp 表示 HTTP 协议|
| alpha | CGFloat | 否 |0.3 |验证码遮罩的透明度<br>范围：0~1，0表示全透明，1表示不透明。默认值：0.3|
| color | UIColor | 否 |blackColor |验证码遮罩的颜色，默认值：黑色|
| fallBackCount | NSUInteger | 否 | 3 | 设置发生第fallBackCount次错误时，将触发降级，取值范围 >=1。默认设置为3次，第三次服务器发生错误时，触发降级，直接通过验证。|
| openFallBack | BOOL | 否 | YES | 设置极端情况下，当验证码服务不可用时，是否开启降级方案。默认开启，当触发降级开关时，将直接通过验证，进入下一步。|
| closeButtonHidden | BOOL | 否 | NO |是否隐藏关闭按钮。默认不隐藏，设置为 YES 隐藏，NO 不隐藏|
| shouldCloseByTouchBackground | BOOL | 否 | YES |点击背景是否可以关闭验证码视图，默认可以关闭。|
| slideIconURL | NSString | 否 | 无 |验证码滑块 icon url，不传则使用易盾默认滑块显示。|
| delegate | id <NTESVerifyCodeManagerDelegate> | 否 | 无 |遵守协议 self.manager.delegate = self|
| lang | enum | 否 | NTESVerifyCodeLangCN | 设置验证码语言类型，具体值参考[LangType 枚举值说明](#2.2)|
| extraData | NSString | 否 | 无 |extraData透传业务数据|
| deviceOrientation | enum | 否 | 无 |- NTESDeviceOrientationUnknown 方向未知 <br>- NTESDeviceOrientationPortrait 固定竖屏，验证码不会跟随设备旋转而旋转  <br> - NTESDeviceOrientationLandscape 固定横屏，验证码不会跟随设备旋转而旋转 |
| mournTheme | BOOL | 否 | 不开启 | 是否开启哀悼主题 |
| fontSize | enum | 否 | 无 |- NTESVerifyCodeFontSizeSmall 小号字体 <br> - NTESVerifyCodeFontSizeMedium 中号字体  <br>- NTESVerifyCodeFontSizeLarge 大号字体   <br>- NTESVerifyCodeFontSizeXlarge 超大号字体|
| userInterfaceStyle | enum | 否 | NTESUserInterfaceStyleLight |- NTESUserInterfaceStyleLight 明亮主题 <br> - NTESUserInterfaceStyleDark 暗黑主题 |
| innerCloseButtonShow | BOOL | 否 |NO|是否显示验证码内的关闭按钮， 默认不显示，设置为YES显示，NO隐藏|
| ipv6 | BOOL | 否 |NO| 默认为 no，传 yes 表示支持ipv6网络。|
| refreshInterval | NSUInteger | 否 |NO| 滑动拼图、智能无感知、短信、语音验证失败后，停留时间。如果需要延长错误提示时间（为了让用户感知到）可自定义配置，单位为 ms|
| allowUploadSystemInfo | BOOL | 否 |YES| 验证码私有化环境配置，是否允许采集系统信息,默认为YES采集，设置NO不采集。|
### 弹出验证码

#### 代码说明：

```
[self.manager openVerifyCodeView:nil];
```
* 入参说明：

    |类型|是否必填|默认值|描述|
    |----|--------|------|----|
    |UIView|否|无|在指定的视图上展示验证码视图，如果传递值为nil，则使用默认值：[[[UIApplication sharedApplication] delegate] window]|

### 弹出验证码、自定义加载页和错误页

#### 代码说明：

```
[[self.manager openVerifyCodeView:nil customLoading:YES customErrorPage:YES];];
```
* 入参说明：

    |类型|是否必填|默认值|描述|
    |----|--------|------|----|
    |UIView|否|无|在指定的视图上展示验证码视图，如果传递值为nil，则使用默认值：[[[UIApplication sharedApplication] delegate] window]|
    |customLoading|否|NO|传YES，自行设置加载页，在 verifyCodeInitFinish 方法里面隐藏加载页。NO，显示易盾加载页|
    |customErrorPage|否|NO|传YES，自行设置错误页，在verifyCodeInitFailed 里显示错误页。NO，显示易盾加载页|

### 弹出无感知验证码、自定义加载页和错误页

#### 代码说明：

```
[[self.manager openVerifyCodeView:nil loadingView nil customLoading:YES customErrorPage:YES];];
```
* 入参说明：

    |类型|是否必填|默认值|描述|
    |----|--------|------|----|
    |UIView|否|无|在指定的视图上展示验证码视图，如果传递值为nil，则使用默认值:[[[UIApplication sharedApplication] delegate] window]|
    |loadingView|否|无|如果是智能无感知验证码，并且是自定义loading页面，loadingView必传|
    |customLoading|否|NO|传YES，自行设置加载页，在 verifyCodeInitFinish 方法里面隐藏加载页。NO，显示易盾加载页|
    |customErrorPage|否|NO|传YES，自行设置错误页，在verifyCodeInitFailed 里显示错误页。NO，显示易盾加载页|
	
### 关闭验证码

#### 代码说明：

```
[self.manager closeVerifyCodeView];
```
### SDK 日志打印

#### 代码说明：

```
[self.manager enableLog:];
```

* 入参说明：

    |类型|是否必填|默认值|描述|
    |----|--------|------|----|
    |BOOL|否|NO|是否开启 SDK 日志打印，YES 表示开启；NO 表示不开启。默认为 NO|
	
### 验证码 SDK 版本号

#### 代码说明：

```
NSString  *version = [self.manager getSDKVersion];
```

* 返回值说明：

    |类型|描述|
    |----|----|
    |NSString|当前 SDK 的版本号|

### 自定义loading文案

#### 代码说明：

```
[self.manager configLoadingText:@""];
```

* 入参说明：

    |类型|是否必填|默认值|描述|
    |----|----|------|----|
    |NSString|否|安全检测中| loadingText，加载中的文案|

### 自定义loading文案，支持gif、png、jpg等格式

#### 代码说明：

```
NSString *bundlePath = [[NSBundle mainBundle] pathForResource:@"face" ofType:@"gif"];
NSData *imageData = [NSData dataWithContentsOfFile:bundlePath];
[self.manager configLoadingImage:nil gifData:imageData];
```

* 入参说明：

    |参数|类型|是否必填|描述|
    |----|----|--------|----|
    | animationImage |UIImage|否|animationImage 单张图片，gifData传空|
	| gifData |NSData|否|gifData 图片格式为gif的二进制数据，animationImage为空|

	
### 验证码协议方法

#### 代码说明：

```
/**
 * 验证码组件初始化完成
 */
- (void)verifyCodeInitFinish;

/**
 * 验证码组件初始化出错
 *
 * @param error 错误信息
 */
- (void)verifyCodeInitFailed:(NSArray *)error;

/**
 * 完成验证之后的回调
 *
 * @param result 验证结果 BOOL:YES/NO
 * @param validate 二次校验数据，如果验证结果为false，validate返回空
 * @param message 结果描述信息
 * @param captchaType 验证码类型
 *
 */
- (void)verifyCodeValidateFinish:(BOOL)result validate:(NSString *_Nullable)validate message:(NSString *_Nullable)message captchaType:(NSString *_Nullable)captchaType;


/**
 * 关闭验证码窗口后的回调
 *
 * @param close 关闭的类型
 */
- (void)verifyCodeCloseWindow:(NTESVerifyCodeClose)close;

```
## 附录
#### 错误码

| code | 含义 |
|------|------|
| 200  | 校验未通过，是因为业务错误，包含超限 |
| 300  | 校验未通过，包含轨迹错误等|
| 432  | 非法业务ID，包含业务到期等|
| 501  | 请求失败，包括网络原因等 |
| 502  | 请求脚本资源失败 |
| 503  | 请求图片资源失败 |
| 505  | 请求音频资源失败 |
| 1000 | 未知错误 |
| 1004  |初始化失败，接口超时 |
|-1 | 未知的错误|
|-999| 请求被取消|
|-1000| 请求的URL错误，无法启动请求|
|-1001| 请求超时|
| -1002|不支持的URL Scheme|
|-1003/-1006|URL的host名称无法解析，即DNS有问题|
|-1004|连接host失败|
|-1005|连接过程中被中断|
|-1007|重定向次数超过限制|
|-1008|无法获取所请求的资源|
|-1009|断网状态|
|-1010|重定向到一个不存在的位置|
|-1011|服务器返回数据有误|
|-1012|身份验证请求被用户取消|
|-1013|访问资源需要身份验证|
|-1014|服务器报告URL数据不为空，却未返回任何数据|
|-1015|响应数据无法解码为已知内容编码|
|-1016|请求数据存在未知内容编码|
|-1017|响应数据无法解析|
|-1018|漫游时请求数据，但是漫游开关已关闭|
|-1019|EDGE、GPRS等网络不支持电话和流量同时进行，当正在通话过程中，请求失败错误码|
|-1020|手机网络不允许连接|
|-1021|请求的body流被耗尽|
|-1100|请求的文件路径上文件不存在|
|-1101|请求的文件只是一个目录，而非文件|
|-1102|缺少权限无法读取文件|
|-1103|资源数据大小超过最大限制|
|-1200|安全连接失败|
|-1201|服务器证书过期|
|-1202|不受信任的根服务器签名证书|
|-1203|服务器证书没有任何根服务器签名|
|-1204|服务器证书还未生效|
|-1205|服务器证书被拒绝|
|-1206|需要客户端证书来验证SSL连接|
|-2000|请求只能加载缓存中的数据，无法加载网络数据|
|-3000|下载操作无法创建文件|
|-3001|下载操作无法打开文件|
|-3002|下载操作无法关闭文件|
|-3003|下载操作无法写文件|
|-3004|下载操作无法删除文件|
|-3005|下载操作无法移动文件|
|-3006|下载操作在下载过程中，对编码文件进行解码时失败|
|-3007|下载操作在下载完成后，对编码文件进行解码时失败|

#### <a id=2.2>LangType 枚举值说明</a>

|语言|枚举值|
|------|-------|
| 美式英文 | NTESVerifyCodeLangENUS |
| 英式英文 | NTESVerifyCodeLangENGB |
| 台湾繁体 | NTESVerifyCodeLangTW   |
| 香港繁体 | NTESVerifyCodeLangHK  |
| 日文 | NTESVerifyCodeLangJP  |
| 韩文 | NTESVerifyCodeLangKR  |
| 泰文 | NTESVerifyCodeLangTL  |
| 越南语| NTESVerifyCodeLangVT  |
| 法语 | NTESVerifyCodeLangFRA   |
| 俄语 | NTESVerifyCodeLangRUS   |
| 阿拉伯语   | NTESVerifyCodeLangKSA   |
| 德语  | NTESVerifyCodeLangDE  |
| 意大利语   | NTESVerifyCodeLangIT  |
| 希伯来语   | NTESVerifyCodeLangHE  |
| 印地语   | NTESVerifyCodeLangHI  |
| 印尼语   | NTESVerifyCodeLangID  |
| 缅甸语   | NTESVerifyCodeLangMY  |
| 老挝语   | NTESVerifyCodeLangLO  |
| 马来语   | NTESVerifyCodeLangMS  |
| 波兰语   | NTESVerifyCodeLangPL  |
| 葡萄牙语   | NTESVerifyCodeLangPT  |
| 西班牙语   | NTESVerifyCodeLangES  |
| 土耳其语   | NTESVerifyCodeLangTR  |
| 荷兰语   | NTESVerifyCodeLangNL  |
| 维吾尔语   | NTESVerifyCodeLangUG  |
| 蒙古语   | NTESVerifyCodeLangMN  |
| 迈蒂利语   | NTESVerifyCodeLangMAI   |
| 阿萨姆语   | NTESVerifyCodeLangAS  |
| 旁遮普语   | NTESVerifyCodeLangPA  |
| 欧里亚语   | NTESVerifyCodeLangOR  |
| 马来亚拉姆语   | NTESVerifyCodeLangML  |
| 卡纳达语  | NTESVerifyCodeLangKN  |
| 古吉拉特语   | NTESVerifyCodeLangGU  |
| 泰米尔语  | NTESVerifyCodeLangTA  |
| 马拉地语  | NTESVerifyCodeLangMR  |
| 泰卢固语  | NTESVerifyCodeLangTE  |
| 阿姆哈拉语   | NTESVerifyCodeLangAM  |
| 毛利语  | NTESVerifyCodeLangMI  |
| 斯瓦西里语   | NTESVerifyCodeLangSW  |
| 尼泊尔语  |  NTESVerifyCodeLangNE |
| 爪哇语  |  NTESVerifyCodeLangJV |
| 菲律宾语  |   NTESVerifyCodeLangFIL   |
| 孟加拉语  |  NTESVerifyCodeLangBN |
| 哈萨克语（西里尔文）  |  NTESVerifyCodeLangKK |
| 白俄罗斯语   |  NTESVerifyCodeLangBE |
| 藏语  |  NTESVerifyCodeLangBO |
| 乌尔都语  |  NTESVerifyCodeLangUR |
| 僧伽罗语  |  NTESVerifyCodeLangSI |
| 高棉语  |  NTESVerifyCodeLangKM |
| 乌兹别克语   |  NTESVerifyCodeLangUZ |
| 阿塞拜疆语   |  NTESVerifyCodeLangAZ |
| 格鲁吉亚语   |  NTESVerifyCodeLangKA |
| 巴斯克语  |  NTESVerifyCodeLangEU |
| 加利西亚语   |  NTESVerifyCodeLangGL |
| 加泰罗尼亚语   |  NTESVerifyCodeLangCA |
| 波斯语  |  NTESVerifyCodeLangFA |
| 乌克兰语  |  NTESVerifyCodeLangUK |
| 克罗地亚语   |  NTESVerifyCodeLangHR |
| 斯洛文尼亚语   |  NTESVerifyCodeLangSL |
| 立陶宛语  |  NTESVerifyCodeLangLT |
| 拉脱维亚语   |  NTESVerifyCodeLangLV |
|爱沙尼亚语  |  NTESVerifyCodeLangET |
| 芬兰语  |  NTESVerifyCodeLangFI |
| 保加利亚语   |  NTESVerifyCodeLangBG |
| 马其顿语  |  NTESVerifyCodeLangMK |
| 波斯尼亚语   |  NTESVerifyCodeLangBS |
| 塞尔维亚语（拉丁文）  |  NTESVerifyCodeLangSR |
| 希腊语  |  NTESVerifyCodeLangEL |
| 罗马尼亚语   |  NTESVerifyCodeLangRO |
| 斯洛伐克语   |  NTESVerifyCodeLangSK |
| 匈牙利语  |  NTESVerifyCodeLangHU |
| 捷克语  |  NTESVerifyCodeLangCS |
| 丹麦语  |  NTESVerifyCodeLangDA    |
| 挪威语  | NTESVerifyCodeLangNN  |
| 瑞典语  |  NTESVerifyCodeLangSV    |
| 巴西葡语  | NTESVerifyCodeLangPTBR   |
| 拉美西语  | NTESVerifyCodeLangESLA   |

#### 验证码类型

| 返回值       | 解释           |
| ------------ | -------------- |
| jigsaw       | 滑动拼图验证码 |
| point        | 文字点选验证码 |
| sms          | 短信上行验证码 |
| intellisense | 智能无感知     |
| avoid        | 躲避障碍验证码 |
| icon_point   | 图标点选验证码 |
| word_group   | 词组验证码     |
| inference    | 图片推理验证码 |
| word_order   | 语序选词验证码 |
| space        | 空间推理验证码 |
| voice        | 语音验证码     |
