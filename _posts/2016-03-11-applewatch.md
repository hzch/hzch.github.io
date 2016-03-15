---
layout: default
title:  "Apple Watch简述"
subtitle: "简单介绍Apple Watch，包括watchOS 1和watchOS 2。"
date:   2016-03-11 12:00:00 +0800
---

2015年4月10日Apple Watch（以下简称watch）开启预售，24日开始正式上市。笔者2015年3月开始研究watch，由于没有真机，所有前期调研和开发都是在模拟器中完成。经验少，如果发现有什么问题，欢迎指出。

>**PS：**屌丝猿现在都还没有一台Apple Watch。

#Apple Watch

毕竟是新生的系统，开放的接口很少。所以对第一代的watch，就是为了给大家尝尝鲜的。

##硬件结构
整个watch只有两个按钮。Digital Crown和辅助按钮。

**Digital Crown:**功能类似与home 键，可以回到主界面、呼出siri。它还可以转动，用来放大缩小或者滚动视图登。

**辅助按键:**笔者找了好久它的名字，最后的结论是，它就叫辅助按键。类似于power键，长按可以进入到关机界面。在关机界面再次长按，可以杀死当前程序。
它还有个功能，就是进入watch特有的朋友应用。

##软件结构
整个软件结构笔者自己将它分为五个部分。siri、主界面、应用程序、通知（Notification）、Glance。

**siri:** 这个没玩过，估计是iPhone版siri的简化板。

**主界面:**应用图标的集合，承蜂窝状排列。可以通过Digital Crown放大缩小界面。界面中间有一个时钟应用，时钟应用里下滑出现通知栏，上滑出现Glance界面。

**应用程序:**watch的应用程序是无法脱离iPhone独立运行的，watch只是提供一个显示作用，你可以把它认为是一个高级的显示屏。但是，系统应用是可以自己独立运行的，毕竟人家自己的系统，自己加点存储和应用还是可以的。

**Notification:**通知有4种界面。系统通知栏、Short-Look、静态Long-Look、动态Long-Look。Loog-Look是由开发者来有限的控制排版的。系统首先尝试显示动态Long-Look，开发者可以拿到推送的整个消息体，自己解析，自己控制排版。如果构建失败或者超时，系统会显示静态Long-Look，这时，开发者无法拿到推送数据，只能指定推送数据`aps.alert`的显示位置。Long-Look的操作按钮与iPhone共用，无法单独设置。

**Glance:**显示各个应用最关键信息的地方。用户可以通过左右滑，在各个应用间切换。Glances的范围很小，只有一个屏幕，超出的部分会被截断。左右划会切到其他界面，下滑会回到时钟。点击屏幕任何位置，都会进入应用。所以在这边放button是没有意义的。

###设置
苹果有一个癖好，就是把大部分应用的设置都放在一个地方。针对watch，苹果把它大部分设置放在了iPhone端的`Apple Watch`应用中。

有watch的壕们，可以通过这个应用进行与iPhone的配对。

完成配对后，就可以看到许多针对watch的设置了，里面包括了排版、通知、Glances、时间等，还有针对各个应用的设置。

设置里还提供了一个应用商店，里面有所有支持watch的app。


###打包上传
上传app store的流程，和以前一样，watch的包会包含在iphone包的子文件夹中。不用单独提交。
>现在大部分应用都是通过脚本来打包，方便又快捷。根据苹果的解释，如果不用Xcode打包，就需要手动在ipa包里添加一些文件夹。具体方法大家可以在苹果的官方论坛还有stackoverflow上找到。

##watchOS
>**watchOS**是Apple公司推出的，使用在Apple Watch上的操作系统。

###控件
watchOS 刚刚推出，苹果开放出来的控件特别少。所以产品们要合理设计功能，不要为难程序猿。

- Label
- Image
- Group
- Table
- Button
- Switch
- Slider
- Maps
- Date
- Timer
- Menu

**Group:**watch的排版很简单，要么横着排，要么竖着排。因为排版的简单，所以要形成混排，就需要用到Group。

Group其实就是一个容器，可以放控件，也可以放Group，每个Group可以决定里面的内容是横着排还是竖着排。

**Force touch:**watch的屏幕是柔性材质，force touch就是用力按，把屏幕按凹下去。用力按后，就会出现的界面就是menu。

Menu最多只能有4个按钮，每个按钮都是针对整个界面的，你没有办法让Menu针对某个界面的某一个元素。比如你长按一个cell，想把这个cell删掉，这是做不到的，开发者并不知道你按的哪个cell。不过你也可以做一个类似多选的界面，然后force touch，操作这些选中的单元，这样是可以的。

###动画
如果要做动画，只有一种方式，那就是图片序列（感觉瞬间回到解放前）。比如想要做loading界面，就需要一组loading的图片。

###手势
watch屏幕太小，基本上不支持手势。个人认为屏幕小不是关键原因，而是苹果就是不给你开放权限。比如watch上的mail应用，就支持左滑手势。

目前苹果开放的手势只有Force touch和右滑返回。

###通信

![Alt text]({{ site.url }}/photo/1454567475779.png)

一个完整运行的watch应用可以分为三个部分： watch app、watch extension、iPhone app。

**watch app：**运行在手表中，其他两个运行在手机中。watch app由于电量和计算能力问题，只负责UI的显示。而数据则由extension来提供。

**watch extension：** 负责简单的数据准备。因为watch在前台的时间很短暂，用户一旦不操作watch的话，10秒钟内watch就会进入休眠，所以大部分的数据操作还是需要iphone app来做。

**iphone app：**负责大部分的数据操作。

