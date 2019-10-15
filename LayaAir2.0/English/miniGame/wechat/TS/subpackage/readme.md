# 微信小游戏分包实战

> author: Charley

For some large-scale games, the 4M initial package of Weixin small game is far from enough, because JS alone will exceed 4M, so before the launch of the 2.1 small game base library, it can only continue to hack functions until JS is less than 4M. If a novice doesn't understand why? Let's look at some of the basics first and then at this article. ) Since version 2.1, the basic library of mini-games has supported the expansion of uploaded packages to 8M through subcontracting. How to subcontract?

**This article will not only introduce the way of subcontracting, but also for the common problems encountered in the process of subcontracting, through the example of DEMO, to help developers understand the way of subcontracting small games and matters needing attention.**



###Do you really need subcontracting?

If you are a developer who is not familiar with subcontracting processes or windows domains, subcontracting will face some problems. In addition, before we plan to subcontract, we must analyze whether our project really needs subcontracting. In fact, for most of the current products, you can go online without subcontracting.

####1. Have you used UI loading or splitting mode?

LayaAir engine developers, UI is mostly produced through LayaAirIDE.

In F9 UI mode options, as well as project manager, right-click on each UI page to set default properties when the export type options, you can see the embedded mode, load mode, split mode, three options.

![图1](img/1.png) 


**The default is embedded mode**In this mode, when exporting UI pages, content such as configuration information is exported to the code file of the project. The JS file is the final release of the game. Thus occupied some valuable small game local package volume. therefore**To reduce the package size of the game, you can change the mode of exporting UI to load mode or separate mode.**Both modes export page configuration information to JSON files, which can be dynamically loaded remotely through the URL without taking up local package space.

>**Tips:**
>
> 1. The difference between loading mode and detaching mode is that loading mode is to export all UI pages into a JSON file, and detaching mode is to export each UI page into a separate json.
>
> 2. It should be noted that the loading mode and the separating mode can be used only after the code is loaded because the JSON is exported. Embedded mode is not required.

In conclusion, loading mode and separation mode can reduce the size of inclusion JS. If it can be solved in this way, subcontracting may not be necessary. The specific situation depends on the project.

#### **2. Delete unnecessary JS code**

In the absence of subcontracting, JS referenced in HTML pages are merged into a JS file (code. js), unless otherwise referenced in the project. Otherwise, other JS that are not in the HTML page can be deleted directly, such as some unused engine libraries js. It can be deleted directly from the project directory so that it will not appear again when it is published.

####3. Compression and confusion

By compressing the obfuscated JS code, the package experience is significantly reduced. If the JS does not exceed 4M, you can avoid scoring. Resources and other content can be dynamically loaded using the URL. After the first load, there will be physical caches, not more than 50M of commonly used cached content. Next time you open it, there is no need to load.



###2. Learning the Official Subcontracting Documents of Small Games

Before the actual combat subcontracting, the official documents that have not been read must be read carefully first. This is very useful, no matter how much you can understand, first try to understand the main points of the document, in order to better understand subcontracting. The links are as follows. Please read them before proceeding to the next steps.

