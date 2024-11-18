# advjava
JDBC STEPS:
7 STEPS:
1)import---------java.sql;
2)load and register the driver-----com.mysql.cj.jdbc.Driver;
3)create connection
4)create a statement
5)Execute query
6)Process results
7)close
---------------------------------------------------------------------------------------------------------------------------------------
**Prog 1:
mysql code:**


use charan;
show tables;
create table tab1 
(id int ,name varchar(80));
insert into tab1 values(1,'sai');
select * from tab1;
select name from tab1 where id=1;

**java code:**

package adv1;
import java.sql.*;
public class edit1 {
public static void main(String[] args)throws Exception
{
	
	String url="jdbc:mysql://localhost:3306/charan";
	String uname="root";
	String pass="Sai@2003";
	String query="select name from tab1 where id=1";
	Class.forName("com.mysql.cj.jdbc.Driver");
	Connection con=DriverManager.getConnection(url,uname,pass);
	Statement st=con.createStatement();
	ResultSet rs= st.executeQuery(query);
	rs.next();
	String resname=rs.getString("name");
	System.out.println(resname);
	st.close();
	con.close();
}
}
output:
sai
-------------------------------------------------------------------------------------------------------------------------------------
prog 2:**[prog 1 with modifc in query and mysql:]**
**MYSQL CODE:**

use charan;
show tables;
create table tab1 
(id int ,name varchar(80));
insert into tab1 values(1,'sai');
insert into tab1 values(2,'charan');
insert into tab1 values(3,'tej');
select * from tab1;


**Java code:**
package adv1;
import java.sql.*;
public class prac2 {
	
	public static void main(String[] args)throws Exception
	{
		System.out.println("sai");
		String url="jdbc:mysql://localhost:3306/charan";
		String uname="root";
		String pass="Sai@2003";
		String query="select * from tab1";
		
		
		Class.forName("com.mysql.cj.jdbc.Driver");
		Connection con=DriverManager.getConnection(url,uname,pass);
		Statement st=con.createStatement();
		ResultSet rs= st.executeQuery(query);
		
		String data="";
		
		while(rs.next())
		{
		data=rs.getInt(1)+" : "+rs.getString(2);
		System.out.println(data);
		}
		st.close();
		con.close();

	}
}
OUTPUT:
1 : sai
2 : charan
3 : tej
----------------------------------------------------------------------------------------------------------------------------------------
**prog 3:updating the data:**
package adv1;
import java.sql.*;
public class prac2 {
	
	public static void main(String[] args)throws Exception
	{
	
		String url="jdbc:mysql://localhost:3306/charan";
		String uname="root";
		String pass="Sai@2003";
		String query="insert into tab1 values(4,'cherry')";
		
		
		Class.forName("com.mysql.cj.jdbc.Driver");
		Connection con=DriverManager.getConnection(url,uname,pass);
		Statement st=con.createStatement();
		int count= st.executeUpdate(query);
		
		System.out.println(count+ "rows effected");
		st.close();
		con.close();
	}
}
OUTPUT:
1rows effected
//data updated at mysql also
-----------------------------------------------------------------------------------------------------------------------------
**PROG 4:**
INSERTING IN ANOTHER WAY:
package adv1;
import java.sql.*;
public class prac2 {
	
	public static void main(String[] args)throws Exception
	{
	
		String url="jdbc:mysql://localhost:3306/charan";
		String uname="root";
		String pass="Sai@2003";
		
		int id=5;
		String name="bsct";
		String query="insert into tab1 values(" +id+ ", '"+name+"')";
		
		
		Class.forName("com.mysql.cj.jdbc.Driver");
		Connection con=DriverManager.getConnection(url,uname,pass);
		Statement st=con.createStatement();
		int count= st.executeUpdate(query);
		
		System.out.println(count+ "rows effected");
		st.close();
		con.close();

	}
	
}
OUTPUT:
1rows effected
CHANGES MADE TO DB////
----------------------------------------------------------------------------------------------------------
****PROG 5:**USING PREPARED STATEMNT**
package adv1;
import java.sql.*;
public class prac2 {
	
	public static void main(String[] args)throws Exception
	{
	
		String url="jdbc:mysql://localhost:3306/charan";
		String uname="root";
		String pass="Sai@2003";
		
		int id=6;
		String name="bhogapurapu";
		String query="insert into tab1 values(?,?)";
		
		
		Class.forName("com.mysql.cj.jdbc.Driver");
		Connection con=DriverManager.getConnection(url,uname,pass);
		PreparedStatement st=con.prepareStatement(query);
		st.setInt(1,id);
		st.setString(2, name);
		int count= st.executeUpdate();
		
		System.out.println(count+ "rows effected");
		st.close();
		con.close();

	}
}
OUTPUT:
1rows effected
CHANGES MADE TO DB////
------------------------------------------------------------------------------------------------------------------------------------
PROG 6:
USAGE OF forName method:
//It dynamically load a class w/o creating object

package adv1;

public class prac3 {

	public static void main(String[] args) throws ClassNotFoundException {
		// TODO Auto-generated method stub
		Class.forName("adv1.Sai");

	}

}
class Sai
{
	static
	{
		System.out.println("static block");
	}
	{
		System.out.println("Instance block");
	}
}
OUTPUT:
static block
//instance block not executed since obj not created.
------------------------------------------------------------------------------------------------------------------
PROG 7:
TO LOAD INSTANCE BLOCK, WE HAVE TO USE newInstance method:
package adv1;

public class prac3 {

	public static void main(String[] args) throws ClassNotFoundException, InstantiationException, IllegalAccessException {
		// TODO Auto-generated method stub
		Class.forName("adv1.Sai").newInstance();

	}

}
class Sai
{
	static
	{
		System.out.println("static block");
	}
	{
		System.out.println("Instance block");
	}
}

OUTPUT:
static block
Instance block
-----------------------------------------------------------------------------------------------------------------------------------------
PROG 8:
//FETCHING DATA FROM DB:


package adv1;
import java.sql.*;
public class fetching {
    public static void main(String[] args)
    {
    	Tab1data  t1=new Tab1data();
    	Tab1 t2=t1.getName(4);  //we have to print name belongs to this id
    	System.out.println(t2.tname);//we have to print name from db
    }
}
class Tab1data
{
	public Tab1 getName(int tid)          //method to get name from db
	{
		try
		{    String query="select name from tab1 where id="+tid;
			   Tab1 t=new Tab1();
		     t.tid=tid;
       //we dont have name so we are estalibloshing cinnec to db:
		  Class.forName("com.mysql.cj.jdbc.Driver");
		  Connection con=DriverManager.getConnection("jdbc:mysql://localhost/charan","root","Sai@2003");
		  Statement st=con.createStatement();
		  ResultSet rs=st.executeQuery(query);
		  rs.next();
		  String tname=rs.getString(1);
		  t.tname=tname;
		  return t;
		}
		catch(Exception e)
		{
			System.out.println(e); 
		}
		return null;
	}
}

class  Tab1
{
	int tid;
	String tname;
}


OUTPUT:
cherry
----------------------------------------------------------------------------------------------------------------------------------------------------------













