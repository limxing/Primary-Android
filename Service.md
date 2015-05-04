Android组件：activity；broadcast Receiver；Service；

##创建一个服务
结论：服务的父类与Activity的父类是同一个ContextWrapper，服务就是一个特殊的没有界面的长期运行的Activity。  

	1、书写一个类继承Service
	2、在清单文件中配置：<service android:name=""></>
	3、复写onCreate

SystemClock.sleep(2000);安卓中的睡眠，不用抛异常。

激活自己的应用程序，用显式：new Intent(this,Activity.class);

##进程的优先级
Foreground process  
前台进程：用户正在操作的应用程序所在的进程就是前台进程  

Visible process  
可视进程：不能操作应用，但能够看到这个程序

Service process  
后台进程：应用程序的一个服务代码正在运行（杀死后能够自动跳出来，但是强制停止后不能自动运行了）

Background process  
后台进程：程序有界面但是被最小化了

Empty process  
空进程：应用程序没有运行任何的Activity service

##服务的生命周期
onCreate()服务只能被创建一次，在创建时执行onCreate方法，

onStartCommand()：启动时

onstart()启动时被弃用

onDestory()销毁时

###使用服务设计一个简易的音乐播放







