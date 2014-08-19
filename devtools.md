# 开发者工具 {#developertools}

Firefox提供了很多工具协助开发者。许多人一直在用[FireBug](https://addons.mozilla.org/pt-BR/firefox/addon/firebug/)，还没发现其实Firefox已经有内置的开发者工具箱了。在本章，我们将介绍几个对Firefox OS应用开发者最有用的工具。

如果你想多了解这些工具，想知道还有哪些好用的开发者工具会登陆Firefox，可以去Mozilla开发者网络（Mozilla's Developer Network）查看[开发者工具](https://developer.mozilla.org/en-US/docs/Tools)页面（说真的，去看看吧！等你的好消息。）

## 响应式设计视图介绍

Web开发过程中有个常见的流程：修改HTML文件，刷新页面，观察变化。除非你的项目已经用到了类似Grunt或Volo这样的自动化构建工具，否则，一般来说，没必要专门将这个流程自动化。在Firefox OS模拟器上也可以执行同样的流程，但模拟器的分辨率目前被限制在了480x320。如果你还想应用能运行在平板、平板电话、大尺寸电视或其他平台，这个分辨率远不能满足要求。

想查看你的应用在任意分辨率下的情况，可以使用Firefox的**响应式设计视图**工具来调整屏幕（以及视区，也译作视口）。如下图所示，依次展开**工具 -> Web开发者 -> 响应式设计视图**便可开启。开启后，窗口马上发生了变化，这时你可以通过拖拽或选择框来改变视区尺寸。

![Activating Responsive Design View](images/originals/responsive-design-view.png)

测试[**media queries**](https://developer.mozilla.org/en-US/docs/Web/Guide/CSS/Media_queries)时，响应式设计视图非常有用。因为它可以让你在调整分辨率后，实时查看网页布局的变化。响应式设计视图还有个很棒的功能：保存预设尺寸。假设你知道目标分辨率，你无需调整浏览器的尺寸就能查看不同分辨率下的页面情况了。

![Responsive Design View Sample](images/originals/responsive-view-sample.png)

在本书编写之际，市场上大多数Firefox OS手机的分辨率都是480x320，约96 PPI（pixels per inch，每英寸所拥有的像素数目）。但是，你应该预想到，将来随着Firefox OS设备硬件的发展，这些都会改变：屏幕分辨率越来越高，PPI也随之越来越高（就如Apple的retina视网膜显示技术）。

为了让应用能够适应未来的发展，不要把CSS值固定在某一分辨率或PPI，而是应该运用media queries和响应式设计思想，开发出适应任何屏幕尺寸的应用。想学习更多响应式设计技巧的话，我推荐两本书：[Responsive Web Design](http://www.abookapart.com/products/responsive-web-design)和[Mobile First](http://www.abookapart.com/products/mobile-first)。

总之，响应式设计视图使得我们无需调整浏览器窗口，就能在各种不同分辨率下测试Web应用。在我看来，这是最有用的Web开发者工具之一。不过它有个很大的局限性：目前还不能用它测试不同的PPI（例如，查看网站在retina或更高PPI屏幕下的显示情况）。

## 开发者工具

Firefox的开发者工具和Firebug以及其他现代浏览器内置的工具相似。依靠这些工具，你可以在[控制台](https://developer.mozilla.org/en-US/docs/Web/API/console)执行和调试Javascript，操作当前页面的DOM和CSS。

要打开控制台，可以：

  * 依次展开“工具 > Web 开发者 > Web 控制台”
  * 在需要调试的页面右击，选择“查看元素”，然后点击“控制台”标签。

![JavaScript Console](images/originals/console-open.png) 

除了*控制台*，还有很多其他工具供开发者使用，包括但不限于[*样式编辑器*](https://developer.mozilla.org/en-US/docs/Tools/Style_Editor), [*网络*](https://developer.mozilla.org/en-US/docs/Tools/Network_Monitor), [*分析器*](https://developer.mozilla.org/en-US/docs/Tools/Profiler), [*调试器*](https://developer.mozilla.org/en-US/docs/Tools/Debugger), [*查看器*](https://developer.mozilla.org/en-US/docs/Tools/Page_Inspector)。

在上一章我们开发应用时，用到了console来查看应用的进展。这是一种非常强大的调试方法，然而部分开发者还在用“alert()”来调试他们的Javascript代码。
   
使用“alert()”真的不是个好主意，假如开发者忘记把它们移除，最终遭罪的还是用户。而console就能避免这个问题，它会无痛无害（无声无息）地把所有调试信息输出到一个用户平常不会打开的地方，用户体验得以保障。使用console还有个好处，你不必删除代码里的调试信息了，当然删不删都随你。当程序出错的时候（这种事再常见不过了），这对代码维护和调试很有用。

要成为一名称职的开发者，学会正确使用Firefox（或其他浏览器）内置的开发者工具是必不可缺的一步。这就是为何我建议每个人都去看看前面提到的链接，熟悉Firefox现有的各种工具。

有一个特殊工具我上面没提到，[*远程调试*](https://developer.mozilla.org/en-US/docs/Tools/Remote_Debugging)。它允许我们连接Android或Firefox OS手机，然后使用开发者工具实时调试手机的应用。

## 总结

本章我们简单介绍了Firefox内置的开发者工具。这些工具能简化你的开发过程，尤其是结合Firefox OS模拟器使用。它们是开发应用密不可分的一体。下一章，我们将进一步了解模拟器，学习如何充分利用它。