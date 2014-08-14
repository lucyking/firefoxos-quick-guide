# 搭建Firefox OS开发环境 {#setup}

## Gecko引擎
不同的浏览器使用不同的引擎渲染网页：Google Chrome和Opera用的是Blink（WebKit的一个分支），IE的是Trident，Safari的是WebKit。Mozilla也有自己的引擎，名为Gecko，它被应用于桌面版Firefox、Android版Firefox和Firefox OS中。这些项目都使用了相同的引擎，因此在桌面版Firefox浏览器上进行Firefox OS开发也是可行的。（不过有几个注意事项[^engines]）

[^engines]: 虽然Mozilla的这些项目使用的都是同一个引擎，但是Firefox OS的引擎版本一般都落后于桌面版。因为目前Firefox OS的发布周期比桌面版Firefox浏览器的要长。在实际应用中，这意味着在Firefox OS设备上运行的时候，某些功能不能被支持（或者不能按预期正常运作）。所以一定要确保在Firefox OS设备上测试你的应用。还有一点请谨记，用户设备的Firefox OS版本不尽相同，不是每个用户都能用上最前沿的特性。当设备不支持某些特性时，一定要提供备选方案。

## 你需要安装哪些程序？

为了开发和测试Firefox OS应用，你需要：

 * 最新版的[Firefox浏览器](http://getfirefox.com)
 * 如果你的设备运行的是Firefox OS 1.1，[Firefox OS模拟器](https://addons.mozilla.org/en-US/firefox/addon/firefox-os-simulator/)
 * 编程用的文本编辑器[^editors]

注意：**Firefox OS模拟器是用来测试Firefox OS 1.0至1.1的设备的，如果要测试Firefox OS 1.2+，请使用应用管理器(App Manager)**，我们稍后会详述。

最初写这本书时，**Firefox OS 1.1**是主流，官方的测试方式是使用**Firefox OS 1.1模拟器**。现在Mozilla已经改用新的**应用管理器**，比旧版模拟器好用多了，不过很遗憾，它不能连接Firefox OS 1.1设备。

由于现在有很多人在使用Firefox OS 1.1，且零售市场销售的设备，大部分运行的还是1.1版本，所以我们仍会保留**Firefox OS 1.1模拟器**章节，并沿用以前的内容。同时我们还将演示如何使用**应用管理器**来完成同样的任务。**应用管理器**是当前的主流方式，因此我先讲解**应用管理器**的内容，再讲解旧版模拟器。

**Firefox 29及以上版本**已经内置了**应用管理器**。我们将在[应用管理器章节](#appmanager)详述。

[^editors]: 市面上有许多很棒的编辑器，它们难易不同，功能各异。假如你心尚未有所属，我推荐一个很流行的编辑器，[SublimeText](http://sublimetext.com/)。我个人则比较喜欢[WebStorm](http://www.jetbrains.com/webstorm/)，一个用来开发Web应用并具有完整功能的IDE。

## 应用管理器设置

如果你的Firefox版本是29或以上，那么应用管理器已经内置了。当然，光有应用管理器是不够的。你还需通过应用管理器安装个模拟器，这样当你测试的时候，就不需要拿一部真实的设备连接电脑了。关于应用管理器，Mozilla提供了[很全面的文档](https://developer.mozilla.org/en-US/Firefox_OS/Using_the_App_Manager)，要是你想深入了解一点可去看看。

应用管理器能够管理多个Firefox OS版本，因此你可以同时安装版本1.3, 1.4和2.0。请记住版本号越高，bug也越多。不过既然允许安装多个版本，那最好都装上吧，这样一来我们就能在不同版本的Firefox OS上测试应用了。

让我们看看这个全新的应用管理器，并把接下来要用到的东西都装上。要打开应用管理器，在菜单栏依次展开**工具 -> Web开发者 -> 应用管理器**。

![Where you can find the App Manager](images/originals/locate-app-manager.png)

打开应用管理器后，你会看到类似下图所示的界面

![The App Manager Help](images/originals/app-manager-help.png)

点击**启动模拟器**按钮，选择想要安装的版本。

应用管理器通过ADB与连接的设备进行交互。[安装ADB Helper扩展](https://ftp.mozilla.org/pub/mozilla.org/labs/fxos-simulator/)，能帮你处理一切与ADB相关的操作。

我的建议是，把模拟器的全部版本和ADB Helper都装上。

![The page to download Simulators and the ADB Helper](images/originals/app-manager-add-ons.png)

## 安装Firefox OS 1.1模拟器

假如你的设备运行的是**Firefox OS 1.1**，你得安装**Firefox OS 1.1模拟器**，因为这个新**应用管理器**不支持你的设备。

安装完Firefox后，下一步是安装Firefox OS 1.1模拟器，我们将用它来测试应用。运行Firefox，展开**工具**菜单，选择**附加组件**。

![*Tools* menu with *Add-ons** menu selected](images/originals/tools.png)

在右上角的搜索框，输入**Firefox OS Simulator**进行搜索，点击安装按钮开始安装。

![Add-on manager showing the simulator add-on](images/originals/addons-simulator.png)

W>注意：如果你的Firefox版本为29或以上，且你的设备运行的是**Firefox OS 1.1或更早版本**，那么你需要另一个版本的**Firefox OS 1.1模拟器**，此版本在附加组件商店是找不到的。它尚处于**BETA**阶段，但已经是现在能下载到的最好的版本了。点击链接下载，[Mac OS X](http://ftp.mozilla.org/pub/mozilla.org/labs/r2d2b2g/r2d2b2g-5.0pre7-mac.xpi), [Linux](http://ftp.mozilla.org/pub/mozilla.org/labs/r2d2b2g/r2d2b2g-5.0pre7-linux.xpi) or [Windows](http://ftp.mozilla.org/pub/mozilla.org/labs/r2d2b2g/r2d2b2g-5.0pre7-windows.xpi)。只需把下载到的xpi文件拖进Firefox，然后根据提示进行安装。要是你想研究一下怎么在**Firefox 29**上安装**Firefox OS 1.1模拟器**，可以参考[bug request #1001590 it](https://bugzilla.mozilla.org/show_bug.cgi?id=1001590)。

安装完附加组件，可以在菜单栏的**工具 -> Web开发者 -> Firefox OS Simulator**找到它。

![Where you can find the simulator after is installed](images/originals/tools-web-developer-simulator.png)

此外，你还能浏览[Firefox OS模拟器](https://addons.mozilla.org/en-US/firefox/addon/firefox-os-simulator/)附加组件页面，在那里下载模拟器。

## 总结

在这章我们得知，Firefox浏览器、应用管理器和Firefox OS模拟器（还有一个好用的文本编辑器），就是开发Firefox OS应用所需的全部工具了。

所需的全部工具已经搭建好，在开始首次开发前，让我们先掌握一些基本概念。
