---
layout:     post
title:      "Suspected memory leakage investigation"
date:       2016-11-14 12:00:00
author:     "henry"
header-img: "img/post-bg-05.jpg"
---

<h2 class="section-heading">Problem description</h2>
<p>Memory usage keeps going up, suspected memory leakage in the program.</p>

<h2 class="section-heading">Initial analysis</h2>
<p>From VisualVM</p>
<a href="#">
    <img src="{{ site.baseurl }}/img/suspected-memory-leakage/suspected-memory-leakage-1.png" alt="suspected memory leakage">
</a>
<a href="#">
    <img src="{{ site.baseurl }}/img/suspected-memory-leakage/suspected-memory-leakage-3.png" alt="suspected memory leakage">
</a>

<p>From MemoryAnalyzer</p>
<a href="#">
    <img src="{{ site.baseurl }}/img/suspected-memory-leakage/suspected-memory-leakage-10.png" alt="suspected memory leakage">
</a>
<a href="#">
    <img src="{{ site.baseurl }}/img/suspected-memory-leakage/suspected-memory-leakage-11.png" alt="suspected memory leakage">
</a>
<a href="#">
    <img src="{{ site.baseurl }}/img/suspected-memory-leakage/suspected-memory-leakage-2.png" alt="suspected memory leakage">
</a>

<p>So, the problem should between X and Y.</p>
<p>A litter backgroud here: X provides a webservice interface to Y which will be used to send data to X.</p>

<h2 class="section-heading">At the crossroad</h2>

<p>Sometimes, even google can’t lead you to a solution of a problem. So start reading the source code related to these parts.</p>

<p><a href="http://grepcode.com/file/repository.grepcode.com/java/root/jdk/openjdk/8u40-b25/sun/net/httpserver/ServerImpl.java#ServerImpl.ServerTimerTask1">ServerImpl</a></p>

<p><a href="http://grepcode.com/file/repository.grepcode.com/java/root/jdk/openjdk/8u40-b25/sun/net/httpserver/ExchangeImpl.java#ExchangeImpl">ExchangeImpl</a></p>

<p><a href="http://grepcode.com/file/repository.grepcode.com/java/root/jdk/openjdk/8u40-b25/sun/net/httpserver/Request.java#Request">Request</a></p>

<p><a href="http://grepcode.com/file/repository.grepcode.com/java/root/jdk/openjdk/8u40-b25/sun/net/httpserver/HttpConnection.java#HttpConnection">HttpConnection</a></p>

<p><a href="http://grepcode.com/file/repository.grepcode.com/java/root/jdk/openjdk/8u40-b25/sun/net/httpserver/ServerConfig.java#ServerConfig.0DEFAULT_TIMER_MILLIS">ServerConfig</a></p>

<p>After checking code, I listed here the most important part:</p>

<a href="#">
    <img src="{{ site.baseurl }}/img/suspected-memory-leakage/suspected-memory-leakage-4.png" alt="suspected memory leakage">
</a>

<a href="#">
    <img src="{{ site.baseurl }}/img/suspected-memory-leakage/suspected-memory-leakage-5.png" alt="suspected memory leakage">
</a>

<a href="#">
    <img src="{{ site.baseurl }}/img/suspected-memory-leakage/suspected-memory-leakage-6.png" alt="suspected memory leakage">
</a>

<p>As default setting doesn’t schedule ServerTimerTask1, so I suspect rspConnections still retained the reference to HttpConnection.</p>

<p>So why not to give it a trial?</p>

<p>As we can see from code above, "com.sun.net.httpserver" is used as the logger name. So which naturally lead me to “how to enable java.util.logging.Logger?”.</p>

<h2 class="section-heading">How to enable java.util.logging.Logger</h2>

<p>By default, java.util.logging.Logger would use jre/lib/logging.properties as the configuration file, which can also be changed via -Djava.util.logging.config.file=<path></p>

[root@testlab ~]# ps -ef | grep 'keyword' | grep 'java.util.logging.config.file' --color
user   48908     1  3 05:24 ?        00:07:28 /usr/lib/jvm/jre-1.8.0-openjdk.x86_64/bin/java -server ... -Djava.util.logging.config.file=/opt/xx/conf/logging.properties -Dsun.net.httpserver.debug=true -Dsun.net.httpserver.maxReqTime=60 -Dsun.net.httpserver.maxRspTime=60

<p>Edit /opt/xx/conf/logging.properties to add FileHandler</p>
<blockquote>
handlers=java.util.logging.FileHandler
com.sun.net.httpserver.level = FINEST
java.util.logging.FileHandler.pattern = /var/log/xx/henry-%g.log
java.util.logging.FileHandler.limit = 50000
java.util.logging.FileHandler.count = 1
java.util.logging.FileHandler.formatter = java.util.logging.SimpleFormatter
</blockquote>

<p>Restart service and check log</p>
<blockquote>
[root@testlab ~]# tail -n 5 /var/log/xx/henry-0.log
FINE: GET /NotificationSender/NotificationSenderService?wsdl=1 HTTP/1.1 [200  OK] ()
Nov 11, 2016 8:46:38 AM sun.net.httpserver.ServerImpl$ServerTimerTask1 run
FINE: closing: no response: java.nio.channels.SocketChannel[closed]
Nov 11, 2016 8:46:38 AM sun.net.httpserver.ServerImpl$ServerTimerTask1 run
FINE: closing: no response: java.nio.channels.SocketChannel[closed]
</blockquote>

<h2 class="section-heading">Memory footprint before vs after tuning</h2>
<p>Before</p>
<a href="#">
    <img src="{{ site.baseurl }}/img/suspected-memory-leakage/suspected-memory-leakage-1.png" alt="suspected memory leakage">
</a>
<p>After</p>
<a href="#">
    <img src="{{ site.baseurl }}/img/suspected-memory-leakage/suspected-memory-leakage-7.png" alt="suspected memory leakage">
</a>
<p>Before</p>
<a href="#">
    <img src="{{ site.baseurl }}/img/suspected-memory-leakage/suspected-memory-leakage-9.png" alt="suspected memory leakage">
</a>
<p>After</p>
<a href="#">
    <img src="{{ site.baseurl }}/img/suspected-memory-leakage/suspected-memory-leakage-8.png" alt="suspected memory leakage">
</a>

<h2 class="section-heading">Take away</h2>
<p>When interoperating with outside system, timeout and retry mechanism should be implemented (or at least be taken into consideration) on the client side.</p>
<p>When providing webservice via jdk’s build-in @WebService, parameters can be tuned were listed below. The red one should/must be tuned.</p>
<blockquote>
sun.net.httpserver.idleInterval
sun.net.httpserver.clockTick
sun.net.httpserver.maxIdleConnections
sun.net.httpserver.drainAmount
sun.net.httpserver.maxReqHeaders
sun.net.httpserver.maxReqTime
sun.net.httpserver.maxRspTime
sun.net.httpserver.timerMillis
sun.net.httpserver.debug
sun.net.httpserver.nodelay
</blockquote>

<p>This is effective to jdk1.8, parameters may change as jdk evolve.</p>