# Cocos2d-x中的引用计数（Reference Count）和自动释放池（AutoReleasePool） #

-----------------

## 引用计数

引用计数是c/c++项目中一种古老的内存管理方式。当我8年前在研究一款名叫TCPMP的开源项目的时候，引用计数就已经有了。 

iOS SDK把这项计数封装到了NSAutoreleasePool中。所以我们也在Cocos2d-x中克隆了一套AutoreleasePool。两者的用法基本上一样，所以假如你没有涉及过ios开发，你可以看看苹果官方文档NSAutoreleasePool Class Reference。 

## AutoreleasePool

Cocos2d-x的AutoreleasePool和cocoa的NSAutoreleasePool有相同的概念和API，但是有两点比较重要的不同： 

1. AutoreleasePool不能被开发者自己创建。Cocos2d-x会为我们每一个游戏创建一个自动释放池实例对象，游戏开发者不能新建自动释放池，仅仅需要专注于release/retain cocos2d::Ref的对象。 

2. AutoreleasePool不能被用在多线程中，所以假如你游戏需要网络线程，请仅仅在网络线程中接收数据，改变状态标志，不要这个线程里面调用cocos2d接口。下面就是原因： 



AutoreleasePool的逻辑是，当你调用object->autorelease()，object就被放到自动释放池中。自动释放池能够帮助你保持这个object的生命周期，直到当前消息循环的结束。在这个消息循环的最后，假如这个object没有被其他类或容器retain过，那么它将自动释放掉。例如，layer->addChild(sprite)，这个sprite增加到这个layer的子节点列表中，他的声明周期就会持续到这个layer释放的时候，而不会在当前消息循环的最后被释放掉。 

这就是为什么你不能在网络线层中管理Ref生命周期，因为在每一个UI线程的最后 ，自动释放对象将会被删除，所以当你调用这些被删掉的对象的时候，你就会遇到crash。 

	Ref::release(), retain() and autorelease() 

简而言之，这只有两种情况你需要调用release（）方法 

1. 你new一个cocos2d::Ref子类的对象，例如Sprite，Layer等。 

2. 你得到cocos2d::Ref子类对象的指针，然后在你的代码中调用过retain方法。 

下面例子就是不需要调用retain和release方法： 

	auto sprite = Sprite::create("player.png"); 

这里就没有更多的代码用于sprite了。但是请注意sripte->autorelease()已经在Sprite::create(const char*)方法中被调用了，因此这个sprite将在消息循环的最后自动释放掉。 

## 使用静态构造函数

Sprite::create(“player.png”)是一个使用静态构造函数的例子。所以在Cocos2d-x中所有的类，除了单例，都提供了静态构造函数，这些静态构造函数包含下面4项操作: 

1. 新建一个对象 

2. 调用object->init(…) 

3. 假如初始化成功，例如，成功的找到纹理文件，那么接下来将会调用object->autorelease()。 

4. 返回这个已经被标记了autorelease的对象。 

所有Asdf::createWithXxxx(…)这种类型的函数都有以上这些方式。  
