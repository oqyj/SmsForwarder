# SmsForwarder (短信转发器) 

监控Android手机短信并根据指定规则转发到其他手机：钉钉机器人、企业微信群机器人、企业微信应用消息、邮箱、bark、webhook、Telegram机器人、Server酱、手机短信等。

> ⚠ 首发地址：https://github.com/pppscn/SmsForwarder

> ⚠ 同步镜像：https://gitee.com/pp/SmsForwarder

> ⚠ 网盘下载：https://wws.lanzous.com/b025yl86h 访问密码：`pppscn`

--------

## 特别声明:

* 本仓库发布的`SmsForwarder`项目中涉及的任何代码/APK，仅用于测试和学习研究，禁止用于商业用途，不能保证其合法性，准确性，完整性和有效性，请根据情况自行判断。

* 间接使用代码/APK的任何用户，包括但不限于在某些行为违反国家/地区法律或相关法规的情况下进行传播, `pppscn` 对于由此引起的任何隐私泄漏或其他后果概不负责。

* 如果任何单位或个人认为该项目的代码/APK可能涉嫌侵犯其权利，则应及时通知并提供身份证明，所有权证明，我们将在收到认证文件后删除相关代码/APK。

--------

## 特点和准则：

* **简单** 只做两件事：监听短信 --> 根据指定规则转发

由此带来的好处：
* 简洁:（当时用Pad的时候，看手机验证码各种不方便，网上搜了好久也有解决方案）
    > + AirDroid:手机管理工具功能太多，看着都耗电，权限太多，数据经过三方，账号分级
    > + IFTTT:功能太多，看着耗电，权限太多，数据经过三方，收费
    > + 还有一些其他的APP(例如：Tasker)也是这些毛病
* 省电：运行时只监听广播，有短信才执行转发，并记录最近n条的转发内容和转发状态
* 健壮：越简单越不会出错（UNIX设计哲学），就越少崩溃，运行越稳定持久

### 工作流程：
![工作流程](pic/working_principle.png "工作流程")  


### 功能列表：

- [x] 监听短信，按规则转发（规则：什么短信内容/来源转发到哪里）
- [x] 转发到钉钉机器人（支持：单个钉钉群，@某人）
- [x] 转发到邮箱（支持：SMTP）
- [x] 转发到Bark（支持：验证码/动态密码自动复制）
- [x] 转发到webhook（支持：单个web页面（[向设置的url发送POST/GET请求](doc/POST_WEB.md)））
- [x] 转发到企业微信群机器人
- [x] 转发到企业微信应用消息
- [x] 转发到ServerChan(Server酱·Turbo版)
- [x] 转发到Telegram机器人
- [x] 转发到其他手机短信
- [x] 在线检测新版本、升级
- [x] 清理缓存
- [x] 兼容 Android 6.xx、7.xx、8.xx、9.xx、10.xx
- [x] 支持双卡手机，增加卡槽标识/运营商/手机号(如果能获取的话)
- [x] 支持多重匹配规则
- [x] 支持标注卡槽号码(优先使用)、设备信息；自定义转发信息模版
- [x] 支持正则匹配规则
- [x] 支持卡槽匹配规则
- [ ] 转发规则、发送方配置导出与导入

### 使用流程：
1. 在Android手机上安装`SmsForwarder`本APP后点击应用图标打开
2. 在设置发送方页面，添加或点击已添加的发送方来设置转发短信使用的方式，现在支持钉钉机器人、企业微信群机器人、企业微信应用消息、邮箱、bark、webhook、Telegram机器人、Server酱：
    > 发送方配置见《发送方设置参考》章节
3. 在设置转发规则页面，添加或点击已添加的转发规则来设置转发什么样的短信，现在支持转发全部、根据手机号、根据短信内容、指定卡槽：
    + 当设置转发全部时，所以接收到的短信都会用转发出去。
    + 当设置根据手机号或短信内容时，请设置匹配的模式和值，例如：”手机号 是 10086 发送方选钉钉“。
4. 点击主页面右上角的菜单可进入设置页面，在设置页面可以更新应用查看应用信息提交意见反馈等
5. 在主页面下拉可刷新转发的短信，点击清空记录可删除转发的记录


> ⚠ 该APP打开后会自动后台运行并在任务栏显示运行图标，请勿强杀，退出后请重新开启，并加入到系统白名单中，并允许后台运行

> ⚠ 近期接收到部分用户反馈，`SmsForwarder`无法正确转发通知类短信，涉及 ROM 有华为 EMUI 和 小米 MIUI。这两个系统提供了验证类短信安全保护功能，导致验证码不能正常通过广播获得。以下是解决方案。

