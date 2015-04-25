m##软件的测试

###分类：  
黑盒测试：不知道软件的源代码  
白盒测试：知道应用程序的源代码  
###测试的粒度
单元测试：junit Test  
集成测试：intergration Test  
系统测试：System Test  
###测试的程度
压力测试（pressure Test）  
冒烟测试（smoke Test）自带monkey工具进行测试，adb命令行，暴力测试  

##软件的单元测试
安卓下的junittest：  
需要把应用程序部署到真是的手机或模拟器中，  
步骤：
1、编写一个业务方法  
2、

##Logcat的使用
模拟器4.1.1版本稳定 不会乱打日志  

Log.v(tag,msg) verbose提醒  黑色  
Log.d(tag,msg) debug调试  蓝色  
Log.i(tag,msg) info信息  绿色  
Log.w(tag,msg) warn警告  黄色  
Log.e(tag,msg) error错误  红色  
Log.wtf(tag,msg) 很严重的错误 what the fuck  红色
##文件保存路径
###应用程序内部
this.getFilesDir()===/date/date/包名/files/  保存重要的配置信息  
this.getCacheDir()===/date/date/包名/cache/  缓存目录
###SD卡公共目录
**检查SD卡是否存在，是否可用：**  
Envirment.getExternalStorageState();返回值是Envirment的常量  
Environment.MEDIA_MOUNTED说明SD卡可用  
**查询可用空间**  (2.3版本以后的手机Api9以上)
long size=Envirment.getExternalStorageDirctory().getFreeSpace();  
String info=Formatter.formatFileSize(this,size);  
**获取外部存储卡的目录**  
sdcard在不同的手机路径不相同，因此不能直接使用/mnt/sdcard/  
Environment.getExternalStorageDirectory()===/mnt/adcard/  
**权限：**
>写的权限 write-external-storage  
>读的权限（4.0以后）read_external-storage
##文件的权限
*	应用程序在/date/date/包名/目录下存储的文件默认是私有的，其他应用程序无法访问
*	FileOutputStream fos=openFileOutput("info.txt",Context....);不再使用这种方式，在应用程序的目录里创建其他程序也能访问的文件。

##使用sharedPreferences
**其实写的是一个xml文件**  info.xml,0表示私有的

*	声明sp：private SharedPreferences sp;
*	获取到一个参数：sp=this.getSharedPreferences("info",0);
*	得到sp文件的编辑器：Edite editor=sp.edit();
*	存储数据：editor.putString("qq",qq)
*	提交事务：editor.commit()
*	获取数据：sp.getString("qq");...getInt("")

##使用安卓Api面向对象的方式
*	XmlSerializer serializer=Xml.newSerializer();得到序列器
*	File file=new File(getFilesDir,name+".xml");设置参数
*	FileOutputStream os=new FileOutputStream(file);
*	serializer.setOutput();
*	serializer.startDocument("utf-8",true);写开头
*	serializer.startTag(null,"name");开始标签
*	serializer.text();写入文本标签
*	serializer.attribute(null,name,value);写属性
*	serializer.endTag(null,"name");结束标签
*	serializer.endDocument();写结尾
##解析XML文件
*	SAX
*	Dom4J
*	Pull(安卓的解析)：
>获得一个解析器  
>XmlPullParser parser=Xml.newPullParser();  
>设置参数   
>FileInoutStream inputStream=new FileInputStream(file);  
>parser.setInput(inputStream,"utf-8");  
>解析XMl文件
>int type=parser.getEventType();得到第一个事件的类型  

	while(type!=XmlPullParser.END_DOCUMENT){
		if(type==XMlPullParser.STAR_TAG){
		
			if("name".equals(parser.getName())){
				String nameStr=parser.nextText();

			}

		}
	type=parser.next();
	}
	inputStream.close();


##老师案例中的代码笔记
男女单选<RadioGroup


/>
