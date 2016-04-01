---
layout: default
title:  "制作自己的Xcode插件"
subtitle: "Xcode用了3年，从Xcode4到Xcode7，一直没有尝试过自己写插件，心血来潮，来搞一搞。"
date:   2016-03-11 12:00:00 +0800
---

## 写在前头  

Xcode在启动的时候，会寻找位于~/Library/Application Support/Developer/Shared/Xcode/Plug-ins文件夹中的后缀名为.xcplugin的bundle作为插件进行加载运行。我们利用这一特性，可以给Xcode注入代码。

Xcode用了3年，从Xcode4到Xcode7，一直没有尝试过自己写插件，心血来潮，来搞一搞。

首先，上网查教程，发现一篇喵神的[ Xcode 4 插件制作入门](https://onevcat.com/2013/02/xcode-plugin/)，虽然有点久了，但是很多基本东西都是不变的。

## 跟着做个Hello world  

现在最新的Xcode的为7.3，以此为例。

### 1. 创建工程  

创建工程，OSX，Framework & Library，选择Bundle。将Bundle Extension改为xcplugin。

> Xcode没有提供插件的模版，需要自己通过Bundle改。现在网络上很多人都写了插件的模版，如果觉得自己改麻烦，可以上网看看。

![Alt text]({{ site.url }}/photo/1459490187024.png)

### 2. 修改Info.plist  

![Alt text]({{ site.url }}/photo/1459492823282.png)

- `Principal class`：主类名，告诉Xcode要用哪个类，该类需要自己创建。
- `XCPluginHasUI`：NO，没有UI。
- `XC4Compatible`：YES，适配Xcode4。
> 不明白为什么必须是XC4，笔者改为XC5Compatible、XC7Compatible，都不行。

- `DVTPlugInCompatibilityUUIDs`：支持的Xcode列表。
> 在 Xcode 5 中， Apple 为了防止过期插件导致的在 Xcode 升级后 IDE 的崩溃，添加了一个 UUID 的检查机制。只有包含声明了适配 UUID，才能够被 Xcode 正确加载。UUID可以在Xcode.app的Info.plist中看，命令行：`defaults read /Applications/Xcode.app/Contents/Info DVTPlugInCompatibilityUUID`。

### 3. 更改bundle生成目录  

> 这步可以不做，可以手动把生成的bundle移动到`~/Library/Application Support/Developer/Shared/Xcode/Plug-ins`。

![Alt text]({{ site.url }}/photo/1459492604664.png)

修改工程文件的`Build Setting`：
- `Installation Build Products Location`     设置为`$({HOME}`
- `Installation Directory` 设置为`/Library/Application Support/Developer/Shared/Xcode/Plug-ins`
> 以上两个设置目录，第二个是第一个的相对地址，也可以把第一个设置为空，第二个设置为$({HOME}/Library/Application Support/Developer/Shared/Xcode/Plug-ins

- `Deployment Location` 设置为 `YES`
> 告诉Xcode不要用设置里的build location，而是用Installation Directory来确定build后放哪儿。

- `Skip Install` 设置为`NO`
> 顾名思义，默认为YES，会跳过安装，bundle会放在另外的地方。

### 4. 新建主类  

随便建个NSObject的子类即可，然后把`Principal class`改为这个类。

在.m文件中加入类方法`+pluginDidLoad:`

``` objectivec
+ (void)pluginDidLoad:(NSBundle *)plugin { 
    NSLog(@"Hello World"); 
}
```

### 5.Build && Restart  

运行工程、完全退出Xcode、重新打开Xcode、Load Bundle、Done。

![Alt text]({{ site.url }}/photo/1459494561504.png)

> 在重新打开Xcode前，可以先打开控制台查看系统日志，如果看到有Hello World，那么恭喜，你成功了。