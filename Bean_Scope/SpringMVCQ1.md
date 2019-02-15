# SpringMVC中文乱码问题

## 页面编码
将charset和pageEncoding都改为UTF-8
```
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<meta http-equiv="Content-Type" content="text/html;charset=UTF-8"/>
```

## Get请求乱码
改tomcat中server.xml中Connector的port=“8080”，加上一个 URIEncoding=”utf-8”
```
<Connector port="8080" protocol="HTTP/1.1" URIEncoding="utf-8"
               connectionTimeout="20000"
               redirectPort="8443" />
<!-- A "Connector" using the shared thread pool-->
```

## POST请求乱码
修改web.xml文件，添加spring的编码过滤器
```
  <!-- 配置编码方式过滤器,注意一点:要配置在所有过滤器的前面 -->
  <filter>
    <filter-name>CharacterEncodingFilter</filter-name>
    <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
    <init-param>
      <param-name>encoding</param-name>
      <param-value>utf-8</param-value>
    </init-param>
  </filter>
  <filter-mapping>
    <filter-name>CharacterEncodingFilter</filter-name>
    <url-pattern>/*</url-pattern>
  </filter-mapping>
```

## 文件编码
将文件另存为UTF-8编码格式

## 数据库编码
连接字符串指定编码格式
```
public static String URL="jdbc:mysql://127.0.0.1:3306/mvcdb?useUnicode=true&characterEncoding=UTF-8"
```
创建数据库时指定UTF-8编码格式
