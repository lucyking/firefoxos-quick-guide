# Firefox OS模拟器{#simulator}

![Firefox OS Simulator Dashboard](images/originals/simulator-dashboard.png)

W>注意：本章内容适用于运行Firefox OS 1.1的设备。当前的主流测试和调试方法是使用**应用管理器**，我们在上一章已详细介绍。本章仅供需要在Firefox OS 1.1设备测试的人参考。

W>注意：如果你的Firefox版本为29或以上，且设备运行的是**Firefox OS 1.1或更早版本**，那么你需要另一个版本的**Firefox OS 1.1模拟器**，此版本在附加组件商店是找不到的。它尚处于**BETA**阶段，但已经是现在能下载到的最好的版本了。点击链接下载，[Mac OS X](http://ftp.mozilla.org/pub/mozilla.org/labs/r2d2b2g/r2d2b2g-5.0pre7-mac.xpi), [Linux](http://ftp.mozilla.org/pub/mozilla.org/labs/r2d2b2g/r2d2b2g-5.0pre7-linux.xpi) or [Windows](http://ftp.mozilla.org/pub/mozilla.org/labs/r2d2b2g/r2d2b2g-5.0pre7-windows.xpi)。只需把下载到的xpi文件拖进Firefox，然后根据提示进行安装。要是你想研究一下怎么在**Firefox 29**上安装**Firefox OS 1.1模拟器**，可以参考[bug request #1001590 it](https://bugzilla.mozilla.org/show_bug.cgi?id=1001590)。

我们在[搭建Firefox OS开发环境](#setup)章节安装好了模拟器，并在[开发首个应用](#firstapp)章节使用了它。现在让我们进一步研究模拟器的功能，学会用它来完成常见任务。

若想深入了解模拟器，请移步MDN查看[Firefox OS Simulator](https://developer.mozilla.org/en-US/docs/Tools/Firefox_OS_Simulator)文档。

请谨记，如果设备运行的是Firefox OS 1.2+版本，你需要使用应用管理器，而不是Firefox OS模拟器。应用管理器的内容已在上一章详述。

## 添加应用

可以在应用管理器上添加托管应用和已打包应用。让我们分别看看添加方法。

### 添加已打包应用

在[开发首个应用](#firstapp)章节，我们已经学习了如何添加已打包应用，但为了方便对比，我决定重述一遍要点。

如下图所示，在**模拟器面板**点击**添加文件夹**来添加已打包应用。

![Showing the *Add Directory* button that adds a packaged app to the simulator](images/originals/simulator-add-directory.png)

按照图中箭头的指示，点击按钮，Firefox会弹出文件选择框。浏览硬盘文件，选择要添加的**应用manifest**。如果manifest文件和起始页的配置正确无误，应用将被成功添加，同时模拟器会开始运行它。假如manifest文件或其他地方出错，面板上会显示错误报告。

![Example of an invalid manifest](images/originals/simulator-invalid-manifest.png)

每次应用改动后，都需要先点击**刷新**按钮以更新模拟器上的应用版本（也可在模拟器内按下CMD/CTRL+R来刷新）。

### 添加托管应用

如果你的应用属于托管应用，你得在Web服务器上测试。别用之前的方法来测试托管应用，因为某些错误只在服务器环境产生，模拟器是不会提示的，例如识别不出manifest的*MIME类型*。在把应用提交到Mozilla Marketplace前，必须解决掉这些错误。

大部分托管应用并不是为Firefox OS单独开发的应用，而是基于响应式设计的网站，它们能够适配不同的设备和分辨率。为了能作为应用正常运行，它们的后台通常都十分复杂，这就是为什么要在真实的Web服务器环境测试的原因。

要想在模拟器运行托管应用，在顶部文本框填上应用的URL，点击**添加URL**按钮。

![Adding a hosted app to the simulator](images/originals/simulator-add-url.png)

点击后，manifest会被验证，如果配置正确，应用将被成功添加，同时模拟器会开始运行它。跟已打包应用一样，假如manifest配置不正确，面板会显示错误报告（比如“提交到marketplace的应用要求至少有一个128x128图标”）。

每次应用改动后，都需要先点击**刷新**按钮以更新模拟器上的应用版本（也可在模拟器内按下CMD/CTRL+R来刷新），这点也跟已打包应用一样。

## 调试

安装完毕后，可以点击应用旁边的**连接**按钮进行调试。此时你的应用将会在模拟器中运行，并且**调试器**也会打开，与应用连接。

![What button to press](images/originals/simulator-press-connect.png)

点击按钮后出现的界面如下图所示：

![Developer Tools connected to the app running on the simulator](images/originals/simulator-connected.png)

应用连接上开发者工具后，你就可以进行测试Javascript，调试DOM，编辑CSS等操作了。套用一句创业公司常挂嘴边的话：*不断调整直到好用为止*。

如果应用在模拟器上运行良好，那么是时候在真实设备上测试了。

## 在真实设备上测试应用

真实设备测试的重要性无可取代。在模拟器上，你对着电脑屏幕靠点击鼠标进行测试；在真实设备上，则要用手指在触摸屏滑动以及按压物理按钮。两者的用户体验和开发体验都相差甚远。

我给大家讲个简单的故事，这个故事体现了在真实设备测试的重要性。几年前，我和Raphael Eckhardt（本书封面的设计者）开发了一个类似
宝石迷情的智力游戏。游戏涉及到一些拖放操作，要把棋子拖放到棋盘。它在模拟器上完美运行。

然后我们把游戏装到了手机上，此时我们才察觉，这个游戏在触摸屏上根本没法玩。手在屏幕操控时，棋盘被手遮住了。更糟的是，棋子太小了，比指尖还小，用户完全看不到他们在拖动什么。一句话，用户体验烂成渣。究其原因，我们一直在凭借小小的鼠标指针在模拟器测试。当使用比指针粗得多的手指去操控时，我们意识到，整个UI不得不推倒重做。

为了避免惨剧重演，记住一定要找部真实设备来测试，哦不，两部，哦不，有多少测多少，越多越好。要经常做简单的原型测试，否则，你会浪费大量宝贵的时间金钱在重做资源文件上。

你可以到[Geeksphone Shop](http://shop.geeksphone.com/en/)买部Firefox OS开发者预览版手机。我推荐[Geeksphone Keon](http://www.geeksphone.com/)，因为它的硬件配置和Mozilla合作伙伴推出的设备相近。

面向消费者的Firefox OS设备已经在多个国家推出，假如你身处的国家刚好是其中之一，也可通过这种途径购买。还有个办法，把Android机刷成Firefox OS（只有几部特定型号才能刷，变砖了可别怪我！）。我不推荐此法，除非你是个喜欢折腾的高手。

## 连接Firefox OS设备

假如你有一部Firefox OS设备（所有驱动都已装好），并连接好电脑了，那么你可以通过模拟器直接把应用安装进手机。模拟器检测到插入的Firefox OS手机后，它会提示**设备已连接**。

![Device Connected!](images/originals/simulator-device-connected.png)

如果手机已经连接好，模拟器会在**刷新**和**连接**旁边新增一个**推送**按钮。点击它，设备屏幕出现个**许可请求窗口**，询问是否确定安装此应用。

![Which button to press to push apps to the connected device](images/originals/simulator-press-push.png)

下图是许可请求界面。

![Not the best picture in the world but shows the permission screen (sorry for the face it was 4:25 AM)](images/originals/simulator-remote-push.jpg)

你可以通过*远程调试*连接调试器来调试应用。

## 总结

总而言之，Firefox OS模拟器在开发某些应用时很好用。但假如你想用模拟器开发一个适配多种设备的应用，它的局限性暴露无遗（例如，目前它模拟不了Firefox OS在平板上的使用情景）。

学到现在，除了感叹和鼓舞，但愿你还掌握了开发Firefox OS应用的流程。

下一章我们来谈谈全新的应用管理器，它是Mozilla新引入的应用调试系统，相比Firefox OS模拟器而言，它拥有更多功能。