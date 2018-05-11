
Android五大布局：
- FrameLayout
- LinearLayout
- RelativeLayout
- AbsoluteLayout和TableLayout


### 1. FrameLayout
---- 帧布局，组件从屏幕左上方布局组件。

所有添加到FrameLayout的控件放置在该区域的左上角，以层叠的方式显示。

a. 第一个添加的控件放在最底层;

b. 最后一个添加的控件显示在最顶层;

c. 上一层的控件会覆盖下一层的控件。

### 2. LinearLayout
---- 线性布局，按照垂直或水平方向布局组件。

按照垂直或水平方向来布局，通过"android: orientation"属性设置线性布局的方向。属性值有垂直(vertical)和水平(horizontal)两种。

常用属性：

a. android: orientation 设置布局的方向;

b. android: gravity 控制组件的对齐方式;

c. android: layout_weight 控制各个组件在布局种的相对大小。


### 3. RelativeLayout
----相对布局，相对其他组件来布局组件。

按照子控件之间的位置关系完成布局。相对布局的子控件会根据他们所设置的参照控件和参数进行相对布局。参照控件可以是父控件，也可以是子控件。

a. RelativeLayout继承于android.widget.ViewGroup;

b. 在引用其它子控件之前，引用的id必须已经存在，否则将出现异常

### 4. AbsoluteLayout
---- 绝对布局，按照绝对坐标来布局组件。

将所有子控件通过设置android:layout_x和android:layout_y属性，将子控件的坐标位置固定下来，即坐标(android:layout_x, android:layout_y)。

a. layout_x表示横坐标;

b. layout_y表示纵坐标;

c. 屏幕左上角为坐标(0,0);

d. 横向往右为正;

e. 纵向往下为正。

### 5. TableLayout
---- 表格布局，按照行列方式布局组件。

采用行、列的形式管理UI组件，不需要明确包含多少行、多少列，通过添加TableRow、其它组件来控制表格的行数和列数。

a. 向TableLayout中添加一个TableRow代表一行;

b. 向TableRow中添加一个子控件代表一列;

c. 直接向TableLayout中添加组件，表示该组件将直接占用一行。

### PS: Android4.0新增GridLayoutGridLayout：网格布局

---- 与TableLayout相似，把容器划分为rows * columns个网格，每个网格可以放置一个组件。

a. GridLayout性能及功能比TableLayout好;

b. GridLayout布局种的单元格可以跨越多行，而TableLayout不可以;

c. GridLayout渲染速度比TableLayout快。

**布局类型选择与优化：**

a. 布局类型的嵌套越多越深越复杂，会使布局加载变慢，使Activity的启动时间延长，尽量减少布局嵌套，减少View的数量;

b. 减少布局层次，可以考虑用RelativeLayout来代替LinearLayout。通过RelativeLayout的相对其它控件的位置来布局，可减少嵌套;

c. 另一种减少布局层次是使用<merge />标签来合并布局;

d. 重用布局：Android支持在XML中使用<include />标签，通过android:layout属性来指定要包含的布局。
