### 1. RelativeLayout和LinearLayout性能PK

LinearLayout
Measure：0.762ms
Layout：0.167ms
draw：7.665ms

RelativeLayout
Measure：2.180ms
Layout：0.156ms
draw：7.694ms

----layout和draw的过程两者相差无几，关键是Measure的过程RelativeLayout却比LinearLayout慢了一大截。

### 2. FrameLayout和LinearLayout性能PK

LinearLayout
Measure：2.058ms
Layout：0.296ms
draw：3.857ms

FrameLayout
Measure：1.334ms
Layout：0.213ms
draw：3.680ms

----layout和draw的过程两者相差无几，关键是Measure的过程LinearLayout比FrameLayout慢一些。
原因在于：LinearLayout在某一方向onMeasure，发现还存在LinearLayout，就存在再次遍历，时间就自然存在消耗。

### 结论：
1. RelativeLayout会让子View调用2次onMeasure()，LinearLayout只调用一次，但是LinearLayout中有weight属性时，也会调用子View2次onMeasure()。
2. RelativeLayout的子View如果高度和RelativeLayout不同，则会引发效率问题，当子View很复杂时，这个问题会更加严重。如果可以，尽量使用padding代替margin。
3. 在不影响层级深度的情况下，使用LinearLayout和FrameLayout，而不是RelativeLayout。
4. 能用两层LinearLayout，尽量用一个RelativeLayout，在时间上此时RelativeLayout耗时更小。
5. LinearLayout慎用layout_weight属性，会增加一倍的耗时操作。


### 问题：
为什么Google给开发者默认新建了个RelativeLayout，而自己却在DecorView中用了个LinearLayout？

----因为DecorView的层级深度是已知而且固定的，上面一个标题栏，下面一个内容栏。采用RelativeLayout并不会降低层级深度，所以此时在根节点上用LinearLayout是效率最高的。而之所以给开发者默认新建了个RelativeLayout是希望开发者能采用尽量少的View层级来表达布局以实现性能最优，因为复杂的View嵌套对性能的影响会更大一些。