> ⚠ 风险警示：转发验证码可能导致您的个人隐私、账户安全受到损害，如果您已经知晓该风险，请继续进行以下操作。

> 华为 EMUI：

> 信息 > 更多 > 设置 > 高级 关闭验证码安全保护开关。

> via:https://club.huawei.com/thread-17770781-1-1.html

> 小米 MIUI：

> 安全中心 > 授权管理 > `短信转发器` > 权限 > 勾选通知类短信

### 发送方设置参考

#### 钉钉机器人

* 任意拉两个人成立一个群组，然后将其他人踢出群
* 在群设置->智能群助手->添加机器人，添加一个新的「自定义机器人」
* 自定义机器人，安全设置->加签，复制到「加签Secret」一栏
* 复制自定义机器人的链接中的"access_token="后面的内容到「设置Token」一栏
* 点击【测试】按钮验证一下

#### 邮件

* 发件服务器：邮箱的SMTP服务器地址，如 smtp.qq.com
* SMTP端口：SMTP服务器的端口号：通常是25；开启SSL之后，通常是465
* 发件账号：用于发送提醒邮件的邮箱，例如 xxxx@qq.com
* 登录密码/授权码：用于发送提醒邮件的密码，QQ邮箱可在邮箱设置中生成一组三方邮件服务专用的授权码，其他邮箱可能需要输入登录密码
* 收件地址：用于接收提醒的邮箱，例如 yyyy@qq.com
* 点击【测试】按钮验证一下

#### Bark（转发IOS最佳体验，强烈推荐）

