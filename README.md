*[慕课网：JAVA遇见HTML——JSP篇](http://www.imooc.com/learn/166)*

在慕课网学习这一内容，收集和记录的笔记~

---

##第一章：JSP基础语法

JSP页面元素构成：指令、表达式、小脚本、声明、注释、静态内容

####一、JSP指令

1. page指令：通常位置jsp页面顶端，同一个页面可以有多个
（1）语法格式：
<%@ page 属性1="属性值" 属性2="属性值1,属性值2"...属性n="属性n"(键值对)%>
（2）常用属性
language 指定JSP页面使用的脚本语言默认值为java


- import 通过该属性来引用脚本语言中使用的类文件默认值为无
- contentType 用来指定JSP页面所采用的编码方式默认值为text/html,ISO-885901
如：```<%@page language="java" import="java.util.*"contentType="text/html; charset=utf-8"%>```


2. include指令：
将外部文件嵌入当前文件，同时解析这个页面的JSP语句，是包含指令
3. taglib指令：
使用标签库定义新的自定义标签，在JSP页面中启用制定行为

####二、JSP注释

1、HTML的注释：```<!--html注释 --> ```客户端可见
2、JSP的注释：```<%-- jsp注释 --%> ```客户端不可见
3、JSP脚本注释：在JSP脚本里 ```<% //单行注释和 /**/多行注释 &> ```客户端不可见

####三、JSP脚本 
```<% java代码%>```

#####四、JSP声明
1、指在JSP页面中中定义变量或者方法
2、语法：```<%! java代码 %>```

####五、JSP表达式
1、指的是在JSP页面中执行的表达式
2、语法：```<%=表达式 %>//注意：表达式不以分号结束```

####六、JSP页面生命周期
1、生成字节码文件，执行jspInit()只在第一次请求时执行，重复请求仅只执行一次。生成的文件在work里，【每次修改会重新编译，生成新的字节码文件】
2、jspService()用于处理用户请求，对于每一个请求，JSP引擎会创建一个新的线程来处理该请求【JSP多线程：降低资源需求，提高系统并发量及响应时间，但注意同步问题】。
3、由于该Servlet常驻于内存里，所以响应速度非常快。
![](https://github.com/Agatha03/note/blob/master/images/jspgrammer.jpg)

##第二章：JSP内置对象
####一、内置对象简介
1. JSP内置对象是Web容器创建的一组对象，【不使用new关键字】就可以使用的内置对象。例如：out

2. JSP九大内置对象：
5个常用对象：out(输出) request(请求) response(响应) session(会话) application(应用)
4个不常用： page pageContext exception config

####二、Web程序的请求响应模式
用户发送请求（request）
服务器给用户响应（response）

####三、out对象

#####什么是缓冲区
1. 缓冲区：Buffer，所谓缓冲区就是内存的一块区域，用来保存临时数据
2. IO输出最原始的就是一个字节一个字节输出，效率很低。缓冲区可以先将多个字节读出来，再一次性的输出，提高效率

#####out对象
1. out对象是JspWriter类的实例，是向客户端（这里指浏览器）输出内容的常用对象。
2. 常用方法：
```void println()向客户端打印字符串
void clear()清除缓冲区的内容。如果在flush之后调用，会抛出异常
void clearBuffer() 也是清除缓冲区内容，但在flush调用之后不会抛出异常
void flush()将缓冲区内容输出到客户端
int getBufferSize()返回缓冲区的大小（字节数），如不设缓冲区则为0
int getRemaining()返回缓冲区还剩余多少可用
boolean isAutoFlush()返回缓冲区满时，是否自动清空缓冲区
void close()关闭输出流```

1、表单标签：
```<form name="loginForm"(表单名字) action="a.jsp"(表示表单提交给谁去处理) method="get"(或者是"post) /form> ```
get(在URL中可以知道相关信息如搜索关键词等)、 post

2、 使用Table设置表单
```<tr></tr>：一行
<td></td>:一行中的一项
如标签：<td><input type="text"name="username"/></td> //name可以实现验证功能
name中type对应的是类型，如text类型，password类型。```


#####get和post 
表单有两种提交方式：get与post
定义在```<form action="dologin.jsp"name="loginForm" method="提交方式***"></form> ```动作／名称等顺序无所谓。
1. get：以【明文】方式，通过URL提交数据，数据在URL中【可以看到】。提交数据最多不超过【2KB】。安全性较低，但效率比post方式高。适合提交数据量不大，且安全要求不高的数据：比如：搜索、查询等功能。
2. post：将用户提交的信息封装在HTML HEADER内。适合提交数据量大，安全性高的用户信息。如：注册、修改、上传等功能。

####四、request对象

#####request对象
1. request用于封装客户端的请求信息。
2. 它是HttpServletRequest类的实例。
3. request对象具有请求域，即完成客户端的请求之前，该对象一直有效。

#####常用方法(一)
```
String getParameter(Stringname) 返回name制定参数的参数值

String[]getParameterValues(String name) 返回包含参数name的所有值的数组

void setAttribute(String,Object);存储此请求中的属性

object getAttribute(Stringname) 返回指定属性的属性值

String getContentType() 得到请求体的MIME类型

String getProtovol() 返回请求用的协议类型和版本号

String getSeverName() 返回接受请求的服务器主机名```

#####解决url传中文参数出现乱码问题:

tomcat > conf> server.xml

<Connector port="8080"protocol="HTTP/1.1"

connectionTimeout="20000"

redirectPort="8443" 【URLEncoding="utf-8"】添加这一句，修改服务器编码方式/>

#####常用方法(二)
```
int getSeverPort()返回服务器接受此请求所用的端口号

StringgetCharacterEncording() 返回字符编码方式

void setCharacterEncording()设置请求的字符编码方式

int getContentLength()返回请求体的长度（以字节数）

String getRemoteAddr()返回发送此请求的客户端IP地址

String getRealPath(Stringpath) 返回一虚拟路径的真实路径

Stringrequest.getContextPath() 返回上下文路径```


####五、response对象

#####HttpServletResponse类的实例
```
PrintWritergetWriter（）也可以在网页上输出，写在out对象后面也能在out前面输出，要解决可以用out.flash（）
response.sendRedirect(Stringaddr) 请求重定向
response.setContentType(“text/html;charset=utf-8”); 设置响应的MIME类型```
 

####六、请求重定向与请求转发的区别

1. 请求重定向：
服务端responce.sendRedirect("xx.jsp")重定向。【客户端行为】：即客户端会访问两次，第一次访问后会立即跳转到第二个重定向页面上，【从本质上讲等于两次请求】，而前一次的请求封装的request对象不会保存，地址栏的URL地址会改变。
2. 请求转发：
服务端request.getRequestDispatcher("xx.jsp").forward(request,response)请求转发。forward(request,response)用于保存内置对象request和response。【服务器行为】：服务器会代替客户端去访问转发页面，【从本质是一次请求】，转发后请求对象会保存，地址栏的URL地址不会改变。

####七、session对象
#####什么是session
1. session表示客户端与服务器的一次会话
2. Web中的session指：用户在浏览某个网站时，从进入网站到浏览器关闭所经过的这段时间，也就是用户浏览网站所花费的时间。
3. 从上述定义中可以看到，session实际是一个【特定的时间概念】

#####session对象
（1）session是JSP的一个内置对象，是HttpSession类的实例。
（2）从客户打开浏览器并连接到服务器开始，到客户关闭浏览器窗口断开与服务器的连接，这一过程成为一个会话。
（3）当客户在同一个网站的不同页面之间进行切换并访问时，服务器是通过session来判断这些请求是否来自同一个客户。
（4）session一般有时间限制，长时间不操作可能会导致session失效。session失效后原session中保存的属性值会全部丢失。
（5）setMaxInactiveInterval(int i) 该方法可直接设定session的生存时间，超过该时间session会重新创建。（单位：秒）

#####session对象的常用方法
```
long getCreationTime() ：返回session的创建时间；

public String getId() ：返回session的唯一ID号（该ID在session生成时，由JSP引擎创建）

public Object setAttribute(String name,Objectvalue) ：按照键值对的方式在该session会话中保存一个属性（该属性是一个对象类型）

public Object getAttribute(String name) ：返回指定名称的属性的值（如果该名称的属性不存在，则返回null）

String[] getValueNames() ：返回一个（包含在此session中所有可用属性）的数组。
int getMaxInactiveInterval() ：返回一个时间，该时间表示当前session间隔多少时间之后会失效（单位：秒）```
 
######session的生命周期

1. 创建：当客户端第一次访问某个jsp或者servlet时候，服务器会为当前会话创建一个SessionId，每次客户端向服务器发送请求时，都会将此SessionId携带过去，服务端会对此SessionId进行校验
2. 活动：
①某次会话当中通过超链接打开的新页面属于同一次会话
②只要当前会话页面没有全部关闭，重新打开的浏览器窗口访问同一个项目资源时属于同一次会话
③除非本次会话的所有页面都关闭后在重新访问某个JSP或者servlet将会创建新的会话
注意：注意原有会话还存在，只是这个旧的SessionId仍然存在于服务端，只不过再也没有客户端会携带它然后交予服务端校验
3. 销毁：三种方式
①调用了session.invalidate()方法
②Session过期（超时）
③服务器重新启动

#####设置session超时的两种方式： 
tomcat默认session超时时间为30分钟
1. ```session.setMaxInactiveInterval(时间);//单位是秒```
2. 在web.xml配置
```<session-config>
<session-timeout>
10
<session-timeout>
<session-config> //单位是分钟```

####八、application对象
1. 实现了用户间数据的共享，可存放全局变量。（类似静态对象）
2. 开始于服务器启动，终止于服务器的关闭（生命周期）
3. 在用户的前后连接或不同用户之间的连接中，可以对application对象的同一属性进行操作
4. 在任何地方对application对象属性进行操作，都将影响到其他用户对此的访问
5. application对象是ServletContext类的实例
 
#####常用方法
```
publicvoid setAttribute(String name, Object value) 使用指定名称将对象绑定到此对话
publicObject getAttribute(String name) 返回与此会话中的指定名称绑定在一起的对象，如果没有对象绑定在该名称下，则返回null
EnumerationgetAttributeNames() 返回所有可用属性名的枚举
StringgetServerInfo() 返回JSP(SERVLET)引擎名及版本号```
 
####九、page对象
page对象就是指向当前JSP页面本身，有点像类中的this指针，它是java.lang.Object类的实例。

#####常用对象：
```
class getClass() 返回此Object的类
int hashCode() 返回此object的hash码
boolean equals(Object obj)判断此Object是否与指定的Object对象相等
void copy(Object obj)把此Object拷贝到指定的Object对象中
Object clone() 克隆此Object对象
String toString() 把此Object对象转换成String类的对象
void notify() 唤醒一个等待的线程
void notifyAll() 唤醒所有等待的线程
void wait(int timeout)使一个线程处于等待直到timeout结束或被唤醒
void wait() 使一个线程处于等待直到被唤醒```
 
####十、pageContext对象

```
pageContext对象提供了对JSP页面内所有的对象及名字空间的访问
pageContext对象可以访问到本页所在的session，也可以取本页面所在的application的某一属性值
pageContext对象相当于页面中所有功能的集大成者
pageContext对象本类名也叫pageContext```
![](https://github.com/Agatha03/note/blob/master/images/pagcontext.jpg)

####十一、Config对象
![](https://github.com/Agatha03/note/blob/master/images/config.jpg)

####十二、Exception对象
1. Exception对象：实际上是java.lang.Throwable的对象
2. 使用方法：
在可能会抛出异常的页面page指令里，设置errorPage="xxx.jsp"，表示出现异常将抛给xxx页面去处理
在xxx页面里，要使用Exception对象，需要把page指令里的isErrorPage属性设置为true。
3. 常用方法：getMessage()和toString()方法


##第三章：Javabean

#####什么是javabean

1. Javabeans就是符合某种特定规范Java类。使用Javabeans的好处是【解决代码的重复编写】，减少代码冗余，功能区分明确，提高代码的维护性。
2. 设计原则四点：公有类，属性私有，包含无参的公有构造方法，getter和setter方法封装属性

#####JSP动作元素
（1）什么是JSP动作元素
JSP动作元素，动作元素为请求处理阶段提供信息。
动作元素遵循XML元素的语法，有一个包含元素名的开始标签，可以有属性、可选的内容、与开始标签匹配的结束标签。
（2）JSP动作分类--五大类
![](https://github.com/Agatha03/note/blob/master/images/jspaction.jpg)

#####普通方式应用Javabean像使用普通java类一样
1、创建一个web project项目。
2、在src文件下，创建一个包，在包中创建一个类，满足设计原则即可
3、在index.jsp页面中通过import导入之前创建的类（import="包名.类名"）
4、通过使用new创建Javabean实例(创建对象)
5、使用set方法赋值
6、使用get方法取值

#####在jsp页面中如何使用javabeans
1. 像使用普通java类一样，创建javabean实例
2. 在jsp页面中通常使用jsp动作标签使用javabean

#####常用的三个动作
**useBeans动作**
```<jsp:useBeanid="标识符"class="java类"scope="使用范围">```
**setProperty动作**
- 在JSP中为javabean对象实例的属性赋值的方法
1. 普通的set方法
2. JSP的动作指令：```<jsp:setProperty></jsp:setProperty>```
1)根据表单自动匹配对象的所有属性
```<jsp:setPropertyproperty="*" name="myUser" ></jsp:setProperty>```
2）根据表单自动匹配指定的属性
```<jsp:setProperty property="username"name="myUser" ></jsp:setProperty>```
3）与表单无关，通过手工赋值给属性
```<jsp:setProperty property="username"name="myUser" value="lisi"></jsp:setProperty>```
4）通过URL传参数给属性赋值
```<jsp:setProperty property="username"name="myUser" param="myname">            </jsp:setProperty>```
**getProperty动作**
作用：获取指定Javabean对象的属性值。
```<jsp:getProperty name = "javabean实例名" property = "属性名" />```

#####scope四种作用域范围
1. application 全局范围(application.getAttrbytes("id".get....))
2. session 会话范围（用法同上替换关键字）
3. request 请求范围重定向无效转发有效用法同上
4. page 当前页面有效
 
#####Model1简介
Model 1 模型出现前，整个Web应用的情况：几乎全部由JSP页面组成，jsp页面接受处理客户端请求，对请求处理后做出相应
弊端：
在界面层充斥着大量的业务逻辑的代码和数据访问层的代码，Web程序的可扩展性和可维护性非常差。
javabean 的出现可以使jsp页面中使用javabean封装的数据或者调用javabean的业务逻辑代码，这样大大提升了程序的可维护性
**jsp 界面层 +javabean 业务逻辑层 =Model1 **

##第四章：JSP状态管理
####1、http协议无状态性（http协议：超文本传输协议）、
无状态是指，当浏览器发送请求给服务器的时候，服务器响应客户端请求。
但是当同一个浏览器再次发送请求给服务器的时候，服务器并不知道他就是刚才那个浏览器。
简单地说，就是服务器不会去记得你，所以就是无状态协议
保存用户状态的两大机制
####2、Cookie简介
由于http协议无状态性，所以为了保存用户状态，采用Session/cookie
cookie:中文名是“小甜饼”，是web服务器保存在客户端的一系列文本
*典型应用一：判断注册用户是否已经登录网站；
典型应用二：购物车的处理
典型应用三：系统会自动记录已经浏览过的视频。
典型应用四：记住用户名和密码实现自动登录功能。*
生活中的cookie：1）浏览记录；2）记住密码
保存用户状态的两大机制：**Session 和Cookie**。
#####Cookie的作用：
1. 保存用户对象的追踪；
2. 保存用户网页浏览记录与习惯；
3. 简化登录；
4. 容易泄露用户信息。（安全风险）

####3、Cookie 的创建与使用

1. cookies:是保存在客户端的一系列文本，
在客户端的保存形式是：Cookie对象的集合，以键值对的形式存放
2. Cookie.SetMaxAge(int expiry); 
expiry:为负值：表示当浏览器关闭时，删除该cookie
为0时：表示删除该cookie


#####Jsp中创建与使用Cookie：
1. 创建Cookie对象：
```Cookie cookie=new Cookie(String key,Object value);```
2. 写入Cookie：
```response.addCookie(cookie);```
3. 读取Cookie：
```Cookie[ ] cookies=request.getCookies( );```
4. Session 与 Cookie 的对比
*1. Session保存在服务器端，Cookie保存在客户端；*
*2. Session保存的是Object类型，Cookie保存的是String类型；*
*3. Session随会话的结束将其存储的数据销毁，Cookie可以长期保存在客户端；*
*4. Session保存重要信息，Cookie保存不重要信息。*
 
##第五章：JSP指令与动作元素
##### Include指令
语法：```<%@includefile="url" %>```
注：使用include标签，要删掉jsp页面生成的html无用的代码，否则会报错
定义时间类：
```Date xx=new Date();
simpleDateFormat XX=newsimpleDateFormat("yyyy-MM-dd");
String XXX=XX.format(xx);```

#####include动作：
```<jsp:include page="url"flush="false"/>```

#####include指令：
```<%@include file="url" %>```

*page 要包含的页面
flush 被包含的页面是否从缓冲区读取*

#####include指令和include动作比较
![](https://github.com/Agatha03/note/blob/master/images/ommandandaction.jpg)
#####Forward动作
```<jsp:forward page="URL"/>```

这个指令等同于```request.getRequestDispatcher().forward(request,response);```
 
#####param动作
1. param动作用法：```<jsp:param name="参数名" value="参数值">```
一般和forward动作连用，作为forward的子标签进行使用
2. 向另一个页面传递参数
可以传递新的参数+可以修改原有的参数


---
这里只是罗列了一些知识点，然而看这些知识点并没有什么用，因为很枯燥，所以，感兴趣的小伙伴可以去学习一下这门课~


[慕课网：JAVA遇见HTML——JSP篇](http://www.imooc.com/learn/166)