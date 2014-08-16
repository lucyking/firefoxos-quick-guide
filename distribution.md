# Distributing Your Apps {#distribution}
# 发布你的应用 {#distribution}

应用已经开发完毕，我们得考虑一下怎样给用户使用。我在[介绍](#introduction)章节提到过，Mozilla不会强迫我们使用官方发布渠道，在哪里发布作品完全由我们自己做主。本章将介绍如何**在[Firefox Marketplace](http://marketplace.firefox.com)外**发布应用。

个人愚见，在以下两个场景你可能会选择在Mozilla Marketplace外发布。
  
 1. 你的应用仅供公司内部或指定用户使用。放到Marketplace的话，所有人都能下载了。若想限制使用人群，你需要在服务器后台增加身份验证或类似的模块。举个例子，*Evernote*首次运行时，它会要求用户先登录到它的服务器。

 2. 你已经拥有庞大的用户群，可直接向他们发布。以*Financial Times*这类新闻报纸为例，他们在自己网站上发布就能覆盖大部分用户。请记住，你的应用可以同时在Firefox Marketplace发布，这样不但能利用自己的发布渠道，还能借助Firefox Marketplace开拓新用户。

托管应用和已打包应用的发布流程差不多，但所用函数不同，因此我会分开讨论。不管是托管应用还是已打包应用，流程大体相同：在主页放一个类似**点击安装**的按钮（超链接），或者提供一个特殊URL，用户点开后自动开始安装。两种方法都会弹出提示框，询问用户是否确定安装此应用。

## 托管应用

<<[托管应用的安装代码](code/distribution/hosted_apps_distribution.js)

在上面的示例代码中，“manifestURL”的值是manifest文件的地址。代码运行时，系统会询问用户是否确定安装此应用，然后根据用户选择返回success或error事件。

若想深入了解这个API，请移步MDN查看[安装应用](https://developer.mozilla.org/docs/Apps/JavaScript_API)页面。

## 已打包应用

已打包应用的安装代码和托管应用差不多，不过不是调用“mozApps.install()”，而是“mozApps.installPackage()”，示例代码如下：

<<[已打包应用的安装代码](code/distribution/packaged_apps_distribution.js)

W> 注意：如果已打包应用是在Marketplace外发布的话，估计Firefox OS 1.0.1设备安装不了。虽然提供了API，但我没试过。有试过的同学，请把结果反馈给我，我会更新本书内容。

## 总结

本章讨论了如何使用*Open Web Apps*的安装管理API在Firefox Marketplace外发布应用。这些API还可以做很多事，例如检测应用是否已安装，如果是，你可以隐藏*点击安装*按钮。 若想进一步了解API的用法，可移步MDN查看[安装应用](https://developer.mozilla.org/docs/Apps/JavaScript_API)页面（没错，我上面提过了，但这一次，点开它吧，里面有重要内容）。

下一章我们将学习如何在Firefox Marketplace发布应用。
