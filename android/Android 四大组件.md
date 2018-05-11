Android四大组件：
- Activity
- Service
- Broadcasteceiver
- ContentProvider

### 1. Activity
a. Activity是一个单独的屏幕;

b. Activity之间通过Intent进行通信;

c. Activity必须在AndroidManifest.xml配置文件中声明，否则系统将不识别也不执行该Activity。

### 2. Service
a. Service用于在后台完成用户指定的操作。

b. 启动方式：startService()和bindService();

c. 关闭方式：stopService()和unbindService()。

### 3. BroadcastReceiver
a. 对外部事件进行过滤，只对感兴趣的事件进行接收并做出响应;

b. 广播接收者的注册方式有两种：程序动态注册和AndroidManifest.xml静态注册;

c. 动态注册广播接收者特点：当用来注册的Activity关掉后，广播失效;

d. 静态注册广播接收者特点：随系统的启动一直处于活跃状态，只要收到感兴趣的广播就会触发，即使app未启动。

### 4. ContentProvider
a. Android平台提供ContentProvider使一个应用程序的指定数据集提供给其它的应用程序。其它应用可以通过ContentResolver类从该内容提供者中获取或存入数据;

b. 只有需要在多个应用程序间共享数据时才需要使用ContentProvider;

c. ContentProvider实现数据共享。ContentProvider用于保存和获取数据，并使其对所有应用程序可见。（这是不同应用程序间共享数据的唯一方式）

d. 通过ContentResolver对象实现对ContentProvider的操作;

e. ContentProvider使用URI来唯一标识数据集，URI以content://作为前缀，标识该数据由Content Provider来管理。

### 四大组件注册：
----四大组件都需要注册才能使用。每个Activity、Service和ContentProvider都需要在AndroidManifest.xml配置文件中进行配置。AndroidManifest.xml文件中未声明的Activity、Service和ContentProvider都将不为系统所见，从而不可用。而BroadcastReceiver的注册分通过代码动态创建并调用Context.registerReceiver()的方式注册至系统和静态注册(在AndroidManifest.xml文件中进行配置)。

### 四大组件激活：
a. ContentProvider激活：当接收到ContentResolver发出的请求后被激活;

b. Activity、Service和BroadcastReceiver被Intent的异步消息激活。

### 四大组件关闭：
a. Activity：调用finish()来关闭一个Activity;

b. Service：通过startService()启动Service时，需要调用Context.stopService()关闭Service;通过调用bindService()启动Service时，需要调用Context.unbindService()关闭Service；

c. ContentProvider：仅在响应ContentResolver提出请求的时候激活，没必要显示关闭;

d. BroadcastReceiver：仅在响应广播信息时激活，没必要显示关闭。

### Android的任务：Activity栈
----任务其实是Activity的栈，由一个或多个Activity组成，共同完成一个完整的用户体验。栈底是启动整个任务的Activity，栈顶是当前运行的用户可以交互的Activity。当一个Activity启动另外一个Activity时，新的Activity被压入栈，并成为当前运行的Activity。而前一个Activity仍在栈中，当用户按下BACK键时，当前Activity出栈，而前一个Activity恢复为当前运行的Activity。栈中保存的是对象，栈中的Activity不会重排，只会压入或弹出。
