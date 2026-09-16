# signinpub

填写Google Play Console的**Sign in details**声明（原名"App access"，登录/
访问权限声明）。这份声明不属于Store listing，却会卡住Publishing
overview页的"Submit changes for review"入口——即使Store listing本身已经
全部填完，这份声明只要"Incomplete"，送审按钮就是禁用的。

## 何时用这个skill

- Publishing overview页出现"X issues found"红色横幅，点开"View issues"看到
  "Incomplete target audience and content declaration"这类跟登录访问相关
  的字样（Sign in details未完成常常连带卡住这一项）。
- 需要给App配置"Sign in details"（审核员用来登录测试App的账号）。
- 路径：`App content` → `Sign in details`（旧称"App access"）。

## 通用陷阱：未点Save就等于没填

跟Play Console其它声明表单同一个坑（见`assetpub`/`datapub`skill）：
Sign in details页面上的所有勾选/输入，在点击**"Save"**之前，都是本地
未持久化状态。**重新打开这个页面的URL会清空未保存的选择**，不会有任何
警告。填到一半要临时切走，先确认已经保存过。

## 核心问题

"Is any part of your app restricted?" —— 只要App有任何登录门槛
（几乎所有需要账号的App都是），选**Yes**。

## 陷阱：点Add之后报"Your changes couldn't be saved"，但看不出哪里错

选Yes后有个**"Sign in details in this declaration provide full access to
all the features and content within this app, including premium or paid
content"**复选框，紧跟着一行小字"Is any other information required?"。
这个复选框是**必答的**（哪怕看起来像可选的辅助说明勾选框），漏勾会导致
点"Add"直接报通用的"Your changes couldn't be saved"，红色的"Is any other
information required?"提示行会出现在复选框正下方——但这行文字本身长得
不像"这里必填"的错误提示，很容易被当成又一条说明文字划过去。

**怎么判断该勾Yes还是No**：这个勾选框问的是"用下面这组账号密码登录，
能不能看到/用到App里的一切，包括付费内容"——不是问"这个账号本身有没有
付费"。如果App没有真正的功能付费墙（比如只是免费版有用量上限，付费只是
提额度，核心界面/功能免费账号也都能看到），这组测试账号事实上就能访问
App的全部内容，**应该勾Yes**，即使这个测试账号本身没花钱升级过。反之，
如果App确实有登录后才解锁的付费专属界面/功能，且这组测试账号没有解锁，
应该勾No并在下方追加信息说明如何单独测试付费部分。

## 填Add sign in details表单：需要一个真实可用的测试账号

Google审核员会拿这组账号密码真的登录App。**不能是虚构的/不存在的账号**，
必须是能连上生产环境（不是本地dev环境）、真实注册成功的账号。

如果项目后端有公开的注册API，最快的方式是直接调用API创建一个专用审核
账号，不用打开App手动注册：

```bash
curl -s -X POST https://<生产域名>/v1/auth/register \
  -H "Content-Type: application/json" \
  -d '{"phone_or_email":"googleplay-reviewer@<域名>","password":"<强密码>","name":"Reviewer", ...其它必填字段按项目API调整}'
```

如果App的核心价值需要"账号里有内容"才能体现（比如一个记录物品位置的
App，空账号看起来什么都做不了），**顺手用同一个token通过API种一两条
示例数据**，别指望审核员自己动手创建内容：

```bash
TOKEN="<上一步返回的token>"
curl -s -X POST https://<生产域名>/v1/items \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $TOKEN" \
  -d '{"title":"...", "location_text":"...", ...其它必填字段}'
```

表单里的字段：
- **Name**：给这组账号起个人类可读的名字（如"Google Play Reviewer Test
  Account"）。**提示文字明确要求"Information must be provided in
  English"**——这个字段和下面的"Any other information required"说明文字
  都要用英文写，不要图省事直接抄项目语言（比如中文项目也要写英文说明）。
- **Username, email address, or phone number** / **Password**：刚创建的
  测试账号凭证。
- **Any other information required to access your app**（可选，但强烈
  建议填）：用一两句英文说清楚登录后去哪个界面能看到App的核心功能，别让
  审核员自己摸索——尤其App没有明显导航文字、靠图标/语音交互的情况。

## 相关

- [`play-console-data-safety-skill`](https://github.com/mosjin/play-console-data-safety-skill)（datapub）——
  同一个Publishing overview送审前置声明的另一半，Data safety问卷。
- [`play-console-assets-skill`](https://github.com/mosjin/play-console-assets-skill)（assetpub）——
  同一个Play Console Publishing流程的另一段，商店列表图形资源上传。
- [`play-console-app-publish`](https://github.com/mosjin/play-console-app-publish-skill)（playpub）——
  Chrome CDP连接方式、App创建、Internal testing上传、订阅商品创建。
