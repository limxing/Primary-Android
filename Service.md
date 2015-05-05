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
#服务
##调用服务的方法
bindService() 绑定服务，可以间接的调用服务中的方法，可以与服务进行通讯。
###绑定服务调用服务方法的步骤
1、编写服务代码

	public IBinder onBind(Intent intent){}
2、在服务内部定义一个代理人对象MyBinder

	代理人对象有一个方法可以间接的调用服务内部的方法
3、在onBind方法里面返回代理人对象  
4、在Activity代码采用绑定的方式连接到服务

	binService(intent, new MyCo(),BIND_AUTO_CREATE);
5、在serviceConnection的实现类里面有一个方法，获取到服务返回的代理人对象

	public void onServiceConnected(ComponentName name,IBinder service)
6、强制类型转换IBinder转换成MyBinder类型

	myBinder=(MyBinder)service;
7、调用代理人对象的方法-间接调用了服务里面的方法

##服务周期
绑定服务的生命周期

已绑定的方式开启服务是不会执行onStart和onStartcommend  
解除绑定服务：onunbind()--->indestory()  
多次绑定服务，服务只会创建一次，ocreate执行一次  
多次绑定服务，onbind方法不会重复的执行  
在实际开发中，如果需要调用服务的方法，就绑定服务，只能绑定一次  
服务只可以绑定一次，如果用同一个conn绑定多次服务，则解除绑定时会出现异常  
##两种开启服务方式的比较
*	start的方式开启服务  
>服务一旦开启，长期后台运行，服务和开启者（Activity）没有任何关系，开启者退出，服务还是继续在后台长期运行，开启者不可以调用服务里面的方法，在系统设置界面可以观察到；  

*	bind的方式开启服务
>如果开启者退出了，服务也就停止了。开启者可以间接的利用中间人调用服务里面的方法，在系统设置里面看不到进程的运行。

注意：服务如果被开启同时被绑定，服务就停不掉，必须解除绑定服务才可以停止服务。
###混合的方式开启服务
*	为了保证服务又能长期的在后台运行，又能够调用到服务里面的方法，就采用混合的方式开启服务。  
**开启的步骤**：  
1、start的方式开启服务（保证服务的长期后台运行）；  
2、bind方式绑定服务（调用服务的方法）  
3、unbind的方式解除绑定  
4、stop的方式停止服务
##本地服务与远程服务


###绑定远程服务调用服务方法的流程
1、跟本地服务的代码编写一样；  
2、远程服务的接口定义文件IService.java改为.aidl  
3、把接口定义文件的访问权限修饰符全部删掉 public  
4、原来代理人MyBinder extends Binder implements IService--->extends Iservice.Stub  
5、先把远程服务.aidl。文件拷贝到本地应用程序的工程目录里面，包名一直  
6、iservice=IService.Stub.asInterface(service);得到远程服务的代理对象  
7、通过代理对象调用远程对象的方法








