Java WebSockets
===============
[![Build Status](https://travis-ci.org/marci4/Java-WebSocket-Dev.svg?branch=master)](https://travis-ci.org/marci4/Java-WebSocket-Dev)
[![Javadocs](https://www.javadoc.io/badge/org.java-websocket/Java-WebSocket.svg)](https://www.javadoc.io/doc/org.java-websocket/Java-WebSocket)
[![Maven Central](https://img.shields.io/maven-central/v/org.java-websocket/Java-WebSocket.svg)](https://mvnrepository.com/artifact/org.java-websocket/Java-WebSocket)
[![Sonatype Nexus (Snapshots)](https://img.shields.io/nexus/s/https/oss.sonatype.org/org.java-websocket/Java-WebSocket.svg)](https://oss.sonatype.org/content/repositories/snapshots/org/java-websocket/Java-WebSocket/)

This repository contains a barebones WebSocket server and client implementation
written in 100% Java. The underlying classes are implemented `java.nio`, which allows for a
non-blocking event-driven model (similar to the
[WebSocket API](http://dev.w3.org/html5/websockets/) for web browsers).

Implemented WebSocket protocol versions are:

 * [RFC 6455](http://tools.ietf.org/html/rfc6455)

[Here](https://github.com/TooTallNate/Java-WebSocket/wiki/Drafts) some more details about protocol versions/drafts. 


## Build
You can build using Ant, Maven, Gradle or Leiningen but there is nothing against just putting the source path ```src/main/java ``` on your applications buildpath.

### Ant

``` bash
ant 
```

will create the javadoc of this library at ```doc/``` and build the library itself: ```dist/java_websocket.jar```

The ant targets are: ```compile```, ```jar```, ```doc``` and ```clean```

### Maven
To use maven add this dependency to your pom.xml:
```xml
<dependency>
  <groupId>org.java-websocket</groupId>
  <artifactId>Java-WebSocket</artifactId>
  <version>1.3.8</version>
</dependency>
```

### Gradle
To use Gradle add the maven central repository to your repositories list :
```xml
mavenCentral()
```
Then you can just add the latest version to your build.
```xml
compile "org.java-websocket:Java-WebSocket:1.3.8"
```

Writing your own WebSocket Server
---------------------------------

The `org.java_websocket.server.WebSocketServer` abstract class implements the
server-side of the
[WebSocket Protocol](http://www.whatwg.org/specs/web-socket-protocol/).
A WebSocket server by itself doesn't do anything except establish socket
connections though HTTP. After that it's up to **your** subclass to add purpose.

An example for a WebSocketServer can be found in both the [wiki](https://github.com/TooTallNate/Java-WebSocket/wiki#server-example) and the [example](https://github.com/TooTallNate/Java-WebSocket/tree/master/src/main/example) folder.

Writing your own WebSocket Client
---------------------------------

The `org.java_websocket.client.WebSocketClient` abstract class can connect to
valid WebSocket servers. The constructor expects a valid `ws://` URI to
connect to. Important events `onOpen`, `onClose`, `onMessage` and `onError`
get fired throughout the life of the WebSocketClient, and must be implemented 
in **your** subclass.

An example for a WebSocketClient can be found in both the [wiki](https://github.com/TooTallNate/Java-WebSocket/wiki#client-example) and the [example](https://github.com/TooTallNate/Java-WebSocket/tree/master/src/main/example) folder.

Examples
-------------------
 
You can find a lot of examples [here](https://github.com/TooTallNate/Java-WebSocket/tree/master/src/main/example).

WSS Support
---------------------------------
This library supports wss.
To see how to use wss please take a look at the examples.<br>

If you do not have a valid **certificate** in place then you will have to create a self signed one.
Browsers will simply refuse the connection in case of a bad certificate and will not ask the user to accept it.
So the first step will be to make a browser to accept your self signed certificate. ( https://bugzilla.mozilla.org/show_bug.cgi?id=594502 ).<br>
If the websocket server url is `wss://localhost:8000` visit the url `https://localhost:8000` with your browser. The browser will recognize the handshake and allow you to accept the certificate. This technique is also demonstrated in this [video](http://www.youtube.com/watch?v=F8lBdfAZPkU).

The vm option `-Djavax.net.debug=all` can help to find out if there is a problem with the certificate.

It is currently not possible to accept ws and wss connections at the same time via the same websocket server instance.

For some reason Firefox does not allow multiple connections to the same wss server if the server uses a different port than the default port (443).


If you want to use `wss` on the android platfrom you should take a look at [this](http://blog.antoine.li/2010/10/22/android-trusting-ssl-certificates/).

I ( @Davidiusdadi ) would be glad if you would give some feedback whether wss is working fine for you or not.

Minimum Required JDK
--------------------

`Java-WebSocket` is known to work with:

 * Java 1.6 and higher
 * Android 4.0 and higher

Other JRE implementations may work as well, but haven't been tested.


Testing in Android Emulator
---------------------------

Please note Android Emulator has issues using `IPv6 addresses`. Executing any
socket related code (like this library) inside it will address an error

``` bash
java.net.SocketException: Bad address family
```

You have to manually disable `IPv6` by calling

``` java
java.lang.System.setProperty("java.net.preferIPv6Addresses", "false");
java.lang.System.setProperty("java.net.preferIPv4Stack", "true");
```

somewhere in your project, before instantiating the `WebSocketClient` class. 
You can check if you are currently testing in the Android Emulator like this

``` java
if ("google_sdk".equals( Build.PRODUCT )) {
  // ... disable IPv6
}
```


License
-------

Everything found in this repo is licensed under an MIT license. See
the `LICENSE` file for specifics.
