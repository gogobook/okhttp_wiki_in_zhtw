OkHttp requires Java 7 to build and run tests. Runtime compatibility with Java 6 is enforced as part of the build to ensure compliance with Android and older versions of the JVM.

### Desktop Testing with Maven

Run OkHttp tests on the desktop with Maven. Running HTTP/2 and SPDY tests on the desktop uses [Jetty-ALPN][2] when OpenJDK 8 or [Jetty-NPN][1] when running OpenJDK 7.

```
mvn clean test
```

### Desktop Testing without Maven

If you're working in an IDE, or in another environment where Maven configuration isn't honored, you'll need to manually enable NPN/ALPN. Add this JVM flag for OpenJDK 8:
```
-Xbootclasspath/p:~/.m2/repository/org/mortbay/jetty/alpn/alpn-boot/8.0.0.v20140317/alpn-boot-8.0.0.v20140317.jar
```

Add this JVM flag for OpenJDK 7:
```
-Xbootclasspath/p:~/.m2/repository/org/mortbay/jetty/npn/npn-boot/1.1.7.v20140316/npn-boot-1.1.7.v20140316.jar
```

### Device Testing

OkHttp's test suite creates an in-process HTTPS server. Prior to Android 2.3, SSL server sockets were broken, and so HTTPS tests will time out when run on such devices.

Test on a USB-attached Android using [Vogar][3]. Unfortunately `dx` requires that you build with Java 6, otherwise the test class will be silently omitted from the `.dex` file.

```
mvn clean
mvn package -DskipTests
vogar \
    --classpath ~/.m2/repository/org/bouncycastle/bcprov-jdk15on/1.48/bcprov-jdk15on-1.48.jar \
    --classpath ~/.m2/repository/com/squareup/okio/okio/1.0.0/okio-1.0.0.jar \
    --classpath okhttp/target/okhttp-2.0.0-SNAPSHOT.jar \
    ./samples/guide/src/main/java/com/squareup/okhttp/recipes/SynchronousGet.java
```

 [1]: https://github.com/jetty-project/jetty-npn
 [2]: https://github.com/jetty-project/jetty-alpn
 [3]: https://code.google.com/p/vogar/
