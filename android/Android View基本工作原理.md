### View的基本工作原理

### 1. View是什么？
---- View是Android系统在屏幕上的视觉呈现，也就是你在手机屏幕上看到的东西都是View。

### 2. View是如何绘制出来的？
---- View的绘制流程是从ViewRoot的performTraversals()方法开始，依次经过measure()、layout()和draw()三个过程，最终将View绘制出来。

详细绘制流程：从ViewRoot的performTraversals()方法开始依次调用performMeasure()、performLayout()和performDraw()三个方法。这三个方法分别完成顶级View的measure、layout和draw三大流程。其中performMeasure()会调用measure()，measure()又会调用onMeasure()，在onMeasure()方法中则会对所有子元素进行messure。此时measure流程就从父容器传递到子元素，这样就完成了一次measure过程。接着子元素会重复父容器的measure，如此反复就完成了整个View树的遍历。同理，performLayout()和performDraw()也分别完成类似performMeasure()的流程。通过三大流程，分别遍历整个View树，实现Measure、Layout和Draw这一过程，从而View就绘制出来了。

### 3. View是如何呈现在界面上的？
---- Android中的视图是通过Window来呈现的，不管Activity、Dialog还是Toast，都有一个Window，然后通过WindowManager来管理View。
	 Window和DecorView的通信是依赖ViewRoot来完成的。

### 4. View和ViewGroup
---- 不管简单的Button和TextView还是复杂的RelativeLayout和ListView，共同的基类都是View。View是界面层控件的抽象。View本身可以是单个控件，也可以是多个控件组成的控件组。ViewGroup是控件组，即一组View，ViewGroup也是继承View。