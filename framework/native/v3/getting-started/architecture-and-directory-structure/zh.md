# 架构和目录结构        

## 1. Cocos2d-x
### 1.1 架构   
![](./res/cocos2d-x-architecture.jpg)    

### 1.2目录结构 

从github下载Cocos2d-x repo资源包后，或解压稳定版ZIP压缩包后，就会看到如下的目录结构：  

|目录名称  |  说明 |
|---------------|----------------------------------------------------------|
|build  | 用于各个平台的编译。 |
|cmake  | 一些cmake的脚本。 |
|cocos	 | Cocos2d-x框架的主目录。|
|docs	 | 你可以下载doxygen文档系统，利用该系统打开本文档文件夹内的doxygen.config文件，然后再生成离线API文档。|
|extensions	 | 3D粒子、自动升级。|
|external	 | 第三方库，比如Box2d及Chipmunk。|
|licenses	 | cocos2d依赖很多其他开源项目。所有授权许可文件都在这个目录。|
|templates	 | 该目录包括在不同集成开发环境及不同平台中创建Cocos2d-x新项目的模板。这里汇集了数量庞大覆盖各种开发环境和平台的模板！|
|tests	 	 | **重要！这是你该开始用到的文件。从cpp-empty-tst**开始学习，你会在`cpp-tests`中发现所有类的用法。`lua-tests`和`js-tsts`也在改目录下。|
|cocos/script	 | 我知道你不喜欢C++，写起来太复杂。没问题，我们有Lua和Javascript。Scripting文件夹包括来自火狐的lua官方引擎和SpiderMonkey引擎。|
|tools	 	 | 包括将C++绑定至lua及javascript的脚本文件。|
|web | cocos2d-html5引擎。 |


## 2. Cocos2d-html5
### 2.1 架构

![](./res/cocos2d-html5-architecture.png)

### 2.2 目录结构

|目录 |	说明|
|---------------|----------------------------------------------------------|
|cocos2d	 |	主目录，包括所有主目录文件及与Cocos2d-x及Cocos2d-iPhone一样的API。平台兼容性佳！|
|extensions	 |	如果需要更多图形用户界面的控制功能、EditBox、CocosBuilder支持、CocoStudio支持或任何第三方库，都在这里。|
|external | 第三方库，如Box2D、chipmunk。|
|licenses | cocos2d依赖很多其他开源项目。所有授权许可文件都在这个目录。|
|template	 |	用于创建Cocos2d-html5新项目的模板。|
|tools	 |	包括JSDoc及Closure Compiler编译器。|

## 3. Javascript绑定

跨平台虽然很好，但还不够好。用C++语言编程不仅超级慢，C++代码还无法在网络浏览器中运行。这就是为什么我们选择增加Javascript绑定功能。通过JSB我们可以将javascript代码打包至：

1. Cocos2d-x+SpiderMonkey上的本地应用，SpiderMonkey能将javascript代码解析成C语言。
2. Cocos2d-html5上的网页应用。

![](./res/jsbinding-1.jpg)     
我们在Cocos2d-iphone、Cocos2d-x 和Cocos2d-html5框架中使用的API组合是一样的。所以，我们可以百分之百在本地Cocos2d-x或Cocos2d-iphone中创建javascript游戏。当你要在浏览器上运行时，你只需要将引擎切换至Cocos2d-html5，而无需修改源代码。

优势：

1. 编码速度要比C++快得多
2. 无需处理野指针、引用计数及内存泄露的问题
3. 跨越本地及网络平台
4. 在线更新

![](./res/jsbinding-2.jpg)