watch app 与extension之间的通信属于设备与设备级别的，苹果通过蓝牙完成这部分通信。

extension 与iphone这边，两个分别属于不同的进程，这两个间的通信属于IPC级别，进程间通信。

####watch app与Extension的通信

![Alt text]({{ site.url }}/photo/1454567509687.png)

这部分的通信，苹果已经帮我们做好了，我们只需要按苹果的推荐的方式进行数据传输就可以了。

由于考虑到watch现在的超低蓄电量，所以我们应该尽量减少这部分通信的数据量。

在需要显示图片时，最佳的实践是把图片放在watch 中，watch extension使用-setImageNamed:方法，把名字传给从手机传到watch。让app自己去显示。

如果是经常用的网络资源图片，也可以使用-addCachedImage:name:方法，把图片存到watch app的缓存中，然后再把名字传过去。缓存大小为20M，超过后系统会自动淘汰掉旧数据。


####extension 与 iphone app间的通信

**watch extension到iphone的通信：**苹果为我们提供了一个很方便的接口`+openParentApplication:reply:`。watch可以主动让iPhone 在后台起来，做一些事情，包括锁屏状态下也可以。iphone做完之后可以通过回调，把结果返回给watch。
>这里需要注意的是，根据实践经验，这个接口所传递的数据必须是可plist化的，比如要传递UIImage，必须转成NSDate再传递，否则会返回错误。

**iphone到watch extension的通信：**这方面，苹果没有提供一个像`openParentApplication`的接口，让iphone主动把消息传给对应的watch app，我们就必须自己想办法来完成这部分的通信。

**第一个办法：**App Group。App Group是用来让同一Group下的app实现数据共享的。
iphone端需要向watch传递消息时，可以向数据共享区写入数据，watch端通过轮训，检测是否有新消息，有新消息后把数据取出来，再做处理。
>这个方法最大的问题在于：watch需要不断轮训。

在和苹果沟通的时候，他们向我们推荐了一个方法.

**DarwinNotification：**这是一个CoreFoundation层的通知。它是系统级的，可以向外广播通知，但是约束比较多，比如通知不能带任何附加消息，只能发出一个name。
所以，DarwinNotification使用时，还是需要结合App Group，iphone app 可以把数据写到App Group ，然后通过Darwin通知watch。watch收到通知，就可以主动将数据取出。

![Alt text]({{ site.url }}/photo/1454567538067.png)

这就是一套相对完整的通信机制了。不过这套方案其实还存在一些问题，我们也在不断的改进当中。比如目前iphone端是无法知道watch是否正在运行。
也就是说，iphone一旦有数据改动，不管watch是否开启，都会向外发出通知。
我们可以在watch启动时告诉iphone说它启动了，但是没有好的办法让watch退出时告诉iphone。

##watchOS 2
`watchOS 2`最大的改变就是可以构建专属手表的独立app了。
>只是说`watch app`在远离手机端时仍然可以使用，每个watch app还是需要一个iOS app的。

![Alt text]({{ site.url }}/photo/1454567538068.png)

![Alt text]({{ site.url }}/photo/1454567538069.png)

从上面的图中，可以很明显地看出，苹果把原来运行在 iPhone 手机上的 `WatchKit Extension` 移到` Apple Watch` 中了。这样做最直接的影响就是，watchOS 2的通信需要重新实现了。幸好苹果提供了一套完整的API，

> watchOS 2使用`WatchConnectivity`进行通信，使用方法见[官方文档](https://developer.apple.com/library/watchos/documentation/WatchConnectivity/Reference/WatchConnectivity_framework/index.html)。

###升级watchOS
首先，你可以选择不将已经存在的`watchOS app`升级到`watchOS 2 app`。
>"If you choose not to migrate your existing Watch app to watchOS 2, your app will still run on watchOS 2. Apps designed for watchOS 1 run in the same way on watchOS 2."

watchOS 2推出后，就存在两种设备。然后如果你希望你的应用跟上新时代，为watchOS 2的用户提供更好的用户体验，可以选择将应用升级到watchOS 2。

开发者可以选择自己的应用可以支持的系统：
- 只提供watchOS 1，则watchOS 1和watchOS 2的设备都可以正常使用旧版；
- 只提供watchOS 2，则watchOS 1设备无法使用，watchOS 2设备可以正常使用新版。
- 提供两种系统支持，则两种设备都可以正常使用自己的版本。

支持的方式很简单，在项目工程中加入相应的`target`即可，系统会自动配置好需要的基础文件和基础设置。
> 添加target时，可以选择watchOS 类型。如果你支持两种系统，你就会发现，你的工程文件会出现很多个target和文件夹，让你眼花缭乱。

watchOS 1和watchOS 2的代码基本上可以共用，但是由于watchOS 2调整了整个系统结构，新增了一系列接口，还加入了很多新元素。所以有些部分需要分别实现。另外如果要加watchOS 2特有功能，也可以在对应target上单独实现。

###watchOS 2 新特点
watchOS 2的Extension不在手机中运行，所以是不能和主APP通过APP Group方式进行共享数据的。不过系统提供了更强大的接口来进行watch与主APP的通信。

watch可以独立运行后，对于watch app的定位就会发生变化。

watch具备一个独立应用的所有功能，网络、存储、逻辑和UI都可以在watch上独立完成。
> watchOS 2后，每个watch app都有自己的沙盒目录，与iPhone上相同，包括`Documents`、`Library`、`tmp`目录。

`watchOS 2` 还给用户提供很多功能`Complication`、`Digital Crown`、`Layout and Animation`、`Taptic Engine`、`Audio and Video`、`Alerts`、`PassKit`等，大家可以查阅相应的文档。

