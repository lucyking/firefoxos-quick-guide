# Developer Tools {#developertools}

Firefox拥有很多可以为开发者提供帮助的工具。很多人仍然在用[FireBug](https://addons.mozilla.org/pt-BR/firefox/addon/firebug/)而对Firefox当前已经有内置的开发工具一无所知。本章我们将涉略那些在Firefox OS应用开发中对开发者帮助最大的那些工具。

如果你想了解关于这些工具的更多信息，以及又有哪些新的开发登录Firefox，参见MDN上的[developer tools](https://developer.mozilla.org/en-US/docs/Tools)（你真的应该点击链接去看看，等你看完我再继续讲）。

## Responsive Design View 简介
网页开发的时候经常会在一个workflow中修改某个HTML文件然后在浏览器中重载来查看变化。通常没有使用诸如Grunt或Volo之类的工具的话，就没有必要进行诸如重新编译之类的操作。尽管Firefox OS模拟器允许使用类似的workflow，但是屏幕分辨率是固定的（480x320）。这对需要开发跨平台应用的开发者来说并不理想。

为了能够查看你的应用在不同分辨率设备上的不同效果，你可以使用Firefox中的**Responsive Design View**工具来更改屏幕大小（和方向）。如下图所示，按照这个步骤打开该工具：**Tools menu -> Web Developer -> Responsive Design View**。之后你就可以通过拖拽边角和在选框中选择不同选项来改变屏幕。

![Activating Responsive Design View](images/originals/responsive-design-view.png)

在测试[**media queries**](https://developer.mozilla.org/en-US/docs/Web/Guide/CSS/Media_queries)的时候这个工具尤其有用。因为Responsive Design View让你可以重置屏幕尺寸并看到网页的实时变化。该工具的另一个重要功能是可以保存预定义屏幕尺寸。如果你知道目标屏幕的尺寸，这样你就可以在预定义之后快捷地做出选择而无需调整浏览器的窗口大小。

![Responsive Design View Sample](images/originals/responsive-view-sample.png)

到目前为止，当前市场上的Firefox OS手机的屏幕尺寸都是480x320--ppi为96。但是可以预见分辨率日后会随硬件的升级而发生变化：屏幕更大，ppi更高（就像某些手机的视网膜屏幕）。

为了让你的应用可以适应屏幕的变化，不要在CSS布局中将屏幕分辨率或者ppi值设置成固定值。相反，你可以利用media queries和responsive design methodology来创建能够适应任何屏幕尺寸的应用。和responsive design相关的更多详细内容，我推荐[Responsive Web Design](http://www.abookapart.com/products/responsive-web-design)和[Mobile First](http://www.abookapart.com/products/mobile-first)这两本书。

总的来说，responsive design view使我们可以在不改变Firefox浏览器窗口大小的情况下测试应用在不同分辨率屏幕中的表现。在我看来，这是当前最好用的网页开发工具之一——但是也有很大的局限：这个工具给出的ppi是固定的，所以你无法测试应用在不同像素密度屏幕中（比如在一个视网膜屏幕上）的具体表现。

## 开发者工具
Firefox的开发者工具跟FireBug和当前其他主流浏览器的开发工具类似。利用开发者工具，我们可以用[the console](https://developer.mozilla.org/en-US/docs/Web/API/console)运行和调试JavaScript脚本，并可以操纵当前页面的DOM和CSS布局。

可以通过以下任意一种方式打开the console:

* 通过菜单打开："Tools menu > Web Developer > Web Console"。
* 在需要debug的页面点击右键，选择"Inspect Element"，然后选择"Console"栏。

![JavaScript Console](images/originals/console-open.png)

除*JavaScript Console*外还有诸如[*the style editor*](https://developer.mozilla.org/en-US/docs/Tools/Style_Editor)，[*the network monitor*](https://developer.mozilla.org/en-US/docs/Tools/Network_Monitor)，[*the JavaScript profiler*](https://developer.mozilla.org/en-US/docs/Tools/Profiler)，[*the JavaScript debugger*](https://developer.mozilla.org/en-US/docs/Tools/Debugger)， [*the page inspector*](https://developer.mozilla.org/en-US/docs/Tools/Page_Inspector)之类的很多工具。

我们将运用the console来检查memos应用的进程。这是debug应用时一个强有力的手段——但是还是有很多开发者把在JavaScript代码中到处用`alert()`替代问题代码作为他们的“查错工具”。

采用`alert()`是非常糟糕的，因为如果有任何一处`alert()`在事后没被移除的话，应用的使用者就可能因此而受到影响。采用the console的话就可以避免这个问题，因为the console是人畜无害地（并且神不知鬼不觉地）将所有的测试信息保存到用户通常不会访问的文件中——这样就不会影响用户体验。使用the console的话也意味着你没必要再从代码中移除你的console调试信息。这些信息在出错时可以为代码维护和debug提供必要的帮助（就像他们用其他软件那么做一样）。

学习如何正确使用Firefox（抑或其他任何你在用的浏览器）内置的开发者工具是成为一个优秀开发者的必要历程。因此我建议大家认真浏览上文提到的各种工具的相关链接，以期对Firefox的各类开发工具有一个更加深入的了解。

此外还有一个特殊的工具没有在上文中提到：[*remote debugger*](https://developer.mozilla.org/en-US/docs/Tools/Remote_Debugging)。这个工具可以让我们连接到一个运行Android或Firefos OS系统的手机上，并运用开发者工具在手机上实时调试代码。

## 总结
本章中我们对Firefox内置的开发者工具进行初步的了解。尤其在你把它们和Firefox OS模拟器搭配使用的时候，运用那些工具可以使应用的开发过程不再变得那么艰辛。开发者工具和模拟器是创建一个应用过程中不可或缺的一个搭配。下章我们将学习关于模拟器的更多内容。
