---
layout: post
title: Java Web学习笔记
---

= Servlet
* 一个servlet就是一个Java类，并提供基于请求-响应模式的Web服务
* Servlet容器是一个服务端程序负责装载和管理Servlet
* Servlet的生命周期：
** Init方法: 在第一次启动Servlet时或Servlet容器启动时（load-on-start-up配置）被调用
** Service以及HTTP的方法：Servlet响应客户端的请求时调用
** destroy方法：Servlet被销毁（停止Servlet所在的Web应用）之前调用

== 配置信息
=== ServletConfig
```XML
<init-param>
    <param-name>data_1</param-name>
    <param-value>value_1</param-value>
</init-param>
```

```Java
ServletConfig config = this.getServletConfig();
String v1 = config.getInitParameter("data_1");
```
** Servlet初始化过程中，`<init-param>`参数将会被封装到ServletConfig
** 每个Servlet支持设置一个或者多个`<init-param>`
** 它不是全局共享

=== ServletContext
* Servlet容器启动时会为每个Web应用初始化一个ServletContext，它在该Web应用中全局唯一
```XML
<context-param>
    <param-name>global_1</param-name>
    <param-value>111</param-value>
</context-param>
```

```Java
ServletContext ctx = this.getServletContext();
String gv1 = ctx.getInitParameter("global_1");
```

* Servlet之间共享信息
```Java
// In Servlet A
ctx.setAttribute("attr_1", "121212");

// In Servlet B
String at1 = (String) ctx.getAttribute("attr_1");
```

* 读取外部资源配置文件信息的方法`getResource`, `getResourceAsStream`, `getRealPath`
* 配置文件放在src/main/resources文件夹里
```Java
try {
    URL url = ctx.getResource("/WEB-INF/classes/log4j.properties");
    InputStream in = url.openStream();
    String v1 = GeneralUtil.getProperty("log4j.rootLogger", in);
} catch (MalformedURLException e) {
    e.printStackTrace();
} catch (IOException e) {
    e.printStackTrace();
}

// getResourceAsStream 方法
URL in2 = ctx.getResourceAsStream("/WEB-INF/classes/log4j.properties");
String v2 = GeneralUtil.getProperty("log4j.rootLogger", in2);

// getRealPath 方法
String path = ctx.getRealPath("/WEB-INF/classes/log4j.properties");
File f = new File(path);
try {
    InputStream in3 = new FileInputStream(f);
    String v3 = GeneralUtil.getProperty("log4j.rootLogger", in3);
} catch (FileNotFoundException e) {
    e.printStackTrace();
} catch (IOException e) {
    e.printStackTrace();
}
```

* ServletContext还可以实现Servlet转发

== 部署描述符web.xml
* 部署描述符包括`servlet`, `servlet-mapping`, `init-param`（是servlet的子元素）, `context-param`
* url-pattern支持模糊匹配
* ServletMapping匹配规则
1. 精确路径匹配，完全匹配
2. 最长路径，最长前缀匹配
3. 扩展名匹配
4. default servlet 匹配或放弃匹配并报错

=== load-on-startup
* 如果需要Servlet容器启动时执行操作，需要在`sevlet`下配置`load-on-startup`，
* 如果没有设置或设置成负数则表示只有处理请求时容器才加载该Servlet
* 数值越小，启动优先级越高

=== 其它

```XML
<error-page>
    <error-code>404</error-code>
    <location>/404.html</location>
</error-page>
```
* 还可以添加exception-type子元素

* `welcome-file-list`可包含多个`welcome-file`，顺序加载
* `mime-mapping`包括`extension`和`mime-type`子元素

