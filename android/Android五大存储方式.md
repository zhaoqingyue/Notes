


### Android存储方式：
- SharedPreferences
- 文件存储
- SQLite数据库
- ContentProvider
- 网络存储

### 1. SharedPreferences
a. 默认存储路径：/data/data/<PackageName>/shared_prefs

b. 操作模式：

- MODE_PRIVATE（默认）：只有当前的应用程序才能对文件进行读写；
- MODE_MULTI_PROGRESS：用于多个进程对同一个SharePreferences进行读写。

c. 存储数据格式：键值对

获取SharePreferences对象的方法：

- Context.getSharePreferences(String name, int mode);

    name: 文件名
    
    mode：操作模式，默认Context.MODE_PRIVATE
                
- Activity.getPreferences(int mode); 使用当前应用程序包名为文件名

    mode：操作模式，默认Context.MODE_PRIVATE

- PreferenceManager.getDefaultSharedPreferences(Context context); 使用当前应用程序包名为文件名

    context：上下文
    
### 数据存储

- 调用SharePreferences对象的edit()方法获取一个SharePreferences.Editor对象；
- 向Editor对象中添加数据putInt()、putBoolean()、putString()等；
- 调用commit()方法提交数据。

```
SharedPreferences sp = Context.getSharedPreferences("person", Context.MODE_PRIVATE);
SharedPreferences.Editor editor = sp.edit();
editor.putString("name","zhaoqy"); 
editor.putInt("age",12); 
editor.putBoolean("isMarried",false); 
editor.commit();
```

### 数据读取

- 获取SharePreferences对象；
- 调用getInt()、getBoolean()、getString()获取对应key的值。

```
SharedPreferences sp = Context.getSharedPreferences("person", Context.MODE_PRIVATE);
String name = pref.getString("name"); 
int age = pref.getInt("age"); 
boolean isMarried = pref.getBoolean("isMarried");
```

### 2. 文件存储
a. 默认存路径：/data/data/<PackageName>/files；

b. 操作模式：
- MODE_PRIVATE（默认）：覆盖
- MODE_APPEND：追加

### 写入文件
```
public void save() { 
	String data = "save something here"; 
	FileOutputStream out = null; 
	ButteredWriter writer = null; 
	try { 
		out = openFileOutput("data", Context.MODE_PRIVATE); 
		writer = new ButteredWriter(new OutputSreamWriter(out)); 
		writer.write(data); 
	} catch(IOException e) { 
		e.printStackTrace(); 
	} finally { 
		try { 
			if(writer!=null) { 
				writer.close(); 
			} 
		} catch(IOException e) { 
			e.printStackTrace(); 
		} 
	}
}
```
### 读取文件

```
public String load() { 
	FileInputStream in = null; 
	ButteredReader reader = null; 
	StringBuilder builder = new StringBuilder(); 
	try { 
		in = openFileInput("data"); 
		reader = new ButteredReader(new InputStreamReader(in)); 
		String line= "";
	    while((line = reader.readline()) != null) { 
	    	builder.append(); 
	    } 
	} catch(IOException e) { 
		e.printStackTrace(); 
	} finally { 
		if(reader != null) { 
			try { 
				reader.close(); 
			} catch(IOException e) { 
	        	e.printStackTrace(); 
	        } 
	    } 
	} 
}
```

### 3. SQLite数据库
a. 默认存储路径：/data/data/<PackageName>/databases

b. 数据类型：
- integer 整型
- real 浮点型
- text 文本类型
- blob 二进制类型


```
public class DatabaseHelper extends SQLiteOpenHelper { 

	public static final String CREATE_BOOK = "create table book ( " + 
		" id integer primary key autoincrement," + " author text," + 
		"price real," + "pages integer," + "name text)"; 
	private Context context; 
	public DatabaseHelper (Context context, String name, CursorFactory factory, int version) { 
		super(context, name, factory, version); 
		this.context = context; 
	} 

	@Override 
	public void onCreate(SQLiteDatabase db) { 
		db.execSQL(CREATE_BOOK); 
	} 


	// 当打开数据库时传入的版本号与当前的版本号不同时会调用该方法 
	@Override 
	public void onUpgrade(SQLiteDatabase db, int oldVersion, int newVersion) { 

	} 
}
```

### 初始化
```
DatabaseHelper helper = new DatabaseHelper(this,"BookStore.db",null,1); 
// 检测到没有BookStore这个数据库，会创建该数据库并调用DatabaseHelper中的onCreated方法。 
helper.getWritableDatabase();
```

