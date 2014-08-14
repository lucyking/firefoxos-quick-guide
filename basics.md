# 基本内容 {#concepts}

在着手建立第一个应用之前，我们先来了解一下关于Firefox OS开发的各类基础。在[说明](#introduction)这一小节我们将了解到，如同网页一样，Firefox OS应用完全基于HTML5。然而，我们不再详述是什么造成两者之间的差别。

我们可以看到当前的App应用通常包含：

* 名称和图标，用户可以通过点击来加载相关应用。
* 应用具备与系统服务和底层硬件交互的能力。

简言之,一个Firefox OS应用就像是一个带图标和名称的网页界面，并且通常可以离线运行（视其实际用途而定）。应用中诸如名称，图标以及其他数据都在一个名为*mainfest*的文件中进行定义，相关内容将在下节详细介绍。

##应用主菜单
[主菜单文件](https://developer.mozilla.org/docs/Apps/Manifest)是一个[JSON](http://json.org)格式的文件，用以描述hosted网页应用的各方面内容。通常该文件被命名为**mainfest.webapp**且位于名为**index.html**的主HTML文件下方。

<<[Mainfest示例](code/sample_manifest.webapp)

在上例中我们可以看到一个名为memos[^memos]的应用。其内容还描述了应用的作者，图标，名称，通过哪个文件来加载该应用（在本例中是通过*index.html*文件加载），你的应用对各类硬件设备的访问权限等。如下图所示，这个mainfest文件会被Firefox OS用来将应用加载到设备的主界面，Firefox应用市场也会从该文件提取分类信息。

[^memos]: 一个简单的Firefox OS应用在[Firefox应用市场](https://marketplace.firefox.com/app/memos)中的示例 以及该应用在[GitHub](https://github.com/soapdog/memos-for-firefoxos)中的源码。

![Memos app shown at the Firefox Marketplace](images/originals/memos-marketplace.png)

注意系统是如何利用mainfest文件中的信息将应用加载到主界面，如以下截图所示。

![Memos on the simulator](images/originals/memos-simulator.png)

将你的HTML，CSS，JavaScript，和一个mainfest文件放到一起，你就获得了一个可以在Firefox OS上运行的应用。让我们继续基础知识的探讨，认识当前应用的类别。

##应用的类别
Firefox OS应用目前可以分为两大类：在线应用和本地应用-在不久的将来可能会有更多的类别（比如：用户自定义键盘和能够创建其他的系统服务）。

* **在线应用：**和通常的网页应用一样运行在网页服务器上。这意味着当用户加载一个在线应用时，该应用的内容将通过从远程服务器上加载（如果有cache可用的话，也可以从中加载）。

* **本地应用：**在安装时复制到设备中的zip格式文件。当用户加载一个本地应用时，该应用的相关内容直接从其zip文件中加载，无需访问服务器。

应用采用这两种格式各有利弊。一方面，在线应用便于管理，开发者只需理保存在服务器上的文件。然而，在线应用很难在离线模式下运行。因为其在离线运行时极大程度地依赖于[**appcache**](https://developer.mozilla.org/pt-BR/docs/HTML/Using_the_application_cache)。在线应用在使用WebAPI方面也会有所受限，导致其不能完成一些离线应用可以完成的任务。

另一方面，离线应用将其所有的内容保存在本地设备中，这样在没有网络的情况下应用也可以正常运行（避免了在线应用会遇到的appcache问题）。离线应用相对在线应用更是具备了调用一些对设备安全较敏感的WebAPI函数的能力。更新离线应用可能会让人比较头疼，开发者需要将每次更新的版本上传到Firefox应用市场，这样就意味着需要花费一些时间来通过审核。

你在选择模式时，要考虑到如果需要调用较高级的WebAPI函数，那就应当采用离线应用模式。然而，要是你的应用可以在无需访问任何高级系统底层服务和硬件的情况下正常运行，那么在线应用模式是更好的选择。如果你没有自己的远程服务器，就选离线模式。

上面我提到过appcache会是一个棘手的问题（有时候在线应用运行时需要appcache）。但是不要太担心，因为我们可以利用很多工具使appcache的生成和开发变得更加简单[^js-tools]。

在本书中为了展示WebAPI的各方面的应用，我们将会建立离线应用。然而，我们学习的大部分mainfest方面的内容将会应用到在线应用中。如果你想更多地了解分布式在线应用方面的内容，参见[the hosted applications link at the developer hub](https://marketplace.firefox.com/developers/docs/hosted)页面。

[^js-tools]:在以下链接中可以找到很多有用的工具，参见[Gulp](https://github.com/gulpjs/gulp)，[Grunt](http://gruntjs.com)， [Volo](http://volojs.org/)， [Yeoman](http://yeoman.io/)， [Bower](http://bower.io/)这些链接。这些工具功能上可能会产生较大的重叠，如何抉择完全取决于开发者的个人喜好。（相对Grunt本人更加喜欢Volo因为它的文件对我来说更加容易阅读）。

既然已经对Firefox OS支持的两类应用有了初步的涉略，现在让我们了解一下这两类应用在获取系统权限方面的差别。

##安全权限级别
Firefox OS中的安全级别分为三级，级别越高，能调用的API函数也越多。

* **基础级（a.k.a. web）：**是所有应用的默认安全级别。这类在线和离线应用不会在其mainfest文件中声明`类型`这一属性。这类应用具有调用浏览器中普通API函数的权限-但是不能调用任何Mozilla's WebAPI。
* **用户级：**这类级别的应用可以调用Firefox浏览器中所有普通的API函数和一些额外的函数，如联系人，系统警告等。只有**离线应用才可以成为用户级应用**，并且该软件安装包需要获得Firefox OS应用市场的电子签名。
* **认证级：**出于安全方面考虑，只有Mozilla和其合作伙伴（如：手机生产商，电信服务商等）可以使用这个安全级别。认证级应用可以调用所有的API函数，如用于拨打电话的函数等。Firefox OS中的拨号应用就是一个认证级的应用。

在开发中，我们可以直接调用用户级API函数而无需取得Mozilla的许可。但是当在发布一个用户级应用之前，需要先将其提交到Firefox应用市场。代码审查也是严格审核的一部分，如果没有问题的话，应用就会获得电子签名-即告诉Firefox OS用户该应用对那些敏感API函数的调用是获得官方许可的。

在MDN上的[WebAPI函数手册页面](https://developer.mozilla.org/en-US/docs/WebAPI)我们可以看到各类平台对应的API函数以及各API函数需要的权限级别。

![Access levels for the APIs](images/originals/webapi-access.png)

如上图所示，所有的应用都可以调用*IndexedDB API和FileHandle API*函数，但是只有用户级应用可以调用*Contacts API 和 Device Storage API*函数。

##Mozilla's WebAPIs
Firefox OS 为开发者提供各类API函数来使开发出的应用具有较高的平台兼容性。现在通过WebAPI已经可以获得设备硬件和系统服务的访问权限。想要了解当前Firefox OS版本可以的API函数有哪些，可以参见Mozilla维基百科中的[WebAPI页面](https://wiki.mozilla.org/WebAPI)。

通过重温一些代码样例我们就可以发现调用这些API函数是多么的简单。不要将本例作为WebAPI函数的官方完全使用手册。我们只是通过示例来讲解通过JavaScript来调用硬件设备的相关功能。

####例 #1：打电话

假设你的应用需要在已经输入号码的情况下调用手机的拨号器。你可以使用以下代码来完成这个功能：

<<[Sending a phone number to the dialer](code/webapi_samples/dial.js)

这段代码通过向拨号应用发送请求来实现对指定号码的呼叫。需要注意的是，该操作并没有播出电话，用户仍需要通过点击应用下方的拨号按钮来完成拨号。在运行程序前需要用户执行明确的操作是一个不错的安全机制：因为在运行之前首先要需要获得用户的许可。更高安全级别的API函数可以在无需用户的交互操作的情况下独立拨打电话。比如最高安全级别的认证级应用就可以实现这项功能。示例代码中使用的是所有安全级别的应用都可以调用的，名为"Web Activities"的API函数。

该函数更多内容参见[more information about Web Activites](https://hacks.mozilla.org/2013/01/introducing-web-activities/)。

### 例 #2：存号码
假设你的公司有一个内网，你想将内网上同事的联系电话存到你的手机里。那么你可以利用联系人API实现这个操作。

<<[存电话](code/webapi_samples/contact.js)

该API函数会新建一个含联系人数据的对象并无需用户的交互操作直接将其保存到手机的号码簿中。由于访问联系人可能涉及到用户隐私，所以该API函数只能被*用户级*应用调用。很多WebAPIh函数都会像示例中一样建立一个具有成功和失败回调方法的对象。

该API函数的更多参见[the page about the *Contacts API* on the Mozilla Wiki](https://wiki.mozilla.org/WebAPI/ContactsAPI).

### 例 #3：拍照片
假设你建立了一个可以为照片添加各种神奇滤镜的应用。然后为了方便你想要在应用中添加一个按钮，用户通过点击该按钮就可以直接拍照或者从相册中选取图片。

<<[拍照片](code/webapi_samples/pick.js)

我们再来看[WebActivity](https://hacks.mozilla.org/2013/01/introducing-web-activities/)的另一个例子。这些WebActivity可以被到各类级别的应用调用。在本例中我们采用离线应用模式并通过检索文件后缀（*MIME Types*）来获取我们想要的文件。代码运行时，系统通过一个界面和用户交互。用户可以从摄像头拍摄，图片文件夹，壁纸文件夹三个选项中任选一个作为图片来源。执行完选择操作后，就会触发成功回调方法。反之如果用户点击取消按钮的话，就会触发失败回调方法。如下图所示，系统给出一个交互界面让用户选择图片来源：

![Example of the *pick activity*](images/originals/pick_image.png)

##总结
在本章中我们了解到，和传统的网页不同，Firefox OS中的在线和离线应用依赖于mainfest文件。我们还了解到， 出于安全方面考虑，离线应用可以再细分为“用户级”和“认证级”。只有这两类安全级别的应用才可以调用Mozilla家那些功能更为强大的，在线应用和传统网页所无法调用的WebAPI函数。

现在就让我们奋袖出臂，着手建立自己的应用！
