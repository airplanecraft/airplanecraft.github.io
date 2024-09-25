+++
Categories = ["Technology"]
Description = "service catalog"
Tags = ["java", "jdk"]
date = "2017-08-29T10:40:37+08:00"
menu = "java"
title = "JDk版本問題造成的cglib運行時候出錯"
toc = true
+++

## 異常信息 ##

 ```
 Caused by: java.lang.ExceptionInInitializerError: null
        at org.springframework.context.annotation.ConfigurationClassEnhancer.newEnhancer(ConfigurationClassEnhancer.java:122) ~[spring-context-5.0.4.RELEASE.jar!/:5.0.4.RELEASE]
        at org.springframework.context.annotation.ConfigurationClassEnhancer.enhance(ConfigurationClassEnhancer.java:110) ~[spring-context-5.0.4.RELEASE.jar!/:5.0.4.RELEASE]
        at org.springframework.context.annotation.ConfigurationClassPostProcessor.enhanceConfigurationClasses(ConfigurationClassPostProcessor.java:405) ~[spring-context-5.0.4.RELEASE.jar!/:5.0.4.RELEASE]
        ... 20 common frames omitted
Caused by: org.springframework.cglib.core.CodeGenerationException: java.lang.reflect.InaccessibleObjectException-->Unable to make protected final java.lang.Class java.lang.ClassLoader.defineClass(java.lang.String,byte[],int,int,java.security.ProtectionDomain) throws java.lang.ClassFormatError accessible: module java.base does not "opens java.lang" to unnamed module @1134affc
        at org.springframework.cglib.core.ReflectUtils.defineClass(ReflectUtils.java:464) ~[spring-core-5.0.4.RELEASE.jar!/:5.0.4.RELEASE]
        at org.springframework.cglib.core.AbstractClassGenerator.generate(AbstractClassGenerator.java:336) ~[spring-core-5.0.4.RELEASE.jar!/:5.0.4.RELEASE]
        at org.springframework.cglib.core.AbstractClassGenerator$ClassLoaderData$3.apply(AbstractClassGenerator.java:93) ~[spring-core-5.0.4.RELEASE.jar!/:5.0.4.RELEASE]
        at org.springframework.cglib.core.AbstractClassGenerator$ClassLoaderData$3.apply(AbstractClassGenerator.java:91) ~[spring-core-5.0.4.RELEASE.jar!/:5.0.4.RELEASE]
        at org.springframework.cglib.core.internal.LoadingCache$2.call(LoadingCache.java:54) ~[spring-core-5.0.4.RELEASE.jar!/:5.0.4.RELEASE]
        at java.base/java.util.concurrent.FutureTask.run(FutureTask.java:264) ~[na:na]
        at org.springframework.cglib.core.internal.LoadingCache.createEntry(LoadingCache.java:61) ~[spring-core-5.0.4.RELEASE.jar!/:5.0.4.RELEASE]
        at org.springframework.cglib.core.internal.LoadingCache.get(LoadingCache.java:34) ~[spring-core-5.0.4.RELEASE.jar!/:5.0.4.RELEASE]
        at org.springframework.cglib.core.AbstractClassGenerator$ClassLoaderData.get(AbstractClassGenerator.java:116) ~[spring-core-5.0.4.RELEASE.jar!/:5.0.4.RELEASE]
        at org.springframework.cglib.core.AbstractClassGenerator.create(AbstractClassGenerator.java:291) ~[spring-core-5.0.4.RELEASE.jar!/:5.0.4.RELEASE]
        at org.springframework.cglib.core.KeyFactory$Generator.create(KeyFactory.java:221) ~[spring-core-5.0.4.RELEASE.jar!/:5.0.4.RELEASE]
        at org.springframework.cglib.core.KeyFactory.create(KeyFactory.java:174) ~[spring-core-5.0.4.RELEASE.jar!/:5.0.4.RELEASE]
        at org.springframework.cglib.core.KeyFactory.create(KeyFactory.java:153) ~[spring-core-5.0.4.RELEASE.jar!/:5.0.4.RELEASE]
        at org.springframework.cglib.proxy.Enhancer.<clinit>(Enhancer.java:73) ~[spring-core-5.0.4.RELEASE.jar!/:5.0.4.RELEASE]
        ... 23 common frames omitted
Caused by: java.lang.reflect.InaccessibleObjectException: Unable to make protected final java.lang.Class java.lang.ClassLoader.defineClass(java.lang.String,byte[],int,int,java.security.ProtectionDomain) throws java.lang.ClassFormatError accessible: module java.base does not "opens java.lang" to unnamed module @1134affc
        at java.base/java.lang.reflect.AccessibleObject.checkCanSetAccessible(AccessibleObject.java:357) ~[na:na]
        at java.base/java.lang.reflect.AccessibleObject.checkCanSetAccessible(AccessibleObject.java:297) ~[na:na]
        at java.base/java.lang.reflect.Method.checkCanSetAccessible(Method.java:199) ~[na:na]
        at java.base/java.lang.reflect.Method.setAccessible(Method.java:193) ~[na:na]
        at org.springframework.cglib.core.ReflectUtils$1.run(ReflectUtils.java:61) ~[spring-core-5.0.4.RELEASE.jar!/:5.0.4.RELEASE]
        at java.base/java.security.AccessController.doPrivileged(AccessController.java:554) ~[na:na]
        at org.springframework.cglib.core.ReflectUtils.<clinit>(ReflectUtils.java:52) ~[spring-core-5.0.4.RELEASE.jar!/:5.0.4.RELEASE]
        at org.springframework.cglib.core.KeyFactory$Generator.generateClass(KeyFactory.java:243) ~[spring-core-5.0.4.RELEASE.jar!/:5.0.4.RELEASE]
        at org.springframework.cglib.core.DefaultGeneratorStrategy.generate(DefaultGeneratorStrategy.java:25) ~[spring-core-5.0.4.RELEASE.jar!/:5.0.4.RELEASE]
        at org.springframework.cglib.core.AbstractClassGenerator.generate(AbstractClassGenerator.java:329) ~[spring-core-5.0.4.RELEASE.jar!/:5.0.4.RELEASE]
        ... 35 common frames omitted
 ```

## 挖掘問題 ##

1. eclipse compiler和jdk版本都是1.8 但是外面是java16,在eclipse裏面用JAVA8都是沒有任何問題,但是在JAVA16就出錯,這是因爲在java9中引入了module，現實大部分的企業級開發還是在使用java8所以一般不會遇到這個問題，只需要運行的時候如下

2. 
  ```
  java   --add-opens java.base/java.lang=ALL-UNNAMED  -jar redis-cluster-0.0.1-SNAPSHOT.jar
  ```
