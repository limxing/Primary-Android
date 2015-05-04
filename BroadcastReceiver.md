#广播接收者
电池电量不足，SD卡被移除，电话，等事件的发生就是一个广播的产生
##为什么需要广播接受者
特点：即使应用程序进程不存在也，当广播事件来临时，广播接收者的进程也会自动响应，响应广播事件
##常用的广播接收者介绍
####SD卡被移除的广播事例
	1、创建一个类继承BroadcastReceiver,在onReceive方法中编写业务逻辑
	2、在xml中设置receiver android:name="第一步创建类的包名"
	3、在xml文件中配置
	<intent-filter>
		<action android:name="选择MEDIA_UNMOUNTED"/>
		<data android:name="file"></data>
	</intent-filter>
在广播接收者里面开启Activity需要加一个flag  

	intent.setFlag(Intent.FLAG_ACTIVITY_NEW_TASK)
####开机启动的挂广播接收者
配置action:BOOT_COMPLETED
开机启动的广播需要在配置文件中配置permission：RECEIVE_BOOT_COMPLETED

**后退键的业务方法onBackPressed()**
####电话外拨加17951的事例
注意：监听电话外拨的广播需要配置permission  
在onReseiver方法中：getResultData就能获取拨打电话的号码  
setResultData就能够设置这个号码，拨打的时候就能自动加上IP号码
####应用程序的安装和卸载
reveiver:name是自己class的包名
action:PACKAGE_REMOVED/PACKAGE_ADDED  
data:package  
intent.getAction();可获取是卸载还是安装  
intent.getData().toString()可获取下载应用的包名。
####短信的广播接收者
action:android.provider.Telephony.SMS_RECEIVED  
permission：android.permission.RECEIVE_SMS
###不同版本的手机差异
4.0以后版本强行停止后，无法再接收到监听信息，软件被冻结，直到再打开  
**可以做一个一键强行停止的小应用，冻结想冻结的应用，使其不能接受广播**
##发送自定义广播
隐式意图可以跳转其他应用

在发送端的应用中，设置intent.setAction("包名等随便写");sendBroadCast(intent);  
然后再写一个接受者的应用，在其配置文件中设置的action name就是上面的应用

##有序广播和无序广播

**有序广播：**广播消息按照一定的顺序传达的，高优先级的先得到广播消息，的优先级的后得到，高优先级的可以拦截广播消息或者修改广播消息，效率低；  
**无序广播：**优先级相同，随机获取广播内容，但不能修改内容，效率高
###自定义的有序广播
发广播：

	intent.setAction("");
	intent 意图，receiverPermission 接收权限,resultReceiver 结果接受者。。
	sendOrderedBroadcast(intent,null,null,null,1,"内容",null);
可以在发广播的代码中添加自己的接收广播类，不需要在配置文件中添加，（在发送广播的ResultReceiver的地方把其new出来）能够获取自己发送的广播数据。  

收广播：  
在配置文件中设置优先级：receiver的属性中添加provity="1000"优先级

可以在onReceiver的业务中修改广播信息：setResultData("");  
abortBroadcast();终止广播的发送。


我们写的短信接收和IP电话都是有序的广播
###自定无序广播
sendBroadcast(intent)
###系统自带的广播
####手机锁屏状态广播
在安卓里面有一些产生非常频繁的广播事件，在清单文件里面配置是不会生效的。  
点亮变化，屏幕解锁/锁屏，需要在代码中注册。

	protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.activity_main);
		//注册广播接收者
		receiver = new ScreenStatusReceiver();
		IntentFilter filter = new IntentFilter();
		filter.addAction("android.intent.action.SCREEN_OFF");
		filter.addAction("android.intent.action.SCREEN_ON");
		registerReceiver(receiver, filter);
	}
	protected void onDestroy() {
		// 取消注册广播接收者
		unregisterReceiver(receiver);
		receiver = null;
		super.onDestroy();
	}