### 升级数据库
```
public class DatabaseHelper extends SQLiteOpenHelper { 
	...... 
	// 当打开数据库时传入的版本号与当前的版本号不同时会调用该方法
	@Override 
	public void onUpgrade(SQLiteDatabase db, int oldVersion, int newVersion) { 
		db.execSQL("drop table if exists Book"); 
		onCreate(db);
	} 
}

// 将version改为大于原来版本号
DatabaseHelper helper = new DatabaseHelper(this,"BookStore.db",null, 2);
helper.getWritableDatabase(); 
```

### 插入

```
SQLiteDatabase db = helper.getWritableDatabase(); 
ContentValues values = new ContentValues(); 
values.put("name", "The Book Name"); 
values.put("author", "chen"); 
values.put("pages", 100); 
values.put("price", 200); 
/**
 * 参数一: 表名;
 * 参数二: 在未指定添加数据的情况下给某些可为空的列自动赋值为NULL，设置为null即可；
 * 参数三: ContentValues对象。
 */
db.insert("Book", null, values);
```

### 更新

```
SQLiteDatabase db = helper.getWritableDatabase(); 
ContentValues values = new ContentValues(); 
values.put("price",120); 
/**
 * 参数一: 表名；
 * 参数二: ContentValues对象，
 * 参数三、四: 去约束更新某一行或某几行的数据，不指定默认更新所有。
 */
db.update("Book", values, "name= ?", new String[]{"The Book Name"});
```

### 删除

```
SQLiteDatabase db = helper.getWritableDatabase();
/**
 * 参数一: 表名；
 * 参数二、三: 约束删除某一行或某几行的数据，不指定默认删除所有。
 */
db.delete("Book", "pages> ?", new String[]{"100"});
```

### 查询

```
SQLiteDatabase db = helper.getWritableDatabase(); 
/**
 * 参数一: 表名;
 * 参数二: 指定查询哪几列，默认全部;
 * 参数三、四: 去约束更新某一行或某几行的数据，不指定默认查询所有;
 * 参数五: 指定需要去group by的列;
 * 参数六: 对group by的数据进一步的过滤;
 * 参数七: 查询结果的排序方式;
 */
Cursor cursor = db.query("Book",null,null,null,null,null,null); 
if(cursor.moveToFirst()) { 
	do { 
		String name = cursor.getString(cursor.getColumnIndex("name"); 
		String author = cursor.getString(cursor.getColumnIndex("author"); 
		int pages = cursor.getString(cursor.getColumnIndex("pages"); 
		double price = cursor.getString(cursor.getColumnIndex("price"); 
	} while(cursor.moveToNext()); 
} 
cursor.close():
```

### SQL语句操作数据库

```
//添加数据 
db.execSQL("insert into Book(name,author,pages,price) values(?,?,?,?) " ,new String[]{"The Book Name","chen",100,20}); 
//更新数据 
db.execSQL("update Book set price = ? where name = ?",new String[] {"10","The Book Name"}); 
//删除数据 
db.execSQL("delete from Book where pages > ?",new String[]{"100"}); 
//查询数据 
db.execSQL("select * from Book",null);

```

### 事务操作

```
SQLiteDatabase db = helper.getWritableDatabase(); 
//开启事务 
db.beginTransaction(); 
try { 
	...... 
	db.insert("Book",null,values); 
	//事务成功执行 
	db.setTransactionSuccessful(); 
} catch(SQLException e) { 
	e.printStackTrace(); 
} finally { 
    //结束事务
	db.endTransaction(); 
}
```

### 4. ContentProvider
a. 内容URI：内容URI是由权限和路径组成，权限是用于区分不同的应用程序，一般以包名来命名；路径是用于区分同一个应用程序的不同表；

```
// 包名为com.xxx.xxx的表xxx_table的访问路径
String AUTHORITY = "com.xxx.xxx";
String TABLE = "xxx_table";
Uri URI = Uri.parse("content://" + AUTHORITY + "/" + TABLE);
```

b. 使用Uri对象进行数据操作
- 插入
```
ContentValues values = new ContentValues();
values.put("column1","text");
values.put("column2",1);
getContentResolver().insert(uri,values);
```

- 更新
```
String where = null;
ContentValues values = new ContentValues();
values.put("column1","text");
values.put("column2",1);
getContentResolver().update(URI, values, where, null);
```

- 查询

```
Cursor cursor = getContentResolver().query(uri,null,null,null,null);
if(cursor != null) { 
	while(cursor.moveToNext()) { 
		String column1 = cursor.getString(cursor.getColumnIndex("column1")); 
		int column2 = cursor.getInt(cursor.getColumnIndex("column2")); 
	} 
	cursor.close(); 
}

```

- 删除
```
String where = null;
getContentResolver().delete(URI, where, null);
```

### 5. 网络存储
a. 使用网络保存数据；

b. 需要的时候，从网络上获取该数据，进而显示在app中。