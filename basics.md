##基本内容
在着手建立第一个应用之前，我们先来了解一下关于Firefox OS开发的各类基础。在[说明](#说明)这一小节我们将了解到，如同网页一样，Firefox OS应用完全基于HTML5。然而，我们不再详述是什么造成两者之间的差别。

我们可以看到当前的App应用通常包含：

* 名称和图标，用户可以通过点击来加载相关应用。
* 应用具备与系统服务和底层硬件交互的能力。

look at the big pic,一个Firefox OS就像是一个带图标和名称的网页界面，并且通常可以离线运行（视其实际用途而定）。应用中诸如名称，图标以及其他数据都在一个名为*应用主菜单文件夹*中定义，相关内容将在下节详细介绍。

##应用主菜单
[主菜单文件](https://developer.mozilla.org/docs/Apps/Manifest)是一个[JSON](http://json.org)格式的文件用以描述hosted网页应用的各方面内容。通常该文件被命名为**mainfest.webapp**且位于名为**index.html**的主HTML文件下方。

<<[Mainfest示例](code/sample_manifest.webapp)

在上例中我们可以看到一个名为memos[^memos]的应用。其内容还描述了应用的作者，图标，名称，通过哪个文件来加载该应用（在本例中是通过*index.html*文件加载），你的应用对各类硬件设备的访问权限等。如下图所示，这个mainfest文件会被Firefox OS用来将应用加载到设备的主界面，Firefox应用市场也会从该文件提取分类信息。

[^memos]: 一个简单的Firefox OS应用示例在[Firefox应用市场中的示例](https://marketplace.firefox.com/app/memos) 以及该应用在[GitHub中的源码](https://github.com/soapdog/memos-for-firefoxos)。

![Memos app shown at the Firefox Marketplace](images/originals/memos-marketplace.png)

注意系统是如何利用mainfest文件中的信息将应用加载到主界面，如以下截图所示。

![Memos on the simulator](images/originals/memos-simulator.png)

将你的HTML，CSS，JavaScript，和一个mainfest文件放到一起，你就获得了一个可以在Firefox OS上运行的应用。让我们继续基础知识的探讨，认识当前应用的类别。

##应用的类别
Firefox OS应用目前可以分为两大类：远程应用和本地应用-在不久的将来可能会有更多的类别（比如：用户自定义键盘和能够创建其他的系统服务）。

* **远程应用：**和通常的网页应用一样运行在网页服务器上。这意味着当用户加载一个远程应用时，该应用的内容将通过从远程服务器上加载（如果有cache可用的话，也可以从中加载）。

* **本地应用：**在安装时复制到设备中的zip格式文件。当用户加载一个本地应用时，该应用的相关内容直接从其zip文件中加载，无需访问服务器。

There are pros and cons to both types. On the one hand, hosted apps are easier to maintain, as all you need to do is maintain files on your web server. However, it's harder to make them work offline because it requires the use of the much despised [**appcache**](https://developer.mozilla.org/pt-BR/docs/HTML/Using_the_application_cache). Hosted apps are also limited in which WebAPIs they can use, which means they can't do all the things a packaged app can do.   

On the other hand, packaged apps have all their content stored on the device - which means they are always available when the user is offline (and so avoid needing appcache). They also have the ability to access security-sensitive WebAPIs that are not available to hosted apps. Updating them can be a bit painful, because you need to upload any new version to the Firefox Marketplace - which means going through a review process, which can take some time.   

When trying to choose which type of application to build, consider: if you require advanced WebAPIs, then you should use a packaged app. However, if your application works fine without needing to access any advanced system services or device capabilities beyond those already available in a web browser, then always choose a hosted app. It is ok to use packaged apps if you don't have a place to host it.

Above I mentioned that appcache can be problematic (which is sometimes required for hosted apps). Don't worry too much as there are tools available to make appcache generation and deployment easier [^js-tools].

In this book we're going to build packaged apps, as it will allow us to explore what is possible with the WebAPIs. However, most of what we will learn about manifests applies to hosted apps. If you want to know more about distributing hosted apps, check [the hosted applications link at the developer hub](https://marketplace.firefox.com/developers/docs/hosted).

[^js-tools]: There are many useful tools out there, check out [Gulp](https://github.com/gulpjs/gulp), [Grunt](http://gruntjs.com), [Volo](http://volojs.org/), [Yeoman](http://yeoman.io/), [Bower](http://bower.io/). There is a lot of overlap among these tools, its a matter of preference which one you use. (I like Volo more than Grunt mostly because Volofiles are easier for me to read).

Now that we've covered the two types of applications that Firefox OS supports, let's look at the different levels of system access they can have.

## Security Access Levels

There are three security levels on Firefox OS - with each level having more access to APIs than the previous level.

* **Plain (a.k.a. web):** This is the default level for all applications. This level applies to hosted apps and packaged apps that do not declare a `type` property in their manifest file. These apps have access to the common set of APIs found in browsers - but don't have access to any of Mozilla's WebAPIs.
* **Privileged:** This type of app has access to all common APIs found in the Firefox browser, plus some additional ones, such as contacts, and system alarms. Only **packaged apps can be privileged apps** and the package must be digitally signed by the Firefox OS Marketplace.
* **Certified:** For security reasons, this level is only available to Mozilla and its partners (e.g. phone manufacturers, telecoms, etc.). Certified apps are able to access all the APIs, such as telephony and more. An example of certified app is the Firefox OS dialer application. 

During development, it is possible for us to access privileged APIs without needing any special permission from Mozilla. But when we want to distribute a privileged app, it first needs to go to the Firefox Marketplace. There, the code is checked as part of a rigorous approval process, and if it's found to be OK, it will be digitally signed - which tells users of Firefox OS that this application is allowed to access sensitive APIs.

On [the page about the WebAPIs on the Mozilla Developer Network](https://developer.mozilla.org/en-US/docs/WebAPI) we can see what APIs are implemented on what platforms and what access level is needed to use each API.

![Access levels for the APIs](images/originals/webapi-access.png)

As we can see on the image above, any application can access the *IndexedDB API and FileHandle API* but only privileged apps can access the *Contacts API and Device Storage API*.

## Mozilla's WebAPIs

Firefox OS provides us with the APIs that enable us to build applications that are just as capable as native apps on other platforms. Access to hardware and services is done through the WebAPIs. To learn more about the list of available APIs for the current Firefox OS version check out [the WebAPI page on the Mozilla Wiki](https://wiki.mozilla.org/WebAPI).

Lets review some code examples to see how easy those APIs are to use. Don't take this example as a full documentation of the WebAPIs, they are just a small sample to make you understand how we can access device features using JavaScript.

### Example #1: Making calls

Imagine that you have an application that needs to open the dialer with a phone number already filled in. You can just use the following code:

<<[Sending a phone number to the dialer](code/webapi_samples/dial.js)

This code makes a request to the dialer app to call a particular number. Note that this doesn't actually place a call - the user will still need to tap the dial button to place the call. Requiring explicit user action before executing some other action is pretty common: it's a good security pattern because it requires user interaction through consent before allowing something to happen. Other APIs that can place calls without user interaction are available for more elevated access levels. Certified apps can place calls without interaction for example. The API used in the code above, called "Web Activities", is available to all apps though.

Check out the the Mozilla Blog for [more information about Web Activites](https://hacks.mozilla.org/2013/01/introducing-web-activities/). 

### Example #2: Saving a contact

Imagine that you have a company intranet and you want to provide a way to transfer a contact from the online intranet address book to the phone address book. You can do that with the Contacts API.

<<[Saving a contact](code/webapi_samples/contact.js)

This API creates an object with the contact data and saves it into the phone address book without requiring user interaction. Because access to contacts carries potential privacy implications, this API is only available for *privileged apps*. This pattern where you create an object with a success and an error callback is used in many of the WebAPIs.

To learn more about this API, read [the page about the *Contacts API* on the Mozilla Wiki](https://wiki.mozilla.org/WebAPI/ContactsAPI).

### Example #3: Picking an image from the camera

Imagine you are building an application that applies fancy filters to pictures. You want to place a button in your app that allows the user to pick a photo from a photo album or from the camera.

<<[Picking an image](code/webapi_samples/pick.js)

Here we see another example of a [WebActivity](https://hacks.mozilla.org/2013/01/introducing-web-activities/). These activities are available to all applications. In this specific sample we're using the *pick* activity and passing in the *MIME Types* of the files that we wish to retrieve. When this code is executed, the system shows a screen to the user asking where he or she wants to retrieve the image from (camera, gallery, wallpapers). If the user selects an image, the success callback is triggered. If the user cancels the operation, the error callback is executed. On the image below, we can see the dialog that lets the user pick a photo from the device:

![Example of the *pick activity*](images/originals/pick_image.png)

## Summary

In this chapter we saw that, unlike regular web pages, both Firefox OS' hosted apps and packaged apps rely on a manifest file. We also saw that, from a security perspective, packaged apps can be "privileged" or "certified". Only privileged and certified apps can access Mozilla's powerful set of WebAPIs. The more sensitive WebAPIs are not available to hosted apps or to regular web pages. 

Now it's about time we get our hands dirty and create an app!
