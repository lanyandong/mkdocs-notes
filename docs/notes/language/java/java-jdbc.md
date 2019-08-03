### jdbc基本使用流程

第一次使用的话，需要使用mysql 的jdbc 驱动jar包。为了对数据库进行增删改查，首先需要与数据库进行连接，通常使用jdbc，具体的步骤流程如下：

1、首先建立连接，步骤是基本是固定的：

```java
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;

public class Main {
	
	static String url = "jdbc:mysql://localhost:3306/studenttest?useUnicode=true&characterEncoding=utf-8&serverTimezone=GMT";
	static String user = "root";
	static String password ="";
	public static void main(String[] args) throws Exception {
		//1.注册驱动
		Class.forName("com.mysql.cj.jdbc.Driver");
		//2.获取连接
		java.sql.Connection con = DriverManager.getConnection(url, user, password);
		//3.获得预处理对象
		String updsql = "update student set name=? where id=?";
		String selsql = "select * from student";
		PreparedStatement stat = con.prepareStatement(updsql);
		PreparedStatement stat2 = con.prepareStatement(selsql);
		//4.SQL语句占位符设置实际参数
		//SQL语句占位符设置实际参数（占位符就是先占住一个固定的位置，等着你再往里面添加内容的符号，广泛用于计算机中各类文档的编辑）
		stat.setString(1, "小明");    //索引参数1代表着sql中的第一个？号，也就是我需要将条件id所对应的name数据更新为“小明”
		stat.setString(2, "2016222");//索引参数2代表着sql中的第二个？号，也就是条件是id为"2016222"
		//5.执行SQL语句,
		int line = stat.executeUpdate(); //int executeUpdate(); --执行insert update delete语句。
		System.out.println("更新记录数"+ line);
		
		ResultSet rs = stat2.executeQuery(selsql);//ResultSet executeQuery(); --执行select语句。
		while(rs.next()) {
        	String id = rs.getString("id");
        	String name = rs.getString("name");
        	String gendar = rs.getString("gendar");
        	String age = rs.getString("age");
        	System.out.println("id=" + id + "\tname=" + name + "\t" + "\t" + "gendar=" + gendar + "\tage=" + age);	
        }
		
		//6.释放资源
		stat.close();
		stat2.close();
		con.close();
	}
}
```

2、关于`PreparedStatement `接口

`Statement` 对象用于将SQL语句发送到数据库中执行，并从数据库中读取结果，执行的是不带参数的静态SQL语句。

而 `PreparedStatement` 接口是`Statement` 的子接口，可以用于执行动态的SQL语句的。SQL 语句被预编译并存储在 `PreparedStatement` 对象中，然后可以使用此对象多次高效地执行该语句。当然 `PreparedStatement` 对象也可以用于执行不带参数的预编译SQL语句。