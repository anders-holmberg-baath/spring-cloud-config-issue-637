# spring-cloud-config-issue-637
Simple Spring Boot Initializr setup of Spring Cloud Config Server to demonstrate issue [Colon in profile name yields obscure IllegalArgumentException](https://github.com/spring-cloud/spring-cloud-config/issues/637)

## Reproduce issue:

1. `mvn package`
2. `java -jar target/config-server-0.0.1-SNAPSHOT.jar`
3. `curl -vN 'http://localhost:8080/application/test:abc'`

## Response:
```
> GET /application/test:abc HTTP/1.1
> Host: localhost:8080
> User-Agent: curl/7.43.0
> Accept: */*
>
< HTTP/1.1 400
< X-Application-Context: application
< Content-Type: application/json;charset=UTF-8
< Transfer-Encoding: chunked
< Date: Tue, 14 Feb 2017 07:34:29 GMT
< Connection: close
<
{"timestamp":1487057669392,"status":400,"error":"Bad Request","exception":"java.lang.IllegalArgumentException","message":"name","path":"/application/test:abc"}
```

## Stack trace
```
java.lang.IllegalArgumentException: name
        at sun.misc.URLClassPath$Loader.findResource(URLClassPath.java:658) ~[na:1.8.0_111]
        at sun.misc.URLClassPath.findResource(URLClassPath.java:188) ~[na:1.8.0_111]
        at java.net.URLClassLoader$2.run(URLClassLoader.java:569) ~[na:1.8.0_111]
        at java.net.URLClassLoader$2.run(URLClassLoader.java:567) ~[na:1.8.0_111]
        at java.security.AccessController.doPrivileged(Native Method) ~[na:1.8.0_111]
        at java.net.URLClassLoader.findResource(URLClassLoader.java:566) ~[na:1.8.0_111]
        at org.springframework.boot.loader.LaunchedURLClassLoader.findResource(LaunchedURLClassLoader.java:58) ~[config-server-0.0.1-SNAPSHOT.jar:0.0.1-SNAPSHOT]
        at java.lang.ClassLoader.getResource(ClassLoader.java:1096) ~[na:1.8.0_111]
        at org.apache.catalina.loader.WebappClassLoaderBase.getResource(WebappClassLoaderBase.java:996) ~[tomcat-embed-core-8.5.11.jar!/:8.5.11]
        at org.springframework.core.io.ClassPathResource.resolveURL(ClassPathResource.java:147) ~[spring-core-4.3.6.RELEASE.jar!/:4.3.6.RELEASE]
        at org.springframework.core.io.ClassPathResource.exists(ClassPathResource.java:135) ~[spring-core-4.3.6.RELEASE.jar!/:4.3.6.RELEASE]
        at org.springframework.boot.context.config.ConfigFileApplicationListener$Loader.loadIntoGroup(ConfigFileApplicationListener.java:469) ~[spring-boot-1.5.1.RELEASE.jar!/:1.5.1.RELEASE]
        at org.springframework.boot.context.config.ConfigFileApplicationListener$Loader.load(ConfigFileApplicationListener.java:444) ~[spring-boot-1.5.1.RELEASE.jar!/:1.5.1.RELEASE]
        at org.springframework.boot.context.config.ConfigFileApplicationListener$Loader.load(ConfigFileApplicationListener.java:380) ~[spring-boot-1.5.1.RELEASE.jar!/:1.5.1.RELEASE]
        at org.springframework.boot.context.config.ConfigFileApplicationListener.addPropertySources(ConfigFileApplicationListener.java:215) ~[spring-boot-1.5.1.RELEASE.jar!/:1.5.1.RELEASE]
        at org.springframework.boot.context.config.ConfigFileApplicationListener.postProcessEnvironment(ConfigFileApplicationListener.java:184) ~[spring-boot-1.5.1.RELEASE.jar!/:1.5.1.RELEASE]
        at org.springframework.boot.context.config.ConfigFileApplicationListener.onApplicationEnvironmentPreparedEvent(ConfigFileApplicationListener.java:171) ~[spring-boot-1.5.1.RELEASE.jar!/:1.5.1.RELEASE]
        at org.springframework.boot.context.config.ConfigFileApplicationListener.onApplicationEvent(ConfigFileApplicationListener.java:157) ~[spring-boot-1.5.1.RELEASE.jar!/:1.5.1.RELEASE]
        at org.springframework.context.event.SimpleApplicationEventMulticaster.invokeListener(SimpleApplicationEventMulticaster.java:167) ~[spring-context-4.3.6.RELEASE.jar!/:4.3.6.RELEASE]
        at org.springframework.context.event.SimpleApplicationEventMulticaster.multicastEvent(SimpleApplicationEventMulticaster.java:139) ~[spring-context-4.3.6.RELEASE.jar!/:4.3.6.RELEASE]
        at org.springframework.context.event.SimpleApplicationEventMulticaster.multicastEvent(SimpleApplicationEventMulticaster.java:122) ~[spring-context-4.3.6.RELEASE.jar!/:4.3.6.RELEASE]
        at org.springframework.boot.context.event.EventPublishingRunListener.environmentPrepared(EventPublishingRunListener.java:73) ~[spring-boot-1.5.1.RELEASE.jar!/:1.5.1.RELEASE]
        at org.springframework.boot.SpringApplicationRunListeners.environmentPrepared(SpringApplicationRunListeners.java:54) ~[spring-boot-1.5.1.RELEASE.jar!/:1.5.1.RELEASE]
        at org.springframework.boot.SpringApplication.prepareEnvironment(SpringApplication.java:336) ~[spring-boot-1.5.1.RELEASE.jar!/:1.5.1.RELEASE]
        at org.springframework.boot.SpringApplication.run(SpringApplication.java:307) ~[spring-boot-1.5.1.RELEASE.jar!/:1.5.1.RELEASE]
        at org.springframework.boot.builder.SpringApplicationBuilder.run(SpringApplicationBuilder.java:134) [spring-boot-1.5.1.RELEASE.jar!/:1.5.1.RELEASE]
        at org.springframework.cloud.config.server.environment.NativeEnvironmentRepository.findOne(NativeEnvironmentRepository.java:112) [spring-cloud-config-server-1.3.0.BUILD-SNAPSHOT.jar!/:1.3.0.BUILD-SNAPSHOT]
        at org.springframework.cloud.config.server.environment.AbstractScmEnvironmentRepository.findOne(AbstractScmEnvironmentRepository.java:44) [spring-cloud-config-server-1.3.0.BUILD-SNAPSHOT.jar!/:1.3.0.BUILD-SNAPSHOT]
        at org.springframework.cloud.config.server.environment.MultipleJGitEnvironmentRepository.findOne(MultipleJGitEnvironmentRepository.java:170) [spring-cloud-config-server-1.3.0.BUILD-SNAPSHOT.jar!/:1.3.0.BUILD-SNAPSHOT]
        at org.springframework.cloud.config.server.environment.CompositeEnvironmentRepository.findOne(CompositeEnvironmentRepository.java:45) [spring-cloud-config-server-1.3.0.BUILD-SNAPSHOT.jar!/:1.3.0.BUILD-SNAPSHOT]
        at org.springframework.cloud.config.server.environment.EnvironmentEncryptorEnvironmentRepository.findOne(EnvironmentEncryptorEnvironmentRepository.java:53) [spring-cloud-config-server-1.3.0.BUILD-SNAPSHOT.jar!/:1.3.0.BUILD-SNAPSHOT]
        at org.springframework.cloud.config.server.environment.EnvironmentController.labelled(EnvironmentController.java:112) [spring-cloud-config-server-1.3.0.BUILD-SNAPSHOT.jar!/:1.3.0.BUILD-SNAPSHOT]
        at org.springframework.cloud.config.server.environment.EnvironmentController.defaultLabel(EnvironmentController.java:101) [spring-cloud-config-server-1.3.0.BUILD-SNAPSHOT.jar!/:1.3.0.BUILD-SNAPSHOT]
        at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method) ~[na:1.8.0_111]
        at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62) ~[na:1.8.0_111]
        at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43) ~[na:1.8.0_111]
        at java.lang.reflect.Method.invoke(Method.java:498) ~[na:1.8.0_111]
        at org.springframework.web.method.support.InvocableHandlerMethod.doInvoke(InvocableHandlerMethod.java:205) [spring-web-4.3.6.RELEASE.jar!/:4.3.6.RELEASE]
        at org.springframework.web.method.support.InvocableHandlerMethod.invokeForRequest(InvocableHandlerMethod.java:133) [spring-web-4.3.6.RELEASE.jar!/:4.3.6.RELEASE]
        at org.springframework.web.servlet.mvc.method.annotation.ServletInvocableHandlerMethod.invokeAndHandle(ServletInvocableHandlerMethod.java:116) [spring-webmvc-4.3.6.RELEASE.jar!/:4.3.6.RELEASE]
        at org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter.invokeHandlerMethod(RequestMappingHandlerAdapter.java:827) [spring-webmvc-4.3.6.RELEASE.jar!/:4.3.6.RELEASE]
        at org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter.handleInternal(RequestMappingHandlerAdapter.java:738) [spring-webmvc-4.3.6.RELEASE.jar!/:4.3.6.RELEASE]
        at org.springframework.web.servlet.mvc.method.AbstractHandlerMethodAdapter.handle(AbstractHandlerMethodAdapter.java:85) [spring-webmvc-4.3.6.RELEASE.jar!/:4.3.6.RELEASE]
        at org.springframework.web.servlet.DispatcherServlet.doDispatch(DispatcherServlet.java:963) [spring-webmvc-4.3.6.RELEASE.jar!/:4.3.6.RELEASE]
        at org.springframework.web.servlet.DispatcherServlet.doService(DispatcherServlet.java:897) [spring-webmvc-4.3.6.RELEASE.jar!/:4.3.6.RELEASE]
        at org.springframework.web.servlet.FrameworkServlet.processRequest(FrameworkServlet.java:970) [spring-webmvc-4.3.6.RELEASE.jar!/:4.3.6.RELEASE]
        at org.springframework.web.servlet.FrameworkServlet.doGet(FrameworkServlet.java:861) [spring-webmvc-4.3.6.RELEASE.jar!/:4.3.6.RELEASE]
        at javax.servlet.http.HttpServlet.service(HttpServlet.java:622) [tomcat-embed-core-8.5.11.jar!/:8.5.11]
        at org.springframework.web.servlet.FrameworkServlet.service(FrameworkServlet.java:846) [spring-webmvc-4.3.6.RELEASE.jar!/:4.3.6.RELEASE]
        at javax.servlet.http.HttpServlet.service(HttpServlet.java:729) [tomcat-embed-core-8.5.11.jar!/:8.5.11]
        at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:230) [tomcat-embed-core-8.5.11.jar!/:8.5.11]
        at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:165) [tomcat-embed-core-8.5.11.jar!/:8.5.11]
        at org.apache.tomcat.websocket.server.WsFilter.doFilter(WsFilter.java:52) [tomcat-embed-websocket-8.5.11.jar!/:8.5.11]
        at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:192) [tomcat-embed-core-8.5.11.jar!/:8.5.11]
        at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:165) [tomcat-embed-core-8.5.11.jar!/:8.5.11]
        at org.springframework.boot.web.filter.ApplicationContextHeaderFilter.doFilterInternal(ApplicationContextHeaderFilter.java:55) [spring-boot-1.5.1.RELEASE.jar!/:1.5.1.RELEASE]
        at org.springframework.web.filter.OncePerRequestFilter.doFilter(OncePerRequestFilter.java:107) [spring-web-4.3.6.RELEASE.jar!/:4.3.6.RELEASE]
        at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:192) [tomcat-embed-core-8.5.11.jar!/:8.5.11]
        at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:165) [tomcat-embed-core-8.5.11.jar!/:8.5.11]
        at org.springframework.boot.actuate.trace.WebRequestTraceFilter.doFilterInternal(WebRequestTraceFilter.java:108) [spring-boot-actuator-1.5.1.RELEASE.jar!/:1.5.1.RELEASE]
        at org.springframework.web.filter.OncePerRequestFilter.doFilter(OncePerRequestFilter.java:107) [spring-web-4.3.6.RELEASE.jar!/:4.3.6.RELEASE]
        at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:192) [tomcat-embed-core-8.5.11.jar!/:8.5.11]
        at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:165) [tomcat-embed-core-8.5.11.jar!/:8.5.11]
        at org.springframework.web.filter.RequestContextFilter.doFilterInternal(RequestContextFilter.java:99) [spring-web-4.3.6.RELEASE.jar!/:4.3.6.RELEASE]
        at org.springframework.web.filter.OncePerRequestFilter.doFilter(OncePerRequestFilter.java:107) [spring-web-4.3.6.RELEASE.jar!/:4.3.6.RELEASE]
        at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:192) [tomcat-embed-core-8.5.11.jar!/:8.5.11]
        at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:165) [tomcat-embed-core-8.5.11.jar!/:8.5.11]
        at org.springframework.web.filter.HttpPutFormContentFilter.doFilterInternal(HttpPutFormContentFilter.java:105) [spring-web-4.3.6.RELEASE.jar!/:4.3.6.RELEASE]
        at org.springframework.web.filter.OncePerRequestFilter.doFilter(OncePerRequestFilter.java:107) [spring-web-4.3.6.RELEASE.jar!/:4.3.6.RELEASE]
        at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:192) [tomcat-embed-core-8.5.11.jar!/:8.5.11]
        at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:165) [tomcat-embed-core-8.5.11.jar!/:8.5.11]
        at org.springframework.web.filter.HiddenHttpMethodFilter.doFilterInternal(HiddenHttpMethodFilter.java:81) [spring-web-4.3.6.RELEASE.jar!/:4.3.6.RELEASE]
        at org.springframework.web.filter.OncePerRequestFilter.doFilter(OncePerRequestFilter.java:107) [spring-web-4.3.6.RELEASE.jar!/:4.3.6.RELEASE]
        at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:192) [tomcat-embed-core-8.5.11.jar!/:8.5.11]
        at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:165) [tomcat-embed-core-8.5.11.jar!/:8.5.11]
        at org.springframework.web.filter.CharacterEncodingFilter.doFilterInternal(CharacterEncodingFilter.java:197) [spring-web-4.3.6.RELEASE.jar!/:4.3.6.RELEASE]
        at org.springframework.web.filter.OncePerRequestFilter.doFilter(OncePerRequestFilter.java:107) [spring-web-4.3.6.RELEASE.jar!/:4.3.6.RELEASE]
        at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:192) [tomcat-embed-core-8.5.11.jar!/:8.5.11]
        at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:165) [tomcat-embed-core-8.5.11.jar!/:8.5.11]
        at org.springframework.boot.actuate.autoconfigure.MetricsFilter.doFilterInternal(MetricsFilter.java:106) [spring-boot-actuator-1.5.1.RELEASE.jar!/:1.5.1.RELEASE]
        at org.springframework.web.filter.OncePerRequestFilter.doFilter(OncePerRequestFilter.java:107) [spring-web-4.3.6.RELEASE.jar!/:4.3.6.RELEASE]
        at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:192) [tomcat-embed-core-8.5.11.jar!/:8.5.11]
        at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:165) [tomcat-embed-core-8.5.11.jar!/:8.5.11]
        at org.apache.catalina.core.StandardWrapperValve.invoke(StandardWrapperValve.java:198) [tomcat-embed-core-8.5.11.jar!/:8.5.11]
        at org.apache.catalina.core.StandardContextValve.invoke(StandardContextValve.java:96) [tomcat-embed-core-8.5.11.jar!/:8.5.11]
        at org.apache.catalina.authenticator.AuthenticatorBase.invoke(AuthenticatorBase.java:474) [tomcat-embed-core-8.5.11.jar!/:8.5.11]
        at org.apache.catalina.core.StandardHostValve.invoke(StandardHostValve.java:140) [tomcat-embed-core-8.5.11.jar!/:8.5.11]
        at org.apache.catalina.valves.ErrorReportValve.invoke(ErrorReportValve.java:79) [tomcat-embed-core-8.5.11.jar!/:8.5.11]
        at org.apache.catalina.core.StandardEngineValve.invoke(StandardEngineValve.java:87) [tomcat-embed-core-8.5.11.jar!/:8.5.11]
        at org.apache.catalina.connector.CoyoteAdapter.service(CoyoteAdapter.java:349) [tomcat-embed-core-8.5.11.jar!/:8.5.11]
        at org.apache.coyote.http11.Http11Processor.service(Http11Processor.java:783) [tomcat-embed-core-8.5.11.jar!/:8.5.11]
        at org.apache.coyote.AbstractProcessorLight.process(AbstractProcessorLight.java:66) [tomcat-embed-core-8.5.11.jar!/:8.5.11]
        at org.apache.coyote.AbstractProtocol$ConnectionHandler.process(AbstractProtocol.java:798) [tomcat-embed-core-8.5.11.jar!/:8.5.11]
        at org.apache.tomcat.util.net.NioEndpoint$SocketProcessor.doRun(NioEndpoint.java:1434) [tomcat-embed-core-8.5.11.jar!/:8.5.11]
        at org.apache.tomcat.util.net.SocketProcessorBase.run(SocketProcessorBase.java:49) [tomcat-embed-core-8.5.11.jar!/:8.5.11]
        at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1142) [na:1.8.0_111]
        at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:617) [na:1.8.0_111]
        at org.apache.tomcat.util.threads.TaskThread$WrappingRunnable.run(TaskThread.java:61) [tomcat-embed-core-8.5.11.jar!/:8.5.11]
        at java.lang.Thread.run(Thread.java:745) [na:1.8.0_111]
```

The issue does not occur if the application is started with `mvn spring-boot:run` instead, or if for instance `_` is used instead of `:` in the profile name.
