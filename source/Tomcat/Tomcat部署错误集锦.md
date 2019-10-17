## Tomcat部署错误集锦

```shell
Caused by: java.lang.IllegalArgumentException: More than one fragment with the name [spring_web] was found. This is not legal with relative ordering. See section 8.2.2 2c of the Servlet specification for details. Consider using absolute ordering.
    at org.apache.tomcat.util.descriptor.web.WebXml.orderWebFragments(WebXml.java:2200)
    at org.apache.tomcat.util.descriptor.web.WebXml.orderWebFragments(WebXml.java:2159)
    at org.apache.catalina.startup.ContextConfig.webConfig(ContextConfig.java:1124)
    at org.apache.catalina.startup.ContextConfig.configureStart(ContextConfig.java:769)
    at org.apache.catalina.startup.ContextConfig.lifecycleEvent(ContextConfig.java:299)
    at org.apache.catalina.util.LifecycleBase.fireLifecycleEvent(LifecycleBase.java:94)
    at org.apache.catalina.core.StandardContext.startInternal(StandardContext.java:5134)
    at org.apache.catalina.util.LifecycleBase.start(LifecycleBase.java:150)
```

解决方案:在web.xml加一个标签：`<absolute-ordering />`

```xml
<welcome-file-list>
	<welcome-file>/WEB-INF/view/index.jsp</welcome-file>
</welcome-file-list>
<!--具体位置-->
<absolute-ordering />
```

