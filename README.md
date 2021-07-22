# MySQL
### 1、安装和配置MySQL
安装好数据库后还需要配置自己的数据库的用户名和密码。

> 格式：mysql> set password for 用户名@localhost = password('新密码');
> 
> 例子：mysql> set password for root@localhost = password('123'); 


右击“开始”菜单，选择Windows PowerShell(管理员)(A)查看mySql是否成功安装
```
进入数据库命令：mysql -u root -p
```
![image](https://user-images.githubusercontent.com/87808803/126661288-21fc2590-21b2-48e8-a7a7-2c311d23304d.png)
*注意执行语句最后要加“；”结束，否者将无法结束*

同时，还要下载一个MySQL的客户端，用于可视化查看数据库内容，这里使用的是MySQL-Front
![image](https://user-images.githubusercontent.com/87808803/126661424-879c7b0d-2df2-4cec-bebc-0666a54a037b.png)



### 2、加载驱动程序
  JDBC（Java DataBase Connectivity）是指java数据库连接，是用于执行SQL语句的Java应用程序接口，提供了诸如查询和更新数据库中数据的方法。
  
  下载jdbc驱动包
  
![image](https://user-images.githubusercontent.com/87808803/126661665-a56858d8-5b93-439c-9a17-2e01c19269fa.png)

将该驱动包放置在tomcat服务器文件的lib目录下，即可以使用了.

![image](https://user-images.githubusercontent.com/87808803/126661864-135b1aa7-f7cc-4985-afee-7f5353c520d2.png)



### 3、新建数据库，导入数据
  打开MySQL客户端，使用配置的用户名密码登录，添加一个新数据库dict，并导入map_enword.sql的数据。
![image](https://user-images.githubusercontent.com/87808803/126661975-7d9fdb3a-2ffb-4d3a-9d10-98659525b77e.png)

        

### 4、使用JSP链接数据库             
JSP(Java Server Pages)，其根本是一个简化的Servlet设计，它实现了在Java中使用HTML标签。简而言之，就是在html源码中嵌入java代码，以Java语言为基础动态开发网页。

##### JSP的运行原理：
![image](https://user-images.githubusercontent.com/87808803/126662034-55b64fb0-f478-42f2-8c01-d29efd270a8c.png)
  
  运行过程中.jsp文件会自动生成.java文件，再编译成.class文件传入数据库，并且可以通过执行java在服务器中读出数据库的数据，即可以把数据通过服务器显示出来。

 从dict数据库中读取英语单词统计表的JPS代码如下：
```
<body>
<div id="wc"></div>
<%@ page contentType="text/html; charset=gb2312" %>    //定义网页依赖属性
<%@ page language="java" %> 
<%@ page import="com.mysql.jdbc.Driver" %> 
<%@ page import="java.sql.*" %> 
<% 
                        out.print("<center><h1><font color=blue>Matrix Word Cloud English Learning</h1></center>"); 
                        //驱动程序名 
                        String driverName="com.mysql.jdbc.Driver"; 
                        //数据库用户名 
                        String userName="****"; 
                        //密码 
                        String userPasswd="***********"; 
                        //数据库名 
                        String dbName="dict"; 
                        //表名 
                        String tableName="map_enword"; 
                        //联结字符串 
                        String url="jdbc:mysql://localhost/"+dbName+"?user="+userName+"&password="+userPasswd; 
                        Class.forName("com.mysql.jdbc.Driver").newInstance(); 
                        Connection connection=DriverManager.getConnection(url); 
                        Statement statement = connection.createStatement(); 

                        for(int i=0;i<26;i++){
                            int aa=Integer.valueOf('a')+i;
                            char cha = (char) aa;
                            //out.print(aa);
                            out.print(cha);

                            //String sql="SELECT * FROM "+tableName+" where english like 'a%' "+"order by english"; 
                            String sql="SELECT * FROM "+tableName+" where english like '"+cha+"%' "+"order by english"; 
                            ResultSet rs = statement.executeQuery(sql);  
                            // 输出每一个数据值 
                            
                            String str;
                            int j=0;
                            while(rs.next()) { 
                                str=(rs.getString(2)).substring(0,1);
                                out.print(str+" "); 
                                j++;
                            }
                            
                            out.print(" "+j+" <br>"); 
                            rs.close();
                        }
                        
                        statement.close(); 
                        connection.close(); 
%>
</body> 
```
服务器中查看结果：
![image](https://user-images.githubusercontent.com/87808803/126662280-65eeaea6-b1fb-4411-938b-e018360d18dc.png)
