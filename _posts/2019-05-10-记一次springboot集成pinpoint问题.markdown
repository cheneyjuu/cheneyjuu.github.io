---
layout: post
title:  "记一次springboot集成pinpoint问题"
date:   2019-05-10
---

### 问题描述
线上springboot使用内嵌Tomcat，使用jar包的方式启动，在集成pinpoint时，出现了以下错误：

```shell
Buffering:  2019-05-09 22:37:43.901 INFO  [main] Bean 'org.springframework.cloud.sleuth.instrument.async.AsyncDefaultAutoConfiguration' of type [org.springframework.cloud.sleuth.instrument.async.AsyncDefaultAutoConfiguration$$EnhancerBySpringCGLIB$$e7780e1a] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
Buffering:  2019-05-09 22:37:45 [INFO ](com.navercorp.pinpoint.bootstrap.config.DefaultProfilerConfig) profiler.tomcat.enable=true
Buffering:  2019-05-09 22:37:45 [INFO ](com.navercorp.pinpoint.bootstrap.config.DefaultProfilerConfig) profiler.tomcat.conditional.transform=true
Buffering:  2019-05-09 22:37:45 [INFO ](com.navercorp.pinpoint.bootstrap.config.DefaultProfilerConfig) profiler.tomcat.hidepinpointheader=true
Buffering:  2019-05-09 22:37:45 [INFO ](com.navercorp.pinpoint.bootstrap.config.DefaultProfilerConfig) profiler.tomcat.tracerequestparam=true
Buffering:  2019-05-09 22:37:45 [INFO ](com.navercorp.pinpoint.bootstrap.config.DefaultProfilerConfig) profiler.tomcat.excludeurl=/aa/test.html, /bb/exclude.html
Buffering:  2019-05-09 22:37:45 [INFO ](com.navercorp.pinpoint.bootstrap.config.DefaultProfilerConfig) profiler.tomcat.realipheader=null
Buffering:  2019-05-09 22:37:45 [INFO ](com.navercorp.pinpoint.bootstrap.config.DefaultProfilerConfig) profiler.tomcat.realipemptyvalue=null
Buffering:  2019-05-09 22:37:45 [INFO ](com.navercorp.pinpoint.bootstrap.config.DefaultProfilerConfig) profiler.tomcat.excludemethod=
Buffering:  2019-05-09 22:37:45.378 INFO  [main] Tomcat initialized with port(s): 0 (http)
Buffering:  2019-05-09 22:37:45.421 INFO  [main] Initializing ProtocolHandler ["http-nio-auto-1"]
Buffering:  2019-05-09 22:37:45.482 INFO  [main] Starting service [Tomcat]
Buffering:  2019-05-09 22:37:45.483 INFO  [main] Starting Servlet Engine: Apache Tomcat/8.5.35
Buffering:  2019-05-09 22:37:45.538 INFO  [main] Pausing ProtocolHandler ["http-nio-auto-1"]
Buffering:  2019-05-09 22:37:45.539 INFO  [main] Stopping service [Tomcat]
Buffering:  2019-05-09 22:37:45.551 WARN  [main] Exception encountered during context initialization - cancelling refresh attempt: org.springframework.context.ApplicationContextException: Unable to start embedded container; nested exception is org.springframework.boot.context.embedded.EmbeddedServletContainerException: Unable to start embedded Tomcat
Buffering:  2019-05-09 22:37:45.569 INFO  [main] 
Buffering:  
Buffering:  Error starting ApplicationContext. To display the auto-configuration report re-run your application with 'debug' enabled.
Buffering:  2019-05-09 22:37:45.588 ERROR [main] Application startup failed
Buffering:  org.springframework.context.ApplicationContextException: Unable to start embedded container; nested exception is org.springframework.boot.context.embedded.EmbeddedServletContainerException: Unable to start embedded Tomcat
Buffering:  	at org.springframework.boot.context.embedded.EmbeddedWebApplicationContext.onRefresh(EmbeddedWebApplicationContext.java:139)
Buffering:  	at org.springframework.context.support.AbstractApplicationContext.refresh(AbstractApplicationContext.java:537)
Buffering:  	at org.springframework.boot.context.embedded.EmbeddedWebApplicationContext.refresh(EmbeddedWebApplicationContext.java:124)
Buffering:  	at org.springframework.boot.SpringApplication.refresh(SpringApplication.java:693)
Buffering:  	at org.springframework.boot.SpringApplication.refreshContext(SpringApplication.java:360)
Buffering:  	at org.springframework.boot.SpringApplication.run(SpringApplication.java:303)
Buffering:  	at org.springframework.boot.SpringApplication.run(SpringApplication.java:1118)
Buffering:  	at org.springframework.boot.SpringApplication.run(SpringApplication.java:1107)
Buffering:  	at geex.configCenter.ConfigCenterApplication.main(ConfigCenterApplication.java:15)
Buffering:  	at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
Buffering:  	at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62)
Buffering:  	at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
Buffering:  	at java.lang.reflect.Method.invoke(Method.java:498)
Buffering:  	at org.springframework.boot.loader.MainMethodRunner.run(MainMethodRunner.java:54)
Buffering:  	at java.lang.Thread.run(Thread.java:748)
Buffering:  Caused by: org.springframework.boot.context.embedded.EmbeddedServletContainerException: Unable to start embedded Tomcat
Buffering:  	at org.springframework.boot.context.embedded.tomcat.TomcatEmbeddedServletContainer.initialize(TomcatEmbeddedServletContainer.java:138)
Buffering:  	at org.springframework.boot.context.embedded.tomcat.TomcatEmbeddedServletContainer.<init>(TomcatEmbeddedServletContainer.java:87)
Buffering:  	at org.springframework.boot.context.embedded.tomcat.TomcatEmbeddedServletContainerFactory.getTomcatEmbeddedServletContainer(TomcatEmbeddedServletContainerFactory.java:554)
Buffering:  	at org.springframework.boot.context.embedded.tomcat.TomcatEmbeddedServletContainerFactory.getEmbeddedServletContainer(TomcatEmbeddedServletContainerFactory.java:179)
Buffering:  	at org.springframework.boot.context.embedded.EmbeddedWebApplicationContext.createEmbeddedServletContainer(EmbeddedWebApplicationContext.java:166)
Buffering:  	at org.springframework.boot.context.embedded.EmbeddedWebApplicationContext.onRefresh(EmbeddedWebApplicationContext.java:136)
Buffering:  	... 14 common frames omitted
Buffering:  Caused by: org.apache.catalina.LifecycleException: Failed to start component [StandardServer[-1]]
Buffering:  	at org.apache.catalina.util.LifecycleBase.start(LifecycleBase.java:167)
Buffering:  	at org.apache.catalina.startup.Tomcat.start(Tomcat.java:366)
Buffering:  	at org.springframework.boot.context.embedded.tomcat.TomcatEmbeddedServletContainer.initialize(TomcatEmbeddedServletContainer.java:114)
Buffering:  	... 19 common frames omitted
Buffering:  Caused by: org.apache.catalina.LifecycleException: Failed to start component [StandardService[Tomcat]]
Buffering:  	at org.apache.catalina.util.LifecycleBase.start(LifecycleBase.java:167)
Buffering:  	at org.apache.catalina.core.StandardServer.startInternal(StandardServer.java:793)
Buffering:  	at org.apache.catalina.util.LifecycleBase.start(LifecycleBase.java:150)
Buffering:  	... 21 common frames omitted
Buffering:  Caused by: org.apache.catalina.LifecycleException: Failed to start component [StandardEngine[Tomcat]]
Buffering:  	at org.apache.catalina.util.LifecycleBase.start(LifecycleBase.java:167)
Buffering:  	at org.apache.catalina.core.StandardService.startInternal(StandardService.java:422)
Buffering:  	at org.apache.catalina.util.LifecycleBase.start(LifecycleBase.java:150)
Buffering:  	... 23 common frames omitted
Buffering:  Caused by: org.apache.catalina.LifecycleException: Failed to initialize component [Realm[Simple]]
Buffering:  	at org.apache.catalina.util.LifecycleBase.init(LifecycleBase.java:112)
Buffering:  	at org.apache.catalina.util.LifecycleBase.start(LifecycleBase.java:140)
Buffering:  	at org.apache.catalina.core.ContainerBase.startInternal(ContainerBase.java:928)
Buffering:  	at org.apache.catalina.core.StandardEngine.startInternal(StandardEngine.java:262)
Buffering:  	at org.apache.catalina.util.LifecycleBase.start(LifecycleBase.java:150)
Buffering:  	... 25 common frames omitted
Buffering:  Caused by: java.lang.VerifyError: Stack map does not match the one at exception handler 273
Buffering:  Exception Details:
Buffering:    Location:
Buffering:      org/apache/catalina/connector/Request.startAsync(Ljavax/servlet/ServletRequest;Ljavax/servlet/ServletResponse;)Ljavax/servlet/AsyncContext; @273: astore
Buffering:    Reason:
Buffering:      Type 'java/lang/Object' (current frame, locals[5]) is not assignable to 'org/apache/catalina/core/AsyncContextImpl' (stack map, locals[5])
Buffering:    Current Frame:
Buffering:      bci: @258
Buffering:      flags: { }
Buffering:      locals: { 'org/apache/catalina/connector/Request', 'javax/servlet/ServletRequest', 'javax/servlet/ServletResponse', top, 'com/navercorp/pinpoint/bootstrap/interceptor/Interceptor', 'java/lang/Object', null, '[Ljava/lang/Object;', long, long_2nd, 'java/lang/Object', 'javax/servlet/AsyncContext' }
Buffering:      stack: { 'java/lang/Throwable' }
Buffering:    Stackmap Frame:
Buffering:      bci: @273
Buffering:      flags: { }
Buffering:      locals: { 'org/apache/catalina/connector/Request', 'javax/servlet/ServletRequest', 'javax/servlet/ServletResponse', top, 'com/navercorp/pinpoint/bootstrap/interceptor/Interceptor', 'org/apache/catalina/core/AsyncContextImpl', null, '[Ljava/lang/Object;', long, long_2nd, 'java/lang/Object' }
Buffering:      stack: { 'java/lang/Throwable' }
Buffering:    Bytecode:
Buffering:      0x0000000: 1408 6d37 0801 3a0a b208 5099 0015 1608
Buffering:      0x0000010: b808 5637 0816 08b8 085a 3a0a a700 0457
Buffering:      0x0000020: 1102 9cb8 03c2 3a04 013a 0501 3a06 05bd
Buffering:      0x0000030: 0004 5903 2b53 5904 2c53 3a07 1904 c003
Buffering:      0x0000040: c42a 1907 b903 c803 002a b603 cb9a 0036
Buffering:      0x0000050: bb02 7859 b201 7d13 03cd b601 84b7 027b
Buffering:      0x0000060: 4eb2 017b b201 7d13 03cf 04bd 0004 5903
Buffering:      0x0000070: 2ab7 03d2 b803 d853 b603 6d2d b901 8a03
Buffering:      0x0000080: 002d bf2a b400 e8c7 000f 2abb 0167 592a
Buffering:      0x0000090: b703 d9b5 00e8 2ab4 00e8 2ab6 024e 2b2c
Buffering:      0x00000a0: 2b2a b603 8ca6 0012 2c2a b603 dbb6 03b9
Buffering:      0x00000b0: a600 0704 a700 0403 b603 df2a b400 e82a
Buffering:      0x00000c0: b603 e1b6 03e4 b603 e82a b400 e859 3a05
Buffering:      0x00000d0: 013a 0619 04c0 03c4 2a19 0719 0519 06b9
Buffering:      0x00000e0: 03ec 0500 3a0b 190a c600 2616 0819 0a19
Buffering:      0x00000f0: 0b2b 2cb8 0861 1608 190a 190b b808 66a7
Buffering:      0x0000100: 000f 3a0c 190c b808 6ca7 0005 3a0c 190b
Buffering:      0x0000110: b03a 0601 3a05 1904 c003 c42a 1907 1905
Buffering:      0x0000120: 1906 b903 ec05 0019 06bf               
Buffering:    Exception Handler Table:
Buffering:      bci [260, 265] => handler: 268
Buffering:      bci [246, 255] => handler: 258
Buffering:      bci [14, 28] => handler: 31
Buffering:      bci [73, 273] => handler: 273
Buffering:    Stackmap Table:
Buffering:      full_frame(@31,{Object[#2],Object[#455],Object[#1015],Top,Top,Top,Top,Top,Long,Object[#4]},{Object[#2140]})
Buffering:      same_frame(@32)
Buffering:      full_frame(@131,{Object[#2],Object[#455],Object[#1015],Top,Object[#1017],Null,Null,Object[#939],Long,Object[#4]},{})
Buffering:      same_frame(@150)
Buffering:      full_frame(@183,{Object[#2],Object[#455],Object[#1015],Top,Object[#1017],Null,Null,Object[#939],Long,Object[#4]},{Object[#359],Object[#592],Object[#455],Object[#1015]})
Buffering:      full_frame(@184,{Object[#2],Object[#455],Object[#1015],Top,Object[#1017],Null,Null,Object[#939],Long,Object[#4]},{Object[#359],Object[#592],Object[#455],Object[#1015],Integer})
Buffering:      full_frame(@258,{Object[#2],Object[#455],Object[#1015],Top,Object[#1017],Object[#4],Null,Object[#939],Long,Object[#4],Object[#2162]},{Object[#2140]})
Buffering:      full_frame(@268,{Object[#2],Object[#455],Object[#1015],Top,Object[#1017],Object[#4],Null,Object[#939],Long,Object[#4],Object[#2162],Object[#2140]},{Object[#2140]})
Buffering:      chop_frame(@270,1)
Buffering:      full_frame(@273,{Object[#2],Object[#455],Object[#1015],Top,Object[#1017],Object[#359],Null,Object[#939],Long,Object[#4]},{Object[#366]})
Buffering:  
Buffering:  	at java.lang.Class.getDeclaredConstructors0(Native Method)
Buffering:  	at java.lang.Class.privateGetDeclaredConstructors(Class.java:2671)
Buffering:  	at java.lang.Class.getConstructor0(Class.java:3075)
Buffering:  	at java.lang.Class.getConstructor(Class.java:1825)
Buffering:  	at org.apache.tomcat.util.modeler.modules.MbeansDescriptorsIntrospectionSource.isBeanCompatible(MbeansDescriptorsIntrospectionSource.java:167)
Buffering:  	at org.apache.tomcat.util.modeler.modules.MbeansDescriptorsIntrospectionSource.supportedType(MbeansDescriptorsIntrospectionSource.java:140)
Buffering:  	at org.apache.tomcat.util.modeler.modules.MbeansDescriptorsIntrospectionSource.initMethods(MbeansDescriptorsIntrospectionSource.java:258)
Buffering:  	at org.apache.tomcat.util.modeler.modules.MbeansDescriptorsIntrospectionSource.createManagedBean(MbeansDescriptorsIntrospectionSource.java:300)
Buffering:  	at org.apache.tomcat.util.modeler.modules.MbeansDescriptorsIntrospectionSource.execute(MbeansDescriptorsIntrospectionSource.java:78)
Buffering:  	at org.apache.tomcat.util.modeler.modules.MbeansDescriptorsIntrospectionSource.loadDescriptors(MbeansDescriptorsIntrospectionSource.java:71)
Buffering:  	at org.apache.tomcat.util.modeler.Registry.load(Registry.java:587)
Buffering:  	at org.apache.tomcat.util.modeler.Registry.findManagedBean(Registry.java:488)
Buffering:  	at org.apache.tomcat.util.modeler.Registry.registerComponent(Registry.java:619)
Buffering:  	at org.apache.catalina.util.LifecycleMBeanBase.register(LifecycleMBeanBase.java:161)
Buffering:  	at org.apache.catalina.util.LifecycleMBeanBase.initInternal(LifecycleMBeanBase.java:61)
Buffering:  	at org.apache.catalina.realm.RealmBase.initInternal(RealmBase.java:1090)
Buffering:  	at org.apache.catalina.util.LifecycleBase.init(LifecycleBase.java:107)
Buffering:  	... 29 common frames omitted
Buffering:  Exception in thread "main" java.lang.RuntimeException: java.lang.reflect.InvocationTargetException
Buffering:  	at org.springframework.boot.loader.MainMethodRunner.run(MainMethodRunner.java:62)
Buffering:  	at java.lang.Thread.run(Thread.java:748)
Buffering:  Caused by: java.lang.reflect.InvocationTargetException
Buffering:  	at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
Buffering:  	at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62)
Buffering:  	at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
Buffering:  	at java.lang.reflect.Method.invoke(Method.java:498)
Buffering:  	at org.springframework.boot.loader.MainMethodRunner.run(MainMethodRunner.java:54)
Buffering:  	... 1 more
Buffering:  Caused by: org.springframework.context.ApplicationContextException: Unable to start embedded container; nested exception is org.springframework.boot.context.embedded.EmbeddedServletContainerException: Unable to start embedded Tomcat
Buffering:  	at org.springframework.boot.context.embedded.EmbeddedWebApplicationContext.onRefresh(EmbeddedWebApplicationContext.java:139)
Buffering:  	at org.springframework.context.support.AbstractApplicationContext.refresh(AbstractApplicationContext.java:537)
Buffering:  	at org.springframework.boot.context.embedded.EmbeddedWebApplicationContext.refresh(EmbeddedWebApplicationContext.java:124)
Buffering:  	at org.springframework.boot.SpringApplication.refresh(SpringApplication.java:693)
Buffering:  	at org.springframework.boot.SpringApplication.refreshContext(SpringApplication.java:360)
Buffering:  	at org.springframework.boot.SpringApplication.run(SpringApplication.java:303)
Buffering:  	at org.springframework.boot.SpringApplication.run(SpringApplication.java:1118)
Buffering:  	at org.springframework.boot.SpringApplication.run(SpringApplication.java:1107)
Buffering:  	at geex.configCenter.ConfigCenterApplication.main(ConfigCenterApplication.java:15)
Buffering:  	... 6 more
Buffering:  Caused by: org.springframework.boot.context.embedded.EmbeddedServletContainerException: Unable to start embedded Tomcat
Buffering:  	at org.springframework.boot.context.embedded.tomcat.TomcatEmbeddedServletContainer.initialize(TomcatEmbeddedServletContainer.java:138)
Buffering:  	at org.springframework.boot.context.embedded.tomcat.TomcatEmbeddedServletContainer.<init>(TomcatEmbeddedServletContainer.java:87)
Buffering:  	at org.springframework.boot.context.embedded.tomcat.TomcatEmbeddedServletContainerFactory.getTomcatEmbeddedServletContainer(TomcatEmbeddedServletContainerFactory.java:554)
Buffering:  	at org.springframework.boot.context.embedded.tomcat.TomcatEmbeddedServletContainerFactory.getEmbeddedServletContainer(TomcatEmbeddedServletContainerFactory.java:179)
Buffering:  	at org.springframework.boot.context.embedded.EmbeddedWebApplicationContext.createEmbeddedServletContainer(EmbeddedWebApplicationContext.java:166)
Buffering:  	at org.springframework.boot.context.embedded.EmbeddedWebApplicationContext.onRefresh(EmbeddedWebApplicationContext.java:136)
Buffering:  	... 14 more
Buffering:  Caused by: org.apache.catalina.LifecycleException: Failed to start component [StandardServer[-1]]
Buffering:  	at org.apache.catalina.util.LifecycleBase.start(LifecycleBase.java:167)
Buffering:  	at org.apache.catalina.startup.Tomcat.start(Tomcat.java:366)
Buffering:  	at org.springframework.boot.context.embedded.tomcat.TomcatEmbeddedServletContainer.initialize(TomcatEmbeddedServletContainer.java:114)
Buffering:  	... 19 more
Buffering:  Caused by: org.apache.catalina.LifecycleException: Failed to start component [StandardService[Tomcat]]
Buffering:  	at org.apache.catalina.util.LifecycleBase.start(LifecycleBase.java:167)
Buffering:  	at org.apache.catalina.core.StandardServer.startInternal(StandardServer.java:793)
Buffering:  	at org.apache.catalina.util.LifecycleBase.start(LifecycleBase.java:150)
Buffering:  	... 21 more
Buffering:  Caused by: org.apache.catalina.LifecycleException: Failed to start component [StandardEngine[Tomcat]]
Buffering:  	at org.apache.catalina.util.LifecycleBase.start(LifecycleBase.java:167)
Buffering:  	at org.apache.catalina.core.StandardService.startInternal(StandardService.java:422)
Buffering:  	at org.apache.catalina.util.LifecycleBase.start(LifecycleBase.java:150)
Buffering:  	... 23 more
Buffering:  Caused by: org.apache.catalina.LifecycleException: Failed to initialize component [Realm[Simple]]
Buffering:  	at org.apache.catalina.util.LifecycleBase.init(LifecycleBase.java:112)
Buffering:  	at org.apache.catalina.util.LifecycleBase.start(LifecycleBase.java:140)
Buffering:  	at org.apache.catalina.core.ContainerBase.startInternal(ContainerBase.java:928)
Buffering:  	at org.apache.catalina.core.StandardEngine.startInternal(StandardEngine.java:262)
Buffering:  	at org.apache.catalina.util.LifecycleBase.start(LifecycleBase.java:150)
Buffering:  	... 25 more
Buffering:  Caused by: java.lang.VerifyError: Stack map does not match the one at exception handler 273
Buffering:  Exception Details:
Buffering:    Location:
Buffering:      org/apache/catalina/connector/Request.startAsync(Ljavax/servlet/ServletRequest;Ljavax/servlet/ServletResponse;)Ljavax/servlet/AsyncContext; @273: astore
Buffering:    Reason:
Buffering:      Type 'java/lang/Object' (current frame, locals[5]) is not assignable to 'org/apache/catalina/core/AsyncContextImpl' (stack map, locals[5])
Buffering:    Current Frame:
Buffering:      bci: @258
Buffering:      flags: { }
Buffering:      locals: { 'org/apache/catalina/connector/Request', 'javax/servlet/ServletRequest', 'javax/servlet/ServletResponse', top, 'com/navercorp/pinpoint/bootstrap/interceptor/Interceptor', 'java/lang/Object', null, '[Ljava/lang/Object;', long, long_2nd, 'java/lang/Object', 'javax/servlet/AsyncContext' }
Buffering:      stack: { 'java/lang/Throwable' }
Buffering:    Stackmap Frame:
Buffering:      bci: @273
Buffering:      flags: { }
Buffering:      locals: { 'org/apache/catalina/connector/Request', 'javax/servlet/ServletRequest', 'javax/servlet/ServletResponse', top, 'com/navercorp/pinpoint/bootstrap/interceptor/Interceptor', 'org/apache/catalina/core/AsyncContextImpl', null, '[Ljava/lang/Object;', long, long_2nd, 'java/lang/Object' }
Buffering:      stack: { 'java/lang/Throwable' }
Buffering:    Bytecode:
Buffering:      0x0000000: 1408 6d37 0801 3a0a b208 5099 0015 1608
Buffering:      0x0000010: b808 5637 0816 08b8 085a 3a0a a700 0457
Buffering:      0x0000020: 1102 9cb8 03c2 3a04 013a 0501 3a06 05bd
Buffering:      0x0000030: 0004 5903 2b53 5904 2c53 3a07 1904 c003
Buffering:      0x0000040: c42a 1907 b903 c803 002a b603 cb9a 0036
Buffering:      0x0000050: bb02 7859 b201 7d13 03cd b601 84b7 027b
Buffering:      0x0000060: 4eb2 017b b201 7d13 03cf 04bd 0004 5903
Buffering:      0x0000070: 2ab7 03d2 b803 d853 b603 6d2d b901 8a03
Buffering:      0x0000080: 002d bf2a b400 e8c7 000f 2abb 0167 592a
Buffering:      0x0000090: b703 d9b5 00e8 2ab4 00e8 2ab6 024e 2b2c
Buffering:      0x00000a0: 2b2a b603 8ca6 0012 2c2a b603 dbb6 03b9
Buffering:      0x00000b0: a600 0704 a700 0403 b603 df2a b400 e82a
Buffering:      0x00000c0: b603 e1b6 03e4 b603 e82a b400 e859 3a05
Buffering:      0x00000d0: 013a 0619 04c0 03c4 2a19 0719 0519 06b9
Buffering:      0x00000e0: 03ec 0500 3a0b 190a c600 2616 0819 0a19
Buffering:      0x00000f0: 0b2b 2cb8 0861 1608 190a 190b b808 66a7
Buffering:      0x0000100: 000f 3a0c 190c b808 6ca7 0005 3a0c 190b
Buffering:      0x0000110: b03a 0601 3a05 1904 c003 c42a 1907 1905
Buffering:      0x0000120: 1906 b903 ec05 0019 06bf               
Buffering:    Exception Handler Table:
Buffering:      bci [260, 265] => handler: 268
Buffering:      bci [246, 255] => handler: 258
Buffering:      bci [14, 28] => handler: 31
Buffering:      bci [73, 273] => handler: 273
Buffering:    Stackmap Table:
Buffering:      full_frame(@31,{Object[#2],Object[#455],Object[#1015],Top,Top,Top,Top,Top,Long,Object[#4]},{Object[#2140]})
Buffering:      same_frame(@32)
Buffering:      full_frame(@131,{Object[#2],Object[#455],Object[#1015],Top,Object[#1017],Null,Null,Object[#939],Long,Object[#4]},{})
Buffering:      same_frame(@150)
Buffering:      full_frame(@183,{Object[#2],Object[#455],Object[#1015],Top,Object[#1017],Null,Null,Object[#939],Long,Object[#4]},{Object[#359],Object[#592],Object[#455],Object[#1015]})
Buffering:      full_frame(@184,{Object[#2],Object[#455],Object[#1015],Top,Object[#1017],Null,Null,Object[#939],Long,Object[#4]},{Object[#359],Object[#592],Object[#455],Object[#1015],Integer})
Buffering:      full_frame(@258,{Object[#2],Object[#455],Object[#1015],Top,Object[#1017],Object[#4],Null,Object[#939],Long,Object[#4],Object[#2162]},{Object[#2140]})
Buffering:      full_frame(@268,{Object[#2],Object[#455],Object[#1015],Top,Object[#1017],Object[#4],Null,Object[#939],Long,Object[#4],Object[#2162],Object[#2140]},{Object[#2140]})
Buffering:      chop_frame(@270,1)
Buffering:      full_frame(@273,{Object[#2],Object[#455],Object[#1015],Top,Object[#1017],Object[#359],Null,Object[#939],Long,Object[#4]},{Object[#366]})
Buffering:  
Buffering:  	at java.lang.Class.getDeclaredConstructors0(Native Method)
Buffering:  	at java.lang.Class.privateGetDeclaredConstructors(Class.java:2671)
Buffering:  	at java.lang.Class.getConstructor0(Class.java:3075)
Buffering:  	at java.lang.Class.getConstructor(Class.java:1825)
Buffering:  	at org.apache.tomcat.util.modeler.modules.MbeansDescriptorsIntrospectionSource.isBeanCompatible(MbeansDescriptorsIntrospectionSource.java:167)
Buffering:  	at org.apache.tomcat.util.modeler.modules.MbeansDescriptorsIntrospectionSource.supportedType(MbeansDescriptorsIntrospectionSource.java:140)
Buffering:  	at org.apache.tomcat.util.modeler.modules.MbeansDescriptorsIntrospectionSource.initMethods(MbeansDescriptorsIntrospectionSource.java:258)
Buffering:  	at org.apache.tomcat.util.modeler.modules.MbeansDescriptorsIntrospectionSource.createManagedBean(MbeansDescriptorsIntrospectionSource.java:300)
Buffering:  	at org.apache.tomcat.util.modeler.modules.MbeansDescriptorsIntrospectionSource.execute(MbeansDescriptorsIntrospectionSource.java:78)
Buffering:  	at org.apache.tomcat.util.modeler.modules.MbeansDescriptorsIntrospectionSource.loadDescriptors(MbeansDescriptorsIntrospectionSource.java:71)
Buffering:  	at org.apache.tomcat.util.modeler.Registry.load(Registry.java:587)
Buffering:  	at org.apache.tomcat.util.modeler.Registry.findManagedBean(Registry.java:488)
Buffering:  	at org.apache.tomcat.util.modeler.Registry.registerComponent(Registry.java:619)
Buffering:  	at org.apache.catalina.util.LifecycleMBeanBase.register(LifecycleMBeanBase.java:161)
Buffering:  	at org.apache.catalina.util.LifecycleMBeanBase.initInternal(LifecycleMBeanBase.java:61)
Buffering:  	at org.apache.catalina.realm.RealmBase.initInternal(RealmBase.java:1090)
Buffering:  	at org.apache.catalina.util.LifecycleBase.init(LifecycleBase.java:107)
Buffering:  	... 29 more
```

### 解决方案
根据异常信息 `Caused by: java.lang.VerifyError: Stack map does not match the one at exception handler 273`，加上想到pinpoint时通过增强字节码实现监控的，猜测可能是类验证出了问题，尝试关闭类验证，加入以下参数跳过类加载器的验证：
```
-noverify(JAVA8)
-XX:-UseSplitVerifier(JAVA7)
```
问题解决。