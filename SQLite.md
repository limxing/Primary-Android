####ListView很重要
#Android数据库的使用
android的存储方式：文件存储、参数SharedPerference、数据库  
###Sqlite---android的数据库
**创建数据库的步骤：**  

	1、	创建数据库帮助类MyDataBaseOpenHelper继承SQLiteOpenHelper类
	2、	构造方法：
	指定数据库文件的名称，数据库的版本号，默认的游标工厂
	public MyDataBaseOpenHelper(Context context){
		super(comtext,"info.db",null,1)
	}
	3、	创建数据库帮助类对象:new MyDatabaseHelper(context);
	4、	创建可读可写的数据库：helper.getWritableDatabase（）;
**创建数据库的表：**  
MyDataBaseOpenHelper中onCreate()方法的作用是：当数据库第一次被创建	``	的时候调用，这个方法只执行一次，用来初始化数据库的表结构（创建表）。
	
	public void onCreate(SQLiteDatabase db){
	db.execSQL("create table student (_id integer primary key)")
	}
	
**数据库的升级（修改等）**  
MyDataBaseOpenHelper中onUpgrade()方法的作用是：当数据库需要被更新的时候调用的方法，当我们修改了构造方法中的版本号（1）时，就会调用onUpgrade()版本只能升级，不能降级

	public void onUpgrade(SQLiteDatabase db,int oldVersion,int newVersion){
		db.execSQL("");
	}
###数据库的增删改查SQL语句
增：insert into student(name,phone) value("zhangsan",'110');  
删：delete from student where name='zhangsan'  
改：update student set phone='119' where name='zhangsan'  
查：

###使用android程序对数据库进行增删改查（02学生信息管理）
0、	构造方法的参数是Context context  
1、	创建数据库  
2、	在Dao类中创建数据库对象，并添加方法进行语句操作；  
注意：在查找时：需要接收一个对象Cursor

###对增删改查进行junit测试
建立类继承AndroidTestCase类..  
还要在xml文件添加两个属性值  

##在命令行中查看数据库中的内容
	>adb shell
	>7# sqlite3 info.db
	sqlite>select * from student;
	>select * from student
更改编码  
chcp　65001  
查看目录下的文件  
ls -1  

￥在<Linout>外添加<ScrollView>就会有往下划拉的效果

#ListView
###ListView的介绍
在页面上显示很多条目，普通的set方法会出现内存溢出，  
解决内存溢出
###listview
*	是系统给我们提供的一个可以显示很多个item的控件
*	这个控件合理的控制了界面的显示，即使有10000000万个item也能够正常显示。
###使用步骤：
	<ListView
	/>
在类中的步骤：  
	
	

接口的默认实现类：BaseXXX，SimpleXXX，DefalutXXX的其中一个  

	public class MainActivity extends Activity {
	//mvc中的view，视图
	private ListView lv;
	@Override
	protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.activity_main);
		//查找视图
		lv = (ListView) findViewById(R.id.lv);
		//设置控制器 controller
		lv.setAdapter(new MyAdapter());
	}
	
	private class MyAdapter extends BaseAdapter{
		//控制ListView中有多少个item
		public int getCount(){
			return 1000;
		}
		//返回某个位置显示的view对象。
		@Override
		public View getView(int position, View convertView, ViewGroup parent) {
			TextView tv = new TextView(MainActivity.this);
			tv.setText("我是文本："+position);
			tv.setTextSize(24);
			return tv;
		}
	}
###升级学生管理系统-db-listview
使用步骤:  
1、	在布局xml文件中声明ListView控件(高不要设置包裹内容)  
2、	在Java代码中找到listview控件  
3、	给ListView设置lv.setAdapter(new BaseAdapter(){})  
4、	书写匿名内部类的两个方法，int getCount()和View getView()  
5、View.inflate()打气筒，可以把xml文件转化成View
###管理系统的优化，
问题：慢慢拖正常，暴力拖动还是会出现内存溢出  
解决：循环利用

	public View getView(int position, View convertView, ViewGroup parent) {
		if(){
			TextView tv = new TextView(MainActivity.this);
			tv.setText("我是文本："+position);
			tv.setTextSize(24);
			return tv;
			}
		}


把一个布局xml文件转换为view对象：  

	View view=null;
	if(convertView==null){
		view=View.inflate(MainActivity.this,R.layout.item,null);
	}else{
		view=convertView;
	}
	
	TextView tv_name=view.findViewById(R.id.);
	ImageView iv_sex=view.findById(R.id.);
	Student student=students.get(potion);
	...  
	iv_sex.setImageResource(R.drawable.nan);
	tv_name.setText(student.getName());
	retur view;