* 从[App Store](https://apps.apple.com/cn/app/bark-%E7%BB%99%E4%BD%A0%E7%9A%84%E6%89%8B%E6%9C%BA%E5%8F%91%E6%8E%A8%E9%80%81/id1403753865)下载iOS的[Bark App](https://github.com/Finb/Bark)安装
* 打开APP，复制测试URL,粘贴到设置「Bark-Server地址」一栏
* PS.自建服务端参考 [《Bark服务端部署文档》](https://day.app/2018/06/bark-server-document/)
* 点击【测试】按钮验证一下

#### Webhook

* 参考 [向设置的url发送POST/GET请求](doc/POST_WEB.md)

#### 企业微信群机器人

* 任意拉两个人成立一个群组，然后将其他人踢出群
* 在会话列表右键点击刚创建的群->添加群机器人->新创建一个机器人->自定义机器人名称
* 复制WebHook地址到「设置WebHook地址」一栏
* 点击【测试】按钮验证一下

#### 企业微信应用消息

* 登录 [企业微信管理后台](https://work.weixin.qq.com/wework_admin/loginpage_wx?from=myhome_qyh_redirect)
* 在 [我的企业](https://work.weixin.qq.com/wework_admin/frame#profile) 复制「企业ID」
* 在 [应用管理](https://work.weixin.qq.com/wework_admin/frame#apps) 中 [创建应用](https://work.weixin.qq.com/wework_admin/frame#apps/createApiApp)
* 进入自建应用，复制「AgentId」和「Secret」
* 默认是 @all (应用的可见范围内所有人)，如果只想通知一个人，在「指定成员」一栏填写员工账号
* 点击【测试】按钮验证一下

#### Server酱·Turbo版

* 微信扫码登录 [Server酱·Turbo版](https://sct.ftqq.com/login)
* 在 [消息通道](https://sct.ftqq.com/forward) 配置消息通道设置
* 在 [SendKey](https://sct.ftqq.com/sendkey) 栏目复制SendKey，粘贴到设置「设置Server酱·Turbo版的SendKey」一栏
* 点击【测试】按钮验证一下

#### Telegram机器人（需自备梯子）

* 与 @BotFather 私聊，申请 Bot
    * /newbot 后输入机器人昵称
    * 然后输入机器人的用户名（建议：使用密码生成器生成随机字符串，避免一直重复尝试；用户名必须用 bot 作为结尾）
    * /token 获取apiToken，然后输入上面机器人的用户名
    * 获得apiToken，格式参考：1234567890:ABCDEFGHIJKLMNOPQRSTUVWXYZ
* 复制 apiToken 到「设置Telegram机器人的ApiToken」一栏
* 获取自己（或群组）的ChatID，粘贴到「设置被通知人的ChatId」一栏
    * 跟自己的机器人聊天，随便说点什么；或者创建一个群组，把机器人拉入群组，在群组里随便说点什么。
    * 然后打开这个链接 `https://api.telegram.org/bot<apiToken>/getUpdates` 获取（PS.注意<apiToken>换成你自己的）
    * ChatID 取值 result->message->chat->id (个人是纯数字；群组是负数，type：group；)
* 点击【测试】按钮验证一下

#### 其他手机短信

* 指定发送卡槽：1、原进原出——哪个卡槽收到的短信就用哪张卡转发短信出去；2、SIM1/SIM2——固定卡槽转发短信；
* 设置接收手机，多个号码以半角分号分隔，例如：15888888888;19999999999
* 仅当无网络时启用：建议开启，毕竟发短信1毛/条还挺贵的（套餐有送的/土豪可以忽视它）

### 应用截图：

| | |
|  ----  | ----  |
| 前台服务常驻状态栏 | 应用主界面 |
| ![前台服务常驻状态栏](pic/taskbar.jpg "前台服务常驻状态栏") | ![应用主界面](pic/main.jpg "应用主界面") |
| 转发规则 | 转发详情 |
| ![转发规则](pic/rule.jpg "转发规则") | ![转发详情](pic/maindetail.jpg "转发详情") |
| 添加/编辑转发规则测试 | 多重匹配规则 |
| ![添加/编辑转发规则](pic/ruleset.jpg "添加/编辑转发规则") | ![多重匹配规则](pic/multimatch.jpg "多重匹配规则")|
| 支持以下转发方式（发送方） | 添加/编辑发送方钉钉 |
| ![发送方](pic/sender.jpg "发送方") | ![添加/编辑发送方钉钉](pic/sendersetdingding.jpg "添加/编辑发送方钉钉") |
| 添加/编辑发送方邮箱 | 添加/编辑发送方Bark |
| ![添加/编辑发送方邮箱](pic/sendersetemail.jpg "添加/编辑发送方邮箱") | ![添加/编辑发送方Bark](pic/sendersetbark.jpg "添加/编辑发送方Bark") |
| 添加/编辑发送方网页通知 | 添加/编辑发送方企业微信群机器人 |
| ![添加/编辑发送方网页通知](pic/sendersetwebnotify.jpg "添加/编辑发送方网页通知") | ![添加/编辑发送方企业微信群机器人](pic/sendersetqywechat.jpg "添加/编辑发送方企业微信群机器人") |
| 添加/编辑发送方Telegram机器人 | 添加/编辑发送方Server酱·Turbo版 |
| ![添加/编辑发送方Telegram机器人](pic/sendertelegram.jpg "添加/编辑发送方Telegram机器人") | ![添加/编辑发送方Server酱·Turbo版](pic/senderserverchan.jpg "添加/编辑发送方Server酱·Turbo版") |
| 添加/编辑发送方企业微信应用 | 应用设置 |
| ![添加/编辑发送方企业微信应用](pic/sendersetqywxapp.jpg "添加/编辑发送方企业微信应用") | ![应用设置](pic/setting.jpg "应用设置") |
| 关于/在线升级 | 支持正则匹配规则 & 支持卡槽匹配规则 |
| ![在线升级](pic/update.jpg "在线升级") | ![支持正则匹配规则 & 支持卡槽匹配规则](pic/regex.jpg "支持正则匹配规则 & 支持卡槽匹配规则") |
| 转发短信模板增加卡槽标识 | 添加/编辑发送方其他手机短信 |
| ![转发短信模板增加卡槽标识](pic/siminfo.jpg "转发短信模板增加卡槽标识") | ![添加/编辑发送方其他手机短信](pic/sendersetsms.jpg "添加/编辑发送方其他手机短信") |

--------

## 更新记录：（PS.点击版本号下载对应的版本）

+ [v1.0.0](app/release/SmsForwarder_release_20210213_1.0.0.apk) 优化后第一版
+ [v1.1.0](app/release/SmsForwarder_release_20210214_1.1.0.apk) 新增在线升级、缓存清理、加入QQ群功能
    + [v1.1.1](app/release/SmsForwarder_release_20210215_1.1.1.apk) 更新应用/通知栏图标
    + [v1.1.2](app/release/SmsForwarder_release_20210218_1.1.2.apk) 获取系统(ROM)类别及版本号，MIUI通知栏显示标题
    + [v1.1.3](app/release/SmsForwarder_release_20210218_1.1.3.apk) AlertDialog增加滚动条，避免参数过长时无法点击按钮
+ [v1.2.0](app/release/SmsForwarder_release_20210219_1.2.0.apk) 重写SMTP邮件发送（推荐升级）
    + [v1.2.1](app/release/SmsForwarder_release_20210226_1.2.1.apk) 修复bark-server升级到2.0后的兼容性问题
    + [v1.2.2](app/release/SmsForwarder_release_20210302_1.2.2.apk) 【预发布】短信模板增加卡槽标识（SIM1_中国联通_Unknown 或 SIM2_中国移动_+8615866666666）
    + [v1.2.3](app/release/SmsForwarder_release_20210302_1.2.3.apk) 【预发布】转发日志列表/详情增加卡槽标识（SIM1 或 SIM2）
+ [v1.3.0](app/release/SmsForwarder_release_20210303_1.3.0.apk) 支持双卡手机，增加卡槽标识/运营商/手机号(如果能获取的话)
+ [v1.4.0](app/release/SmsForwarder_release_20210304_1.4.0.apk) 支持多重匹配规则
    + [v1.4.1](app/release/SmsForwarder_release_20210304_1.4.1.apk) 设置中允许关闭页面帮助/表单填写提示
+ [v1.5.0](app/release/SmsForwarder_release_20210305_1.5.0.apk) 新增转发到企业微信应用消息
    + [v1.5.1](app/release/SmsForwarder_release_20210310_1.5.1.apk) 解决Android 9.xx、10.xx收不到广播问题
    + [v1.5.2](app/release/SmsForwarder_release_20210311_1.5.2.apk) 支持标注卡槽号码(优先使用)、设备信息；自定义转发信息模版
+ [v1.6.0](app/release/SmsForwarder_release_20210312_1.6.0.apk) 优化获取SIM信息（兼容高版本Android） & 自动填写设备备注 & 自动填充卡槽信息到SIM1备注/SIM2备注 & 支持卡槽匹配规则 & 支持正则匹配规则
    + [v1.6.1](app/release/SmsForwarder_release_20210312_1.6.1.apk) 新增转发到Server酱·Turbo版
    + [v1.6.2](app/release/SmsForwarder_release_20210312_1.6.2.apk) 新增转发到Telegram机器人
    + [v1.6.3](app/release/SmsForwarder_release_20210313_1.6.3.apk) 转发到webhook支持GET方式（节点改变，原配置要重新编辑）；兼容Android5.0（待验证，仅minSdkVersion改为21）；修复钉钉机器人没启用加签时url拼接错误问题
    + [v1.6.4](app/release/SmsForwarder_release_20210313_1.6.4.apk) Android8.1以下手机重启后尝试启动主界面，以便动态获取权限（修复开机自启后无法转发短信，要打开软件后才会转发短信的问题）
+ [v1.7.0](app/release/SmsForwarder_release_20210318_1.7.0.apk) 新增转发到其他手机短信 & 避免热插卡时FC & 规则展示优化 & 获取多卡信息&获取卡槽备注优化 & 新增恢复初始化配置
    + [v1.7.1](app/release/SmsForwarder_release_20210321_1.7.1.apk) 新增转发记录的转发状态(成功/失败&应答信息)
    + [v1.7.2](app/release/SmsForwarder_release_20210325_1.7.2.apk) 新增V1版证书签名，避免部分低版本系统(Android 6.x)无证书错误 & 发送方邮箱允许自定义发件人昵称
    + [v1.7.3](app/release/SmsForwarder_release_20210331_1.7.3.apk) 修复“设置匹配模式”默认选择BUG & 转发到webhook时返回http状态200即为成功 & 转发到其他手机短信支持长短信合并
    + [v1.7.4](app/release/SmsForwarder_release_20210715_1.7.4.apk) 修复转发企业微信群机器人碰到"被截断问题 & 转发到webhook时忽略ssl证书校验（提高自建服务端兼容性） & 转发telegram时将 # 替换为 井，避免被当作标签 & 隐私保护，发送方设置中敏感信息(密码/token/secret等)用星号显示 & 更新友盟基础组件库 & 解决“设置页面关闭卡槽信息，同时使用默认模板时，发送消息卡槽信息仍显示”

--------

## 反馈与建议：

+ 提交issues 或 pr
+ 加入交流群

| | |
|  ----  | ----  |
| QQ交流群：562854376 | 微信交流群 |
| ![QQ交流群：562854376](pic/qqgroup.jpg "QQ交流群：562854376") | ![微信交流群](pic/wechat.jpg "微信交流群") |

## 感谢

> 本项目使用(或借鉴)了以下项目(或部分代码)，在此表示衷心的感谢！

+ https://github.com/xiaoyuanhost/TranspondSms (基于此项目优化改造)
+ https://github.com/square/okhttp （网络请求）
+ https://github.com/xuexiangjys/XUpdateAPI （在线升级）
+ https://github.com/mailhu/emailkit （邮件发送）
+ https://github.com/alibaba/fastjson (Json解析)

## LICENSE    
BSD

## 如果觉得本工具对您有所帮助，给个小星星鼓励一下！

[![starcharts stargazers over time](https://starchart.cc/pppscn/SmsForwarder.svg)](https://github.com/pppscn/SmsForwarder)