[https://developers.weixin.qq.com/minigame/dev/tutorial/base/subpackages.html](https://developers.weixin.qq.com/minigame/dev/tutorial/base/subpackages.html)



###3. The Official Subcontracting Method of Wechat Games

Although many developers have seen the official subcontracting documents, here's a quick look at the key points.

####1. Configure the fields of the subpackage name and path in game.json


```json

{
  ...
  "subpackages": [
    {
      "name": "stage1",
      "root": "stage1/" // 可以指定一个目录，目录根目录下的 game.js 会作为入口文件，目录下所有资源将会统一打包
    }, {
      "name": "stage2",
      "root": "stage2.js" // 也可以指定一个 JS 文件
    }
  ]
  ...
}
```


In subpackages, there can be multiple names and roots, each group represents a subpackage, a single subpackage, not more than 4M, the initial package of all games can not exceed 8M.

Let's take a look at the structure and notes of subcontract configuration, and have a preliminary understanding. If you still don't understand, you can combine the configuration of the actual combat to understand.

####2. Sample Code for Official Packet Loading of Small Game

Game Officials Officially Provided[wx.loadSubpackage()](https://developers.weixin.qq.com/minigame/dev/document/subpackages/wx.loadSubpackage.html)API triggers the download of subpackages. After calling wx. loadSubpackage, it triggers the download and loading of subpackages. After loading is completed, the loading is notified by the success callback of wx. loadSubpackage. The sample code is as follows:


```javascript

const loadTask = wx.loadSubpackage({
  name: 'stage1', // name 可以填 name 或者 root
  success: function(res) {
    // 分包加载成功后通过 success 回调
  },
  fail: function(res) {
    // 分包加载失败通过 fail 回调
  }
})
```


When the load is successful, wx. loadSubpackage returns a[LoadSubpackageTask](https://developers.weixin.qq.com/minigame/dev/document/subpackages/LoadSubpackageTask.html)The current download progress can be obtained through LoadSubpackage Task. The sample code is as follows:


```javascript

loadTask.onProgressUpdate(res => {
  console.log('下载进度', res.progress)
  console.log('已经下载的数据长度', res.totalBytesWritten)
  console.log('预期需要下载的数据总长度', res.totalBytesExpectedToWrite)
})
```


This document mainly talks about the subcontracting methods and the subcontracting problems caused by the window domain that developers often encounter. The download schedule is easy to understand, and there are no feedback problems from developers, so it is not mentioned in the actual code. If you encounter this problem, it can be raised in the community.



###IV. Download sample projects

I have prepared two simple sample projects for you. After downloading and decompressing, the defaultDemo directory is the sample project before subcontracting, and the subPackageDemo directory is the sample project after subcontracting. While reading this document, developers can compare the differences between pre-subcontracting and post-subcontracting projects to help understand the subcontracting of small games.

The download address is:[https://github.com/layabox/layaair-doc/raw/master/project/TS/TS_subPackage_Demo.zip](https://github.com/layabox/layaair-doc/raw/master/project/TS/TS_subPackage_Demo.zip)



###V. Key Points of Actual Subcontracting

####1. Wechat Developer Tools and Publishing Project Notes

The first step in actual subcontracting is to create a small game project in the Wechat Developer Tool. Because once subcontracted, using a small game loading mechanism, the browser will not work, the entire debugging process is completed in the Wechat developer tools. So, download the sample project ready for you, first open the example in the defaultDemo directory, and release a small version of the game. Run the basic debugging process through.

> Tips: It's important to note that the downloaded project, because it has been published, the default record is the published directory, so when publishing, we must change the actual directory cost.

####2. Basic Library Version

Be sure to check what version of the debugging base library of the Wechat Developer Tool is, otherwise, following this article, the version that does not support subcontracting will cause debugging problems.

Developer tools use version 1.02.1806120 and above.

Basic libraries use versions 2.1.0 and above.

This document uses 2.2.0. As shown in Figure 1:

![图2](img/2.png) 


(Fig. 2)

####3. Relevant operation of subcontract directory

#####Modify game.json

Before subcontracting, we need to plan the subcontracting directory and reflect it in game.json.

Here, we simply set up a subpackage directory B. You can first change the game.json in the sample project under defaultDemo to the following code:


```json

{
  "deviceOrientation": "landscape",
  "showStatusBar": false,
  "networkTimeout": {
    "request": 10000,
    "connectSocket": 10000,
    "uploadFile": 10000,
    "downloadFile": 10000
  },
  "subpackages": [
    {
      "name": "subpackage",
      "root": "js/subpackage/"
    }
  ]
}
```


After planning and setting up the sub-directory of the game. Let's create subcontracted directories and files.

#####Notice the root path

TS project`src`The project code in the directory is compiled and published if`bin/index.html`There are quotations. It will be integrated into code.js together with the engine library. Be not in`bin/index.html`References are copied directly to`js`Under the directory. therefore`root`Don't miss js. As shown in Figure 3.

![图3](img/3.png) 


(Fig. 3)

#####An important compilation pit

When subcontracting, the TS project also has a pit caused by IDE compilation, that is, each compilation of the TS project generates a new JS to bin directory. However, after each generation, the generated JS reference is automatically updated to index. html. However, just mentioned that all the references in index.html will be incorporated into code.js. We don't want to incorporate the subcontracting code into code.js. So after each compilation, before releasing the game. Be sure to open index. HTML to see if subcontracted JS are referenced. If quoted, be sure to comment it out. As shown in Figure 4.

![图4](img/4.png) 


(Fig. 4)

> Tips: In future versions, if you have time to consider IDE solution, please pay attention to it before solving. And avoid subcontracting failures due to references when publishing.

If the experience of each release check is not very pleasant. It is recommended that developers create a new project for subcontracted content. Equivalent to one project in the main contract, one project in each subcontract. Load other subpackages in the main package and use the Windows domain to interact.

> For more information on loading and windows domains, you can continue to look at this article's introduction.



#####Create game.js

Although a specific JS file can be specified as the entry in root, considering that there may be multiple JS in the subpackage, the default entry game.js for the directory is used in this document example.

Game.js we can go directly to bin directory and create it in the compiled subcontracting directory. The subpackage JS path is introduced in game.js, as shown below.


```javascript

require('b.js');
```




####4. Beginning subcontracting coding

After creating the subpackage directory and subpackage file in the previous step, you can start subpackage coding.

First of all, in principle, since subcontracting is necessary, then**The logical relevance between the main package and the subpackage should be as little as possible.**。

Of course, some developers will inevitably need some related requirements for the calls between the main package and the subpackage. So I'm going to give you a simple example, which is to take a part of the logic originally in a main package and put it in a subpackage.

Open the sample project in the defaultDemo directory, and we only keep the general UI display method showUI. In the callback onLoaded after loading the atlas, we retain the logic of displaying the UI for the first time. Put the logic of button monitoring and page switching into b.ts.

The separated b.ts code is as follows:


```javascript

/**
* 分包 
*/
module subpackage{

	export class b{
        private GameMain:any;
        private ui:any;
		constructor(){
             //监听按钮btnA的点击事件，触发后处理
            this.GameMain.newUI.btnA.on(Laya.Event.CLICK, this, this.showB);
		}

            //显示B页
        private showB():void
        {
            this.GameMain.showUI(this.ui.bUI,this.GameMain.newUI)

            //监听按钮btnB的点击事件，触发后处理
            this.GameMain.newUI.btnB.on(Laya.Event.CLICK, this, this.showA);
        }

        //显示A页
        private showA():void
        {
           this.GameMain.showUI(this.ui.aUI,this.GameMain.newUI)
        
            //监听按钮btnA的点击事件，触发后处理
            this.GameMain.newUI.btnA.on(Laya.Event.CLICK, this, this.showB);
        }
	}

}
//实例化
new subpackage.b();
```


After the code is separated, we should not forget to call the subpackage loading and callback notification method provided by the official Weixin game in the main package. In the sample project, we load the subpackage directly in the callback of the atlas loading. Then output after successful loading`success`Log. The sample code is as follows:


```javascript

//图集加载后回调
private onLoaded():void
{
    this.showUI(ui.aUI);
    
	//小游戏官方的分包加载方式
    const loadTask = wx.loadSubpackage({
        name: 'subpackage', // name 可以填 name 或者 root
        success: function(res) {
            // 分包加载成功后通过 success 回调
            console.log("success");
        },
        fail: function(res) {
            // 分包加载失败通过 fail 回调
            console.log("fail");
        }
    });       
}
```


At this time, according to the official documentation of the game, in theory, the subcontracting process should be over. We can release the game code and see the effect in the Wechat developer tool.

Not surprisingly, there must be an error. We can continue to read the document.

####5. Windows Domain

In browsers, the default is in the Windows domain. The game is not, so the call between multiple JS will be a problem, so to solve this problem, when IDE is released, all project JS and engine will be integrated into code. js. Now the subcontracting scheme will face the problem of windows domain. Therefore, when the main package and subpackage have call requirements, the function or variable that will be called must be put into the window domain first. Then use the keyword "window" in front of it. Let's use the example project to experience the battle.

First, in the main package, we put the UI classes and the main package classes used in the subpackage into the window domain, so that the subpackage can be used directly from the window when needed, as shown below.


```javascript

//把需要被分包中使用的放到window域里
window["ui"] = ui;
window["GameMain"] = new GameMain();
```


In subpackage b.ts, we need to extract UI and GameMain classes from windows. The following code can be added:


```javascript

//从window域里取出
this.ui = window["ui"];
this.GameMain = window["GameMain"];
```


> In a specific battle, developers can compare two sample projects before and after subcontracting.

Similarly, if subpackaged classes are used in the main package, they should be placed in the window domain first, and then retrieved using the window keyword. The specific use is so simple. Through the understanding of the window domain. Windows-related problems encountered in subcontracting can be solved.

###6. Developer's Practical Suggestions

Developers can subcontract the sample projects I gave them first. If they encounter problems, they can see this document and compare the differences between the two sample projects I gave them. Run through the Wechat game first. After the real understanding of subcontracting, we can subcontract the free practical games. If you encounter a problem, please send it to the community and upload a sample DEMO project in the problem. The group can @administrator Charley and provide a link.

In the future, if there are new problems faced by developers in subcontracting, I will improve and update this document.



##This article appreciates

If you think this article is helpful to you, you are welcome to sweep the code and appreciate the author. Your motivation is our motivation to write more high quality documents.

![wechatPay](../../../wechatPay.jpg)