注意：null的原因是把这个view一个父节点，但是我们return这个节点给一个节点，如果不为null则view就有两个父节点，就会报错，想想界面混乱

**删除按钮**(布局修改为相对布局)  
<ImageView  />

	view.findById(R.id.iv_delete).setOnclickListener(new OnClickListener(){
		public void onClick(View v){
			Student student=students.get(position);
			String name=student.getName();
			dao.delete(name);
			refeshDate();//重新查找了数据库，因此删除总是从第一个开始
		}
	})
	public void refeshDate(){
		students=dao.finAll();
		
		
	}

##常见的对话框
**确定取消对话框**

	AlertDialog.Builder builder=new Builder(this);
	builder.setTitle("对话框标题");
	builder.setMessage("对话框内容");
	builder.setPositiveButton("确认",new OnclickListener(){});
	builder.setNegativeButton("确认",new OnclickListener(){});
	AlertDialog dialog=builder.create();
	dialog.show();
**单选对话框**

	AlertDialog.Builder builder=new Builder(this);
	builder.setTitle("请选择");
	String[] items={"男","女"};
	builder.setSingleChoiceItems(items,checkedItem,new OnClickListener(){});
	builder.setNegtiveButton("取消"，null);
	builer.show();
**多选对话框**

	AlertDialog.Builder builder=new Builder(this);
	builder.setTitle("请选择(多选)");
	final String[] items=new String[]{"","","",""};
	final boolean[] checkedItems=new boolean[]{false,false,false,false};//默认
	builder.setMultiChoiceItems(items,checkedItems,new OnMultiChoiceClickListener(){
		public void onClick(DialogInterface dialog, int which,boolean isChecked) {
				Toast.makeText(MainActivity.this,items[which] + isChecked, 0).show();
				checkedItems[which] = isChecked;
			}
		});
	builder.setNegtiveButton("取消"，null);
	builer.show();
**进度对话框**

	ProgressDialog pd=new ProgessDialog(this);
	pd.setTitle("");
	pd.setMessage("");
	pd.show();
	new Thread(){//多少秒后关闭
		public void run(){
		Thread.sleep(3000);
		pd.dismiss();
		};
	}.start();
**进度条对话框**

	ProgressDialog pd=new ProgessDialog(this);
	pd.setProgressStyle(ProgressDialog.STYLE_HORIZONTAL);
	pd.setMessage("");
	pd.show();
	new Thread(){//多少秒后关闭
		public void run(){
		for(int i=0;i<=100;i++){
			Thread.sleep(3000);
			pd.setProgress(i);
		}
		pd.dismiss();
		};
	}.start();

###快速拖动的条
	在<ListView  
		fastS
		/>
###数据库的面向对象操作

	SQLiteDatebase db=helper.getWritableDatabase();
	ContentValues values=new ContentValues();
	values.put("name",name);
	...put...
	long result=db.insert("student",null,values);//执行语句，返回是否成功的状态
	db.close();
	return result;

int i=db.delete("student","name=?",new String[]{name});//返回值为0 则是失败
int i=db.update("student",values,"name=?",new String[]{name});
db.query("student",new String[]{"sex"},"name=?",new String[]{name},null,null,null);


##数据库的事务
db.beginTransaction();//开启事务
try{
db.
db.
db.settransactionSuccessful();//设置事务执行成功
}catch(Exception e){  db.}
finally{
db.endTransaction();
}
db.close();

##常见的适配器
**array适配器**  
把几个条目打在view上
**SimpleAdapter适配器**  
手机设置里面的样子 前面有图片后边有文字
List<Map<String,Object>> data=new ArrayList<Map<String,Object>>();
Map
lv.setAdapter(new SimpleAdapter(this,data,resource,from,to))



##android动画
1、	在res下创建drawable文件夹

##应用程序的国际化
在res创建values-en,把响应的String 文本放进去更改中英文即可
values-zh-rTW

获取文本的方法：getResources().getString(R.String.helloword),不同的语言不同的文件目录下的文件读取

图片也是如此

##样式和主题
样式在values的styles.xml中  
style="@styles/xxx"   
**样式**：作用在控件上 style
**主题**：作用在Activity身上 theme,在style中设置，在AndroidMan..xml中引用
自定义样式和主题，继承父类的样式和主题

