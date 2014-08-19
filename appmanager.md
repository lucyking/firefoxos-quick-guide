# 应用管理器 {#appmanager}

![Firefox OS Simulator Dashboard](images/originals/app-manager-showing-memos.png)

我们在[搭建Firefox OS开发环境](#setup)章节设置好了模拟器，并在[开发首个应用](#firstapp)章节使用了它。现在让我们进一步研究应用管理器，学会用它来完成常见任务。

若想深入了解应用管理器，请移步MDN查看[Firefox OS: Using the App Manager](https://developer.mozilla.org/docs/Mozilla/Firefox_OS/Using_the_App_Manager)文档。

W> 注意: 如果设备运行的Firefox OS 1.1或更早版本，你需要使用Firefox OS 1.1模拟器扩展，而不是应用管理器。Firefox OS 1.1模拟器的内容在下一章会详述。

## 添加应用

可以在应用管理器上添加托管应用和打包应用。让我们分别看看添加方法。

### 添加打包应用

在[开发首个应用](#firstapp)章节，我们已经学习了如何添加打包应用，但为了方便对比，我决定重述一遍要点。

如下图所示，在**应用管理器**面板点击**添加打包应用**进行添加。

![Showing the *Add Packaged App* option that adds a packaged app to the App Manager](images/originals/app-manager-add-packaged-app.png)

按照图中箭头的指示，点击按钮，Firefox会弹出文件选择框。浏览硬盘文件，选择要添加的**包含应用manifest的文件夹**。如果manifest文件配置正确，你会在应用列表看到刚添加的应用。

### 添加托管应用

如果你的应用属于托管应用，你得在Web服务器上测试。别用之前的方法来测试托管应用，因为某些错误只在服务器环境产生，模拟器是不会提示的，例如识别不出manifest的*MIME类型*。在把应用提交到Mozilla Marketplace前，必须解决掉这些错误。

大部分托管应用并不是为Firefox OS单独开发的应用，而是基于响应式设计的网站，它们能够适配不同的设备和分辨率。为了能作为应用正常运行，它们的后台通常都十分复杂，这就是为什么要在真实的Web服务器环境测试的原因。

要想在模拟器运行托管应用，在文本框填上应用的URL，点击**添加托管应用**按钮。

![Adding a hosted app to the App Manager](images/originals/app-manager-adding-hosted-app.png)

点击后，manifest会被验证，如果配置正确，应用将被添加到应用管理器。

## 运行你的应用

点击**启动模拟器**按钮，从已安装的模拟器中选择一个。

模拟器启动后，在应用列表点击**更新**，你的应用将被安装进模拟器。

一旦安装完毕，应用图标会出现在模拟器的主屏幕，直接点击运行即可。

## 更新你的应用

每次在文件改动后，再次测试前，都需要先点击**更新**按钮以更新模拟器上的应用。

## 调试

安装完毕后，可以点击应用列表上的**调试**按钮进行调试。此时你的应用将会在模拟器中运行，并且**调试器**也会打开。

![What button to press](images/originals/app-manager-click-to-debug.png)

点击按钮后出现的界面如下图所示：

![Developer Tools connected to the app running on the simulator](images/originals/app-manager-dev-tools.png)

应用连接上开发者工具后，你就可以进行测试Javascript，调试DOM，编辑CSS等操作了。套用一句创业公司常挂嘴边的话：*不断调整直到好用为止*。

如果应用在模拟器上运行良好，那么是时候在真实设备上测试了。

## 在真实设备上测试应用 

真实设备测试的重要性无可取代。在模拟器上，你对着电脑屏幕靠点击鼠标进行测试；在真实设备上，则要用手指在触摸屏滑动以及按压物理按钮。两者的用户体验和开发体验都相差甚远。

我给大家讲个简单的故事，这个故事体现了在真实设备测试的重要性。几年前，我和Raphael Eckhardt（本书封面的设计者）开发了一个类似
宝石迷情的智力游戏。游戏涉及到一些拖放操作，要把棋子拖放到棋盘。它在模拟器上完美运行。

然后我们把游戏装到了手机上，此时我们才察觉，这个游戏在触摸屏上根本没法玩。手在屏幕操控时，棋盘被手遮住了。更糟的是，棋子太小了，比指尖还小，用户完全看不到他们在拖动什么。一句话，用户体验烂成渣。究其原因，我们一直在凭借小小的鼠标指针在模拟器测试。当使用比指针粗得多的手指去操控时，我们意识到，整个UI不得不推倒重做。

为了避免惨剧重演，记住一定要找部真实设备来测试，哦不，两部，哦不，有多少测多少，越多越好。要经常做简单的原型测试，否则，你会浪费大量宝贵的时间金钱在重做资源文件上。

## 连接Firefox OS设备

假如你有一部Firefox OS设备（所有驱动都已装好），并连接好电脑了，那么你可以通过应用管理器直接把应用安装进设备。应用管理器检测到插入的Firefox OS设备后，它会在底部的**启动模拟器**按钮旁边显示设备ID。

![Device Connected!](images/originals/app-manager-showing-connected-device.png)

点击ID，设备会请求你的许可，点击同意后才能建立远程调试连接。一旦连接建立，就能通过应用列表的**更新**和**调试**按钮进行相应操作了，使用方法和模拟器没两样。

## 总结

总而言之，应用管理器太棒了。它比旧版Firefox OS 1.1模拟器好用太多，因为它提供了更好的开发者工具，而且能够运行多个Firefox OS版本。可以预想到，依靠内置的manifest编辑器以及更多的功能，应用管理器将变得越来越棒。

学到现在，除了感叹和鼓舞，但愿你还掌握了开发Firefox OS应用的流程。

下一章我们来谈谈旧版Firefox OS 1.1模拟器扩展。这是唯一一种连接Firefox OS 1.1设备的方法。下章内容和本章极其相似，实际上，只是界面不同而已。

讲完模拟器后，我们接着会讨论如何发布你的应用。