{frontmatter}

## 致谢辞

感谢我的太太Lili，她是世上最好的太太！

感谢Mozilla对我们的长久信任，感谢他们维持着网络的开放和自由，永远把用户放在首位！

感谢了不起的巴西Mozilla社区，感谢他们对我的完全接纳！

感谢棒得不能再棒的GSoC导师Marcos Caceres、Mozilla WebAPI团队、Mozilla技术布道团队以及Mozilla开发者团队！

感谢2013年的Google编程之夏！这个活动非常精彩！

我还要对Ryuno-Ki、chisophugis、ghost、dholbert、marcoscaceres表示衷心的感谢，他们花费了时间努力提交贡献，使得本书更加完善。

## 本书永不停更

我打算经常更新本书，扩充内容，修复读者发现的问题。由于Firefox OS的一些API还处于实施阶段，你一定想确保以后阅读到的是最新版本。

## 关于作者

在本书能多处看到我的个人意见和决定，尤其是我觉得此举能更容易解释我想法的时候，虽然其他程序员可能不会这么做。在表达个人意见时，我会尽量清晰地论证我的观点。总之，如果我的表述不当，我会校正文本，更新本书。更多信息请看“反馈与贡献”部分。

## 写这本书的缘由

起初，我一直在利用业余时间写作。后来由于我的Google Summer of Code (GSoC)导师Marcos Caceres的鼎力相助，本书成为了我GSoC项目的一部分，此项目的目标是创造有用的Firefox OS开发者资源。因此，非常感谢Google对活动的赞助，以及感谢Mozilla WebAPI团队能让我整夏参与其中。

## 获取最新动态

本书**免费**发布在[Leanpub](http://leanpub.com)。

如果是在[Leanpub的本书页面](http://leanpub.com/quickguidefirefoxosdevelopment)下载的，可以通过注册邮箱自动获取更新。本书计划每月多更。如果书是从朋友或其他网站获得的，你应该考虑去上述页面下载，并订阅邮件，以确保接收到更新通知。

## 捐助

写书需要耗费大量工夫，在2013年的Google编程之夏闭幕后，我愿在余生倾注更多的精力投身于此类活动。如果你觉得本书有帮助（或很酷），可以在Leanpub下载页面，拖动价格滑块，选择0到任意金额进行捐助。喜欢用PayPal的同学，可捐助到*agarzia@mac.com*账号。

不管捐助与否，你都应该在下载页面填上你的邮箱地址，以便第一时间接收更新通知！

## 联系作者

评论和反馈可发送邮件至[fxosquickguide@andregarzia.com](mailto:fxosquickguide@andregarzia.com)。我的个人主页是[http://andregarzia.com](http://andregarzia.com)。Twitter账号是[@soapdog](http://twitter.com/soapdog)。

如果你想帮忙完善本书内容，请看“反馈与贡献”部分。

## 封面插图

封面由巴西的一位设计师和插画师，Raphael Eckhardt，创作。你可以在[http://raphaeleckhardt.com/](http://raphaeleckhardt.com/)查看他的作品和联系方式（他是自由职业者）。

## 面向人群

本书面向已有一定的HTML, CSS和JavaScript基础，想为Firefox OS开发应用的读者。HTML, CSS和JavaScript的教程不在本书讨论范围。不过我会提供一些不错的参考书籍链接。

## 最佳实践 vs 新手友好

有经验的开发者会留意到，本书的示例代码并不都是最佳写法。虽然我一直在避免反模式，但在本书中，对于立即执行函数和其他类似的最佳实践，我会尽量少用。主要原因是，我想让代码对新手友好，毕竟本书是入门用书。这样一来，老手自然懂得何时何法修改代码，新手也能看得懂。本书的所有代码都能正常运行，以后的更新中，我或许会根据读者的反馈来重写代码，使用更多的最佳实践。

假如你想深入研究如何编写高质量Javascript代码，这里推荐几本好书：

* [JavaScript: The Good Parts](http://shop.oreilly.com/product/9780596517748.do): The JavaScript Book.
* [JavaScript Patterns](http://shop.oreilly.com/product/9780596806767.do): Patterns and best practices.
* [JavaScript Enlightenment](http://shop.oreilly.com/product/0636920027713.do): Advanced JavaScript techniques.
* [Maintainable JavaScript](http://shop.oreilly.com/product/0636920027713.do): Writing code that is easy to maintain and work with.

## 反馈和贡献

本书是免费且开放的，我很兴奋收到任何反馈。本书的全部内容都放在[GitHub仓库](https://github.com/soapdog/firefoxos-quick-guide)，并使用Markdown生成(借助Leanpub的某些扩展)。如果你有任何反馈、bug修复和改进，请随时提交合并请求。在此我先对所有的贡献表示感谢。

本书的Git仓库地址是[https://github.com/soapdog/firefoxos-quick-guide](https://github.com/soapdog/firefoxos-quick-guide)。

## 翻译

本书最初由我用葡萄牙语编写，然后翻译成英文。两种语言的版本都可以在网上免费获取：

* [葡萄牙语版本](http://leanpub.com/guiarapidofirefoxos): Guia Rapido para Desenvolvimendo para Firefox OS.
* [英语版本](http://leanpub.com/quickguidefirefoxosdevelopment): Quick Guide for Firefox OS App Development.

欢迎各方人士帮忙把本书翻译成更多语言（顺便帮我改善蹩脚的英语）。

## 版本历史

### 版本 0.3

新增**应用管理器**章节。由于现在大部分设备运行的仍是**Firefox OS 1.1**，所以我会保留旧版模拟器的章节。

此版本对应用管理器章节的更新有些仓促。我们将在下次更新完善它。如果你发现新章节（或任何章节）表述有误，请到[issue tracker on GitHub](https://github.com/soapdog/firefoxos-quick-guide/issues)报告问题。

### 版本 0.2

本书由Mozilla's WebAPI团队的Marcos Caceres修订。每一章内容都进行了技术性的检查，同时修正了许多语法和拼写错误。

### 版本 0.1

这是本书的首个版本。我尚未在编辑器运行过，同时，拼写错误、语法错误还有其他错误都没有修正。英语不是我的母语，如果表述不当请更正我。本书作为一本快速指南，于2013年8月20号开始编写，发布于22号和23号举行的[BrazilJS大会](http://braziljs.com.br/)。因此这更像是一本在两天内快速写成的草稿。

我使用了[Leanpub](http://leanpub.com)系统写这本书。此系统有助于快速迭代，同时让我保持冷静的头脑管理项目。此版本系根据葡萄牙语版，半直译而成。

{mainmatter}