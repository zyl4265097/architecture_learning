# MVP的学习笔记

学习对象：google官方的工程Demo https://github.com/googlesamples/android-architecture   中的todo-mvp

感觉看过的所有介绍MVP的文章里面的图都是垃圾，根本没说清MVP架构具体是咋回事的。

MVP架构中的独特元素：

* 全局接口：
  * BasePresenter
    * subscribe()
    * unsubscribe()
  * BaseView<T>
    * setPresenter(T presenter)
  
* 业务线
  * 接口 XXContract (Contract 翻译是合同，契约的意思，这个取名很叼)
    * 接口 View extends BaseView<Presenter>,内部是各种view会用到的操作方法。
    * 接口 Prensenter extends BasePrenter，内部是各种事物处理的方法。可用于接收view的各种事件，也可做各种数据处理。
  
  * V层 类XXFragment或者XXActivity,都实现了XXContract.View接口
    View的具体操作（赋值，显示隐藏控制等）还是放在了Fragment或者Activity中，但是View的具体控制时机（事件），以及View所需要赋值的数据，都是从  Presenter层来提供的，Presenter通过操作绑定的view的回调接口来抛出view所需要做的响应给Fragment和activity让他们做最终处理。这样一来Presenter并没有与Fragment和activity有非常强的关联性，相当于是给他们打工的，而且Presenter也可以同时打几份工，服务多个老板，他自身的能力有何变化（业务处理逻辑变化）并不一定要通知老板做出改变（除非是约定好的接口方法都改了）。同理，老板也可以同时找几个打工仔（看业务的整体性和业务边界）。
    
  * P层 类XXPresenter 实现了 XXContract.Presenter接口
    获取具体的View事件，并在内部做出相应的数据业务操作，有需要View的子View做出各种展示操作就回调View提供的对应的接口方法。
    Presenter构造函数就绑定了View和自己。
    
  * M层 各种Bean，数据操作句柄等
  
总结：P层主要是隔离了View(fragment,activity)与数据业务逻辑的处理，理清了可见的View的变化线与不可见的数据逻辑变化线。让原本代码可能比较庞大的fragment,activity轻减了很多。在业务复杂的fragment或者activity中优势较MVC模式更为明显
