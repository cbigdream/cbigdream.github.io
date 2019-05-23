---
title: Tomcat启动时卡住
date: 2019-05-23 14:49:32
categories: 技术
tags: [linux, tomcat]

---
tomcat启动以后卡在INFO: Deploying web application directory ...

<!-- more -->

在weblogic的官方文档中 Monitoring and Troubleshooting 标题为 [Avoiding JVM Delays Caused By Random Number Generation](https://docs.oracle.com/cd/E13209_01/wlcp/wlss30/configwlss/jvmrand.html) 的文档

The library used for random number generation in Sun's JVM relies on /dev/random by default for UNIX platforms. This can potentially block the WebLogic SIP Server process because on some operating systems /dev/random waits for a certain amount of "noise" to be generated on the host machine before returning a result. Although /dev/random is more secure, BEA recommends using /dev/urandom if the default JVM configuration delays WebLogic SIP Server startup.

To determine if your operating system exhibits this behavior, try displaying a portion of the file from a shell prompt:

head -n 1 /dev/random

If the command returns immediately, you can use /dev/random as the default generator for SUN's JVM. If the command does not return immediately, use these steps to configure the JVM to use /dev/urandom:
```
Open the $JAVA_HOME/jre/lib/security/java.security file in a text editor.
Change the line:
  securerandom.source=file:/dev/random
to read:
  securerandom.source=file:/dev/./urandom
Save your change and exit the text editor.
```