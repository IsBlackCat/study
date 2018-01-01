## 前期准备 ##
安装jdk，tomcat和idea。这些网上都有教程，不进行赘述。之后用idea创建一个工程。点击java enterprise，选择相应的sdk，application Server，并勾选Struts2，并在下面勾选set up library later。之后点next，并命名为testStruts2。（名字随意）

![这里写图片描述](http://img.blog.csdn.net/20180101101201555?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd2VpeGluXzM4OTk3MzEx/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)


## 导入相应的struts2的jar包 ##
1，在idea的project的web目录下创建WEB-INF目录，再在WEB-INF目录下创建lib目录。
2，下载jar包[点击此处下载jar包](https://struts.apache.org/download.cgi#struts2514.1)，我下载的是Struts 2.3.34的Full Distribution。解压后找到lib目录。找到以下的jar包，并将其复制到工程的lib目录下。在struts 2.2.3版本前只需要导入5个包就可以了，而之后的版本则需要导入11个包。

![这里写图片描述](http://img.blog.csdn.net/20180101102504982?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd2VpeGluXzM4OTk3MzEx/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

3，在工程的lib目录下选中所有jar包并右击选择Add As Library。

## 配置web.xml文件 ##
在WEB-INF目录下创建web.xml文件，并编辑为以下内容。从>=2.1.3的版本开始，FilterDispatcher被标记为过时。如果你的struts版本大于2.1.3时，filter配置要变成：
org.apache.struts2.dispatcher.ng.filter.StrutsPrepareAndExecuteFilter。

```
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xmlns="http://java.sun.com/xml/ns/javaee"
         xmlns:web="http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd"
         xsi:schemaLocation="http://java.sun.com/xml/ns/javaee
   http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd"
         id="WebApp_ID" version="3.0">
    <display-name>Struts 2</display-name>
    <welcome-file-list>
        <welcome-file>index.jsp</welcome-file>
    </welcome-file-list>
    <filter>
        <filter-name>struts2</filter-name>
        <filter-class>
            org.apache.struts2.dispatcher.ng.filter.StrutsPrepareAndExecuteFilter
        </filter-class>
    </filter>
    <filter-mapping>
        <filter-name>struts2</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
</web-app>
```
## 创建一个DEMO ##
1,创建action类

```
package com;

public class HelloWorldAction {
    private String name;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String execute() throws Exception{
        if("admin".equals(name))
            return "success";
        else
            return "fail";
    }
}

```
2 创建相应的视图
在web目录下创建success.jsp和fail.jsp
```
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%@ taglib prefix="s" uri="/struts-tags" %>
<html>
<head>
    <title>success</title>
</head>
<body>
    <h1>success</h1>
    hello,<s:property value="name"/>
</body>
</html>
```

```
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>fail</title>
</head>
<body>
    <h1>fail</h1>
</body>
</html>
```

3,创建主页index.jsp
在web目录下创建index.jsp,应该是工程自动创建好了,修改一下就好。

```
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
  <head>
    <title>index</title>
  </head>
  <body>
    Please input the string:
    <form action="hello">
      <input name="name" type="text"/>
      <br/>
      <input type="submit" value="提交"/>
    </form>
  </body>
</html>
```

4，配置struts.xml
在project中点击Project Structure，之后选中Facets将之前导入的jar包里面的struts-default.xml放入跟工程中struts.xml同一个Fileset。再将struts.xml的内容修改为以下：

```
<?xml version="1.0" encoding="UTF-8"?>

<!DOCTYPE struts PUBLIC
        "-//Apache Software Foundation//DTD Struts Configuration 2.0//EN"
        "http://struts.apache.org/dtds/struts-2.0.dtd">

<struts>
    <package name="helloworld" extends="struts-default">
        <action name="hello"
                class="com.HelloWorldAction"
                method="execute">
            <result name="success">/success.jsp</result>
            <result name="fail">/fail.jsp</result>
        </action>
    </package>
</struts>
```

最后运行index.jsp就好了。以上