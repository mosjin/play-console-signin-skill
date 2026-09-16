[English](README.en.md) | **简体中文**

# play-console-signin

Claude Code skill：填写Google Play Console的Sign in details（原App
access）声明。这份声明不属于Store listing，却会卡住Publishing overview
页的"Submit changes for review"入口。通用skill，不绑定任何具体项目
目录。内容默认简体中文；代码、CLI指令、文件路径、技术术语保留原文。

## 安装

```
/plugin marketplace add mosjin/play-console-signin-skill
/plugin install signinpub@play-console-signin
```

实际调用用短名`signinpub`（如`/signinpub:version`、
`/signinpub:upgrade`），沿用`mosjin/android-voice-test-kit`的约定——
描述性的marketplace/repo名下配一个简短的插件命令名。

## 覆盖内容

- Sign in details声明的核心陷阱：选Yes后有个"这组账号能否访问全部功能
  含付费内容"的复选框是**必答的**，漏勾会让"Add"报通用的
  "Your changes couldn't be saved"，容易被当成又一条说明文字看漏。
- 判断这个复选框该勾Yes还是No的实际标准：问的是"能不能访问全部内容"，
  不是"这个账号本身有没有付费"——没有真正付费墙的App（付费只是提额度）
  应该勾Yes。
- 创建可用测试审核账号的方法：直接调用项目后端的注册API，在生产环境
  创建一个专用账号，并顺手用同一个token种几条示例数据，别指望审核员
  自己动手建内容。
- 跟`assetpub`/`datapub`共享的通病：表单是"未点Save就等于没填"，重新
  打开页面URL会静默清空未保存的输入。

## 相关

- [`play-console-data-safety-skill`](https://github.com/mosjin/play-console-data-safety-skill)（datapub）——
  同一个Publishing overview送审前置声明的另一半，Data safety问卷。
- [`play-console-assets-skill`](https://github.com/mosjin/play-console-assets-skill)（assetpub）——
  同一个Play Console Publishing流程的另一段，商店列表图形资源上传。
- [`play-console-app-publish`](https://github.com/mosjin/play-console-app-publish-skill)（playpub）——
  Chrome CDP连接方式、App创建、Internal testing上传、订阅商品创建。
