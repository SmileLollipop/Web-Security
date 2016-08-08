##SQL注入

　　所谓SQL注入式攻击，就是攻击者把SQL命令插入到Web表单的输入域或页面请求的查询字符串，欺骗服务器执行恶意的SQL命令。 
　　攻击者通过在应用程序预先定义好的SQL语句结尾加上额外的SQL语句元素，欺骗数据库服务器执行非授权的查询,篡改命令。

　　它能够轻易的绕过防火墙直接访问数据库，甚至能够获得数据库所在的服务器的系统权限。在Web应用漏洞中，SQL Injection 漏洞的风险要高过其他所有的漏洞。

　　攻击原理

 

复制代码
　　假设的登录查询

　　SELECT * FROM  users  WHERE login = 'victor' AND password = '123

　　Sever端代码

　　String sql = "SELECT * FROM users WHERE login = '" + formusr + "' AND password = '" + formpwd + "'";

　　输入字符

　　formusr = ' or 1=1

　　formpwd = anything

　　实际的查询代码

　　SELECT * FROM users WHERE username = ' ' or 1=1  AND password = 'anything'  
复制代码
 

　　发现注入点简单办法

　　1.寻找带有查询字符串的url的网页(例如，查询那些在URL里带有"id=" 的URL)。

　　2.向这个网站发送一个请求，改变其中的id=语句，带一个额外的单引号（例如：id=123’）。

　　3.查看返回的内容，在其中查找“sql”，“statement”等关键字（这也说明返回了具体的错误信息，这本身就很糟糕）。如下图：



　　4.错误消息是否表示发送到SQL服务器的参数没有被正确编码果如此，那么表示可对该网站进行SQL注入攻击。

　　如何防范SQL注入攻击

　　一个常见的错误是，假如你使用了存储过程或ORM，你就完全不受SQL注入攻击之害了。这是不正确的，你还是需要确定在给存储过程传递数据时你很谨慎，或在用ORM来定制一个查询时，你的做法是安全的。

　　参数化查询已被视为最有效的可防御SQL注入攻击的防御方式。目前主流的ORM 框架都内置支持并且推荐使用这种方式进行持久层封装。

　　所谓的参数化查询（Parameterized Query 或 Parameterized Statement）是指在设计与数据库链接并访问数据时，在需要填入数值或数据的地方，使用参数 (Parameter) 来给值。

　　　　例：      
　　　　SELECT * FROM myTable WHERE myID = @myID

　　　　INSERT INTO myTable (c1, c2, c3, c4) VALUES (@c1, @c2, @c3, @c4)或者INSERT INTO myTable (c1, c2, c3, c4) VALUES(?,?,?,?)
　　通过(?)指定占位符，当然在添加参数的时候，必须按照(c1, c2, c3, c4)的顺序来添加，否则会出错。
