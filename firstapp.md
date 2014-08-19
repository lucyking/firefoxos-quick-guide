# 我们的第一个App {#firstapp}

![Memos, a minimalist notepad app](images/originals/memos-app.png)

在本章中，我们将着手建立一个名为**Menos**的简易记事本应用。在开工之前，让我们先来回顾下该应用的运行方式。

该应用具有三个界面。主界面列出记事条目。当你点击一条记事（或者添加一条记事）的时候，就会跳转到可以对相关标题和内容进行编辑的详细界面。如下图所示：

![Memos, editing screen](images/originals/memos-editing-screen.png)

在上示界面中，我们可以通过点击垃圾桶图标来删除这项记事。点击之后会弹出一个请求确认的对话框。

![Memos, note removal confirmation screen](images/originals/memos-delete-screen.png)

该记事本的源码在这里：[the Memos Github Repo](https://github.com/soapdog/memos-for-firefoxos)（或zip格式：[the Memos.zip](https://github.com/soapdog/memos-for-firefoxos/archive/master.zip)）。推荐大家下载简单快捷的zip压缩文件。在本书的**code**文件夹里也有备份：[github repository for this book](https://github.com/soapdog/firefoxos-quick-guide)

Memos采用[IndexedDB](https://developer.mozilla.org/en-US/docs/IndexedDB/Using_IndexedDB)作为数据库来保存条目；采用[Gaia Building Blocks](http://buildingfirefoxos.com/building-blocks)来建立交互界面。再版时我会和大家更加深入细致地探讨Gaia Building Blocks，现在我们只管去用它。读者可以通过上述链接了解交互界面开发工具等各类详细内容。

首先我们为要开发的应用建立一个名为 **memos**的文件夹。

##创建应用的mainfest文件
Memos的mainfest文相当简洁。先在**memos**文件下创建一个名为**manifest.webapp** 的文件。mainfest是一种用来描述应用属性的[JSON](http://json.org)格式文件。该文件通常包含应用名称，应用图标源地址，从哪个文件开始加载应用，该应用会调用哪些用户级API函数等各项信息。

以下我们可以看到Memos应用中mainfest文件的详细内容。在复制这些数据的时候要注意避免在文本中添加额外的逗号导致JSON格式出错。当前有很多帮助开发者合法化JSON文件格式的工具，不过这里推荐一款专门用来生成mainfest文件的在线工具。详情参见[http://appmanifest.org/](http://appmanifest.org/)（译注：这个链接有问题）。关于mainfest的更多内容参见[this page on MDN about them](https://developer.mozilla.org/docs/Apps/Manifest)。

<<[Memos manifest file (*manifest.webapp*)](code/memos/manifest.webapp)

我们来看下以上mainfest文件中包含了哪些字段。

|Field		|Description                                                                        |
|-----------|-----------------------------------------------------------------------------------|
|name		|App的名字.		                                    |
|version	|App的当前版本. 										   |
|launch_path|你的App的启动文件.					                 |
|permissions|你的App请求的 API 权限. 下面是详细信息.				     |
|developer  |谁开发了这个App 									      |
|icons		|这个App使用的图标的不同尺寸. 							  |

这些字段中比较重要的是授权许可字段，通过在该字段中声明对*存储设备*的读写权限，应用才能运用IndexedDB无限制地存储数据。[^存储限制]（由于权限许可应用软件才可以随心所欲地存储-不过要注意不要过多占用设备的存储空间）。

[^存储权限]：相关权限的更多内容参见[the page on MDN about app permissions](https://developer.mozilla.org/en-US/docs/Web/Apps/App_permissions)。

既然已经建好mainfest文件，接下来我们开始着手创建HTML文件。

##创建HTML文件
创建HTML文件之前，我们简略地探讨一些关于[Gaia Building Blocks](http://buildingfirefoxos.com/building-blocks)的内容。Gaia Building Blocks中包含了很多可以重用到开发者自己的应用上，Firefox OS风格的交互界面代码模板。

就像网页上那样，开发者并没有被强制要求采用以上Firefox OS风格的交互界面模块。是否采用Gaia模块完全取决于开发者自身抉择。并且，一个好的应用本来就应当具有别具一格的自身特点和用户体验。还要说明一点，开发者的应用并不会因为没有采用Gaia模板而遭受偏见或惩罚。在本书中采用Gaia界面模板是因为本人UI设计不行（也没钱另聘平面设计师）。

Gaia模块中的HTML布局将应用的每一屏幕界面定义为一个`<section>`，其他的元素也遵循一定的默认布局格式。可以从位于github上的[memos](https://github.com/soapdog/memos-for-firefoxos)仓库下载Memos应用源文件（包含Building Blocks）。要是对git和github不熟悉的话，可以下载压缩包：[memos.zip](https://github.com/soapdog/memos-for-firefoxos/archive/master.zip)。

注意：我使用的这一Gaia模块并不是Mozilla发布的最新版本。更新到最新版本会导致Memos应用崩溃。但是在建立你自己的应用时还是应该尽量使用最新版本的Gaia模块。

###将Gaia模块包含进来

先将已下载的源码中的 **shared** 和 **style** 文件夹复制到之前新建的 **memos** 文件夹里。这样我们就可以在开发应用时使用Gaia模块了。

然后在 **index.html** 文件中声明要调用的各个css文件。

~~~~~~~~
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <link rel="stylesheet" type="text/css" href="style/base.css" />
    <link rel="stylesheet" type="text/css" href="style/ui.css" />
    <link rel="stylesheet" type="text/css" href="style/building_blocks.css" />
    <link rel="stylesheet" type="text/css"
          href="shared/style/headers.css" />
    <link rel="stylesheet" type="text/css"
          href="shared/style_unstable/lists.css" />
    <link rel="stylesheet" type="text/css"
          href="shared/style_unstable/toolbars.css" />
    <link rel="stylesheet" type="text/css"
          href="shared/style/input_areas.css" />
    <link rel="stylesheet" type="text/css"
          href="shared/style/confirm.css" />
    <title>Memos</title>
</head>
~~~~~~~~

在第1行，我们将文件的DOCTYPE声明为HTML5。第5~15行中，我们对在应用中用作标题，列表，输入框等各种布局的css文件进行调用声明。

###创建主界面

现在我们可以开始着手创建各类界面了。像前文提到的那样，应用所用的每个界面被定义为包含在HTML `<body>` 主体中的一个 `<section>` 部件。在body标签之后需要将*role* 的属性设置成 *application* ，以便CSS选择器据此创建相应界面。所以我们这样定义body标签：`<body role="application">`。现在让创建第一个屏幕界面并定义相关的body标签。

~~~~~~~~
<body role="application">

<section role="region" id="memo-list">
    <header>
        <menu type="toolbar">
            <a id="new-memo" href="#"><span class="icon icon-add">add</span></a>
        </menu>
        <h1>Memos</h1>
    </header>
    <article id="memoList" data-type="list"></article>
</section>
~~~~~~~~

以上屏幕界面代码中的 `<header>` 部件包含了该应用的名称和一个用来添加新记事及其的按钮。这个屏幕也包含一个`<article>`部件用来容纳记事列表。在后面的JavaScript应用环节中我们将利用按钮和记事ID来捕获事件（capture events）。

记住，每个屏幕界面都是直接由一大段HTML代码定义布局。用其他编程语言构建同样外观的界面的过程通常更加繁杂。在HTML中，我们只需声明各类容器，并赋以对应的ID值以便引用。

主屏幕已建好，接下来我们开始构建编辑界面。

### 构建编辑界面

因为在用户尝试删除一条记事时需要弹出一个确认删除的对话框，所以编辑界面的代码更加复杂。

~~~~~~~~
<section role="region" id="memo-detail" class="skin-dark hidden">
  <header>
    <button id="back-to-list"><span class="icon icon-back">back</span>
    </button>
    <menu type="toolbar">
      <a id="share-memo" href="#"><span class="icon icon-share">edit</span>
            </a>
    </menu>
    <form action="#">
      <input id="memo-title" placeholder="Memo Title" required="required" 
      type="text">
      <button type="reset">Remove text</button>
    </form>
  </header>
  <p id="memo-area">
    <textarea placeholder="Memo content" id="memo-content"></textarea>
  </p>
  <div role="toolbar">
    <ul>
      <li>
        <button id="delete-memo" class="icon-delete">Delete</button>
      </li>
    </ul>
  </div>
  <form id="delete-memo-dialog" role="dialog" data-type="confirm" 
  class="hidden">
    <section>
      <h1>Confirmation</h1>
      <p>Are you sure you want to delete this memo?</p>
    </section>
    <menu>
      <button id="cancel-delete-action">Cancel</button>
      <button id="confirm-delete-action" class="danger">Delete</button>
    </menu>
  </form>
</section>
~~~~~~~~

在编辑界面的顶端，即代码中的`<header>`部件中包含了：   
* 一个可以返回主界面的按钮，   
* 一个用来包含记事标题的文本框   
* 一个可以通过邮件分享记事的按钮   

在顶部菜单栏下方，我们用一个`<textarea>`部件来容纳记事主内容，用一个带垃圾桶图标的菜单栏来删除当前记事。

编辑界面由以上三个部件以及它们的子节点一起组成。此外我们采用一个`<form>`部件作为用户试图删除记事时的交互部件。这个简单的提示框仅包含提示文本和两个按钮：一个确认，一个删除。

完成这个`<section>`后，我们已经实现了所有的屏幕界面。剩下的任务就是声明JavaScript文件的引用位置，结尾添加</html>呼应前文。

~~~~~~~~
<script src="/js/model.js"></script>
<script src="/js/app.js"></script>
</body>
</html>
~~~~~~~~

## 编写Javascript脚本
现在我们开始着手编写可以使应用变得更加生动的Javascript脚本。我们一共创建了两个脚本文件，以便管理和组织代码。

* **model.js：**包含存储路径和记事检索路径，但是不含任何应用运行逻辑，用户界面，数据项等信息。理论上，我们可以在其他需要记事功能的应用中重用这个文件。

* **app.js：** 将HTML元素和其对应的事件句柄联系起来，并且包含应用的运行逻辑。

以上两个文件都应该放在一个名为**js**的文件夹中，该文件夹位于**style**和**shared**文件夹之后。

### model.js
我们将采用[IndexedDB](https://developer.mozilla.org/en-US/docs/IndexedDB/Using_IndexedDB)存储记事条目。由于事先在应用的mainfest文件中对存储权限进行声明，所以存储条目不会受到限制。但是，我们不能因此而滥用权限。Firefox OS设备的存储空间通常有限。所以开发者需要时刻注意你的应用存储了什么数据（如果你的应用占用了设备过多存储空间的话，用户就会卸载应用并给你的应用差评！）。并且存储过量数据的话会对应用的流畅运行产生影响，使应用在运行时有卡顿的感觉。注意，当你向Firefox OS应用市场提交应用，审查者会问你为什么要声明无限大的存储空间 -- 如果你不能做出合理解释的话，你的应用将不能通过审核。

以下的这段js代码主要用来打开链接和创建存储空间。

注意：这部分代码在编写方面以通俗易懂为主，没有体现出JS脚本在性能方面的特点。一些全局变量在各种代码段中都有使用（我要开始抱怨了啊）。但是这些。本书的主要目的是为了教授开发Firefox OS应用的*主要流程*，而不是JS脚本的最佳结构。此外，我会根据读者的后续反馈，在不影响初学者理解的前提下优化代码。

~~~~~~~
var dbName = "memos";
var dbVersion = 1;

var db;
var request = indexedDB.open(dbName, dbVersion);

request.onerror = function (event) {
    console.error("Can't open indexedDB!!!", event);
};
request.onsuccess = function (event) {
    console.log("Database opened ok");
    db = event.target.result;
};

request.onupgradeneeded = function (event) {

    console.log("Running onUpgradeNeeded");

    db = event.target.result;

    if (!db.objectStoreNames.contains("memos")) {

        console.log("Creating objectStore for memos");

        var objectStore = db.createObjectStore("memos", {
            keyPath: "id",
            autoIncrement: true
        });
        objectStore.createIndex("title", "title", {
            unique: false
        });

        console.log("Adding sample memo");
        var sampleMemo1 = new Memo();
        sampleMemo1.title = "Welcome Memo";
        sampleMemo1.content = "This is a note taking app. Use the plus sign " +
	                      "in the topleft corner of the main screen to " +
			      "add a new memo. Click a memo to edit it. All " +
			      "your changes are automatically saved.";

        objectStore.add(sampleMemo1);
    }
}
~~~~~~~

注：源码中的全局变量只是作为教学之用。原谅我在书本中为了节约排版空间而删除了全局变量的相关注释。Github上的源码中任然包含所有注释。以上代码创建了一个*db*对象和一个*request*对象。*db*对象会被源码中的其他方法调用以管理记事的存储。

在调用`request.onupgradeneeded`方法的同时应用会创建一个新的类似hello worid的记事。该方法会在第一次运行应用（或数据库更新版本）的时候调用。即当应用被首次加载时，数据会初始化并保存一条包含“欢迎使用”内容的记事。

在打开链接和初始化数据库之后，就可以开始编写管理记事内容的相关方法了。

~~~~~~~~
function Memo() {
    this.title = "Untitled Memo";
    this.content = "";
    this.created = Date.now();
    this.modified = Date.now();
}

function listAllMemoTitles(inCallback) {
    var objectStore = db.transaction("memos").objectStore("memos");
    console.log("Listing memos...");

    objectStore.openCursor().onsuccess = function (event) {
        var cursor = event.target.result;
        if (cursor) {
            console.log("Found memo #" + cursor.value.id +
	                         " - " + cursor.value.title);
            inCallback(null, cursor.value);
            cursor.continue();
        }
    };
}

function saveMemo(inMemo, inCallback) {
    var transaction = db.transaction(["memos"], "readwrite");
    console.log("Saving memo");

    transaction.oncomplete = function (event) {
        console.log("All done");
    };

    transaction.onerror = function (event) {
        console.error("Error saving memo:", event);
        inCallback({
            error: event
        }, null);

    };

    var objectStore = transaction.objectStore("memos");

    inMemo.modified = Date.now();

    var request = objectStore.put(inMemo);
    request.onsuccess = function (event) {
        console.log("Memo saved with id: " + request.result);
        inCallback(null, request.result);

    };
}

function deleteMemo(inId, inCallback) {
    console.log("Deleting memo...");
    var request = db.transaction(["memos"],
                  "readwrite").objectStore("memos").delete(inId);

    request.onsuccess = function (event) {
        console.log("Memo deleted!");
        inCallback();
    };
}
~~~~~~~~

在以上代码中我们创建了一个构造器，在必须的字段已经初始化的情况下就可以创建新的应用实例。之后我们调用相关方法来列出记事列表，存储和删除记事条目。其中的部分函数接受一个称为`inCallback`的回调参数，这个参数会在方法运行完成后返回。之所以采用这种机制主要由IndexedDB的异步同步机制决定。所有的回调函数具备类似`callback(error, value)`的结构。有些方法的回调参数value值为零。

注：由于本书面向初学者，我不主张调用涉及[*Promises*](https://developer.mozilla.org/en-US/docs/Mozilla/JavaScript_code_modules/Promise.jsm/Promise)的API函数。推荐使用那些容易实现相同效果且易于阅读的方法。

既然条目存储和方法调用已经完备，接下来就可以将**app.js**中的运行逻辑应用到app中。

### app.js
该文件包含应用的逻辑。在本书中由于源码冗杂限于篇幅以致不可能全部照搬。我会将其分为几个部分然后分别讲解。

~~~~~~~~
var listView, detailView, currentMemo, deleteMemoDialog;

function showMemoDetail(inMemo) {
    currentMemo = inMemo;
    displayMemo();
    listView.classList.add("hidden");
    detailView.classList.remove("hidden");
}


function displayMemo() {
    document.getElementById("memo-title").value = currentMemo.title;
    document.getElementById("memo-content").value = currentMemo.content;
}

function shareMemo() {
    var shareActivity = new MozActivity({
        name: "new",
        data: {
            type: "mail",
            body: currentMemo.content,
            url: "mailto:?body=" + encodeURIComponent(currentMemo.content) +
	                "&subject=" + encodeURIComponent(currentMemo.title)

        }
    });
    shareActivity.onerror = function (e) {
        console.log("can't share memo", e);
    };
}

function textChanged(e) {
    currentMemo.title = document.getElementById("memo-title").value;
    currentMemo.content = document.getElementById("memo-content").value;
    saveMemo(currentMemo, function (err, succ) {
        console.log("save memo callback ", err, succ);
        if (!err) {
            currentMemo.id = succ;
        }
    });
}

function newMemo() {
    var theMemo = new Memo();
    showMemoDetail(theMemo);
}
~~~~~~~~

在程序开头，我们先声明几个全局变量（哇!!!）来存储后面会用到的指向DOM元素的指针（reference）。其中`currentMemo`全局变量存储用户当前阅读的记事对象。

`showMemoDetail()` 和 `displayMemo()`两个方法一起协同工作。前者将选中的记事加载到后一个方法中，并控制元素的CSS布局生成一个可编辑的记事界面。后者加载记事内容并将其显示在屏幕上。我们可以用一个方法同时实现这两个功能，但是分开的话在调用时更易于experiment。

`shareMemo()`方法调用调用[WebActivity](https://hacks.mozilla.org/2013/01/introducing-web-activities/)函数来打另一个邮件应用，并将当前的记事内容预先填写到邮件正文中。

`textChanged()`方法会将所有字段中的数据保存到`currentMemo`对象中然后保存当前记事。这样的话当记事中的内容发生变化的时候，该方法就会自动将变更的内容同步到IndexedDB数据库中。

`newMemo()`方法会创建一个新记事并打开一个相关的编辑界面。

~~~~~~~~
function requestDeleteConfirmation() {
    deleteMemoDialog.classList.remove("hidden");
}

function closeDeleteMemoDialog() {
    deleteMemoDialog.classList.add("hidden");
}

function deleteCurrentMemo() {
    closeDeleteMemoDialog();
    deleteMemo(currentMemo.id, function (err, succ) {
        console.log("callback from delete", err, succ);
        if (!err) {
            showMemoList();
        }
    });
}

function showMemoList() {
    currentMemo = null;
    refreshMemoList();
    listView.classList.remove("hidden");
    detailView.classList.add("hidden");
}
~~~~~~~~

`requestDeleteConfirmation()方法负责弹出确认删除对话框。

`closeDeleteMemoDialog()` 和 `deleteCurrentMemo()`方法会在用户确认删除时触发。

`showMemoList()`方法在显示记事列表前将先执行一次清除。比如，会将`currentMemo`方法中的内容清除掉，因为在显示记事列表时我们还没有读取任何具体记事条目。

~~~~~~~~
function refreshMemoList() {
    if (!db) {
        // HACK:
        // this condition may happen upon first time use when the
        // indexDB storage is under creation and refreshMemoList()
        // is called. Simply waiting for a bit longer before trying again
        // will make it work.
        console.warn("Database is not ready yet");
        setTimeout(refreshMemoList, 1000);
        return;
    }
    console.log("Refreshing memo list");

    var memoListContainer = document.getElementById("memoList");


    while (memoListContainer.hasChildNodes()) {
        memoListContainer.removeChild(memoListContainer.lastChild);
    }

    var memoList = document.createElement("ul");
    memoListContainer.appendChild(memoList);

    listAllMemoTitles(function (err, value) {
        var memoItem = document.createElement("li");
        var memoP = document.createElement("p");
        var memoTitle = document.createTextNode(value.title);

        memoItem.addEventListener("click", function (e) {
            console.log("clicked memo #" + value.id);
            showMemoDetail(value);

        });

        memoP.appendChild(memoTitle);
        memoItem.appendChild(memoP);
        memoList.appendChild(memoItem);


    });
}
~~~~~~~~

`refreshMemoList()`方法通过逐条建立记事列表来调整DOM。你可以使用诸如[handlebars](http://handlebarsjs.com/) 或者 [underscore](http://underscorejs.org/)之类的模板来简化编写过程。但是本文中的应用仅仅使用*vanilla javascript*手工编写完成。 该功能通过上文中的`showMemoList()`方法调用。

以上就是应用中用到的所有函数方法。现在唯独缺少事件句柄的初始化和`refreshMemoList()`方法的初始调用代码。

~~~~~~~
window.onload = function () {
    // elements that we're going to reuse in the code
    listView = document.getElementById("memo-list");
    detailView = document.getElementById("memo-detail");
    deleteMemoDialog = document.getElementById("delete-memo-dialog");

    // All the listeners for the interface buttons and for the input changes
    document.getElementById("back-to-list")
            .addEventListener("click", showMemoList);
    document.getElementById("new-memo")
            .addEventListener("click", newMemo);
    document.getElementById("share-memo")
            .addEventListener("click", shareMemo);
    document.getElementById("delete-memo")
            .addEventListener("click", requestDeleteConfirmation);
    document.getElementById("confirm-delete-action")
            .addEventListener("click", deleteCurrentMemo);
    document.getElementById("cancel-delete-action")
            .addEventListener("click", closeDeleteMemoDialog);
    document.getElementById("memo-content")
            .addEventListener("input", textChanged);
    document.getElementById("memo-title")
            .addEventListener("input", textChanged);

    // the entry point for the app is the following command
    refreshMemoList();

};
~~~~~~~

所有代码准备好后，我们就可以开始在模拟器中测试应用。我们可以分别通过**App Manager**或者较旧的**Firefox OS 1.1 simulator**两种方法进行测试。接下来分别详细讲解这两种技术。

如果你的火狐浏览器版本在29及以上，那么你可以安装 App Manager插件。其他较低版本浏览器可以安装旧版本的模拟器。需要注意的是，App Manager只能连接Firefox OS 1.2以上版本的设备。

如果你有一个Firefox OS 1.1版本的设备，然后你的浏览版本是29，那么你可以根据需要安装对应平台的Firefox OS 1.1 simulator version 5.0 [Mac OS X](http://ftp.mozilla.org/pub/mozilla.org/labs/r2d2b2g/r2d2b2g-5.0pre7-mac.xpi), [Linux](http://ftp.mozilla.org/pub/mozilla.org/labs/r2d2b2g/r2d2b2g-5.0pre7-linux.xpi) or [Windows](http://ftp.mozilla.org/pub/mozilla.org/labs/r2d2b2g/r2d2b2g-5.0pre7-windows.xpi)。安装完成后你就可以按照指导说明连接上你的设备。

如果你的设备系统是Firefox OS 1.1版本并且是无锁的，那么你还可以选择更新到1.2或更高的系统版本。选择更新会使你之后的工作变得更加顺手。 Geeksphone Keon，Geeksphone Peak和Geeksphone Revolution上面每天都会推出Firefox OS的改进版本，参见[http://downloads.geeksphone.com/](http://downloads.geeksphone.com/)。 我国的ZTE Open详细更新步骤参见[Upgrading your ZTE Open to Firefox 1.1 or 1.2 (fastboot enabled)](https://hacks.mozilla.org/2014/01/upgrading-your-zte-open-to-firefox-1-1-or-1-2-fastboot-enabled/)。LG Fireweb暂时不能更新。如果你们觉得我人不错，然后又觉得LG不能更新很讨厌的话，那么快去他们的官方Twitter上提建议。Alcatel One Touch Fire可以更新，但是其详细步骤已经超出了本书的讨论范畴。

注：bugzilla上的这个[bug request #1001590](https://bugzilla.mozilla.org/show_bug.cgi?id=1001590)补丁可以修正当前Firefox 29不能运行Firefox OS 1.1模拟器的问题。

## 用App Manager测试应用
在测试之前，我们最好事先确认下所有的文件都在正确的位置上。你的memos应用的文件布局应当如下图所示：

![List of files used by Memos](images/originals/memos-file-list.png)

如果出现不同的文件层次的话，那么你肯定在某些地方写错了。出错的话请将你的源码和这份源码进行比对[the memos github repository](https://github.com/soapdog/memos-for-firefoxos) 。本书附录[book repository](https://github.com/soapdog/guia-rapido-firefox-os)中的**code**文件夹内也包含了一份相同的源码。

打开*Simulator Dashboard*，依次选择**Tools -> Web Developer -> App Manager**

![Where you can find the App Manager](images/originals/locate-app-manager.png)

打开App Manager后，点击**Apps tab**中的**Add Packaged App**选项，然后浏览并选中你的应用所在的文件夹目录。

![Adding a new app](images/originals/app-manager-add-packaged-app.png)

一切顺利的话接下来你就可以在应用列表中看到你的应用。

![Memos showing on the App Manager](images/originals/app-manager-showing-memos.png)

添加完应用之后，点击**Start Simulator**按钮并启动一个已安装好的设备模拟器。如果你还没有安装任何模拟器的话，建议你按照屏幕上的说明步骤把它们全部安装上去。

当模拟器开始运行时，在**App Manager**的memos应用详细界面中点击**Update**按钮来将memos应用安装到模拟器中。安装完成后memos应用的图标就会出现在模拟器的主屏幕上。可以通过点击图标来运行该应用。


![Memos installed on the Simulator](images/originals/app-manager-updating-memos.png)

干得漂亮！现在你已经创建并测试完成了你的第一个应用。一个没有新意的简单应用--但是我希望通这个示例让你对Firefox OS应用的创建流程有更加深刻的了解。如你所见，这和基本的Web开发没有很大的区别。

注意，更新应用源文件中任何内容之后，都需要通过点击**Update**按钮来更新模拟器中存储的对应文件。

## 在模拟器上测试应用
在测试之前，我们最好事先确认下所有的文件都在正确的位置上。你的memos应用的文件布局应当如下图所示：

![List of files used by Memos](images/originals/memos-file-list.png)

如果出现不同的文件层次结构缩进的话那么你肯定在某些地方写错了。出错的话请将你的源码和这份源码进行比对[the memos github repository](https://github.com/soapdog/memos-for-firefoxos) 。本书附录[book repository](https://github.com/soapdog/guia-rapido-firefox-os)中的**code**文件夹内也包含了一份相同的源码。

打开*Simulator Dashboard*，依次选择**Tools -> Web Developer -> Firefox OS Simulator**。

![How to open simulator dashboard](images/originals/tools-web-developer-simulator.png)

点击**Add Directory**按钮，然后浏览并选中你的应用所在的文件夹目录。

![Adding a new app](images/originals/simulator-add-directory.png)

一切顺利的话接下来你就可以在应用列表中看到你的应用

![Memos showing on the dashboard](images/originals/memos-on-dashboard-display.png)

当你添加一个新的应用时，模拟器会加载并运行你的新应用。接下来你就可以测试应用的各项功能。

干得漂亮！现在你已经创建并测试完成了你的第一个应用。一个没有新意的简单应用--但是我希望通这个示例让你对Firefox OS应用的创建流程有更加深刻的了解。如你所见，这和基本的Web开发没有很大的区别。

注意，更新应用源文件中任何内容之后，都需要通过点击**Refresh**按钮来更新模拟器中存储的对应文件。

## 总结
在本章中我们建立了第一个自己的Firefox OS应用并在模拟器上测试运行。下一章我们将涉略开发者工具，那些套件将在开发中助你一臂之力。
