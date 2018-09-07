# Adding Password Protected Maven Repository URL in Gradle

In my current project at work, I had to setup a private maven repository hosted at [Artifactory](https://jfrog.com/artifactory/). I was assigned login credentials to access the libraries (or artifacts) from the repository. But, when I was fetching it from Android Studio project through ```build.gradle``` file, I was getting this exception on syncing the project.

```
org.gradle.internal.resource.transport.http.HttpRequestException: Could not GET 'MY_MAVEN_ARTIFACT_URL_GOES_HERE'.
Caused by: java.net.SocketTimeoutException: Read timed out
at java.net.SocketInputStream.socketRead0(Native Method)
at java.net.SocketInputStream.socketRead(SocketInputStream.java:116)
at java.net.SocketInputStream.read(SocketInputStream.java:171)
at java.net.SocketInputStream.read(SocketInputStream.java:141)
at org.apache.http.impl.io.SessionInputBufferImpl.streamRead(SessionInputBufferImpl.java:139)
at org.apache.http.impl.io.SessionInputBufferImpl.fillBuffer(SessionInputBufferImpl.java:155)
at org.apache.http.impl.io.SessionInputBufferImpl.readLine(SessionInputBufferImpl.java:284)
at org.apache.http.impl.conn.DefaultHttpResponseParser.parseHead(DefaultHttpResponseParser.java:140)
at org.apache.http.impl.conn.DefaultHttpResponseParser.parseHead(DefaultHttpResponseParser.java:57)
at org.apache.http.impl.io.AbstractMessageParser.parse(AbstractMessageParser.java:261)
at org.apache.http.impl.DefaultBHttpClientConnection.receiveResponseHeader(DefaultBHttpClientConnection.java:165)
at org.apache.http.impl.conn.CPoolProxy.receiveResponseHeader(CPoolProxy.java:167)
at org.apache.http.protocol.HttpRequestExecutor.doReceiveResponse(HttpRequestExecutor.java:272)
at org.apache.http.protocol.HttpRequestExecutor.execute(HttpRequestExecutor.java:124)
at org.apache.http.impl.execchain.MainClientExec.execute(MainClientExec.java:271)
at org.apache.http.impl.execchain.ProtocolExec.execute(ProtocolExec.java:184)
at org.apache.http.impl.execchain.RetryExec.execute(RetryExec.java:88)
at org.apache.http.impl.execchain.RedirectExec.execute(RedirectExec.java:110)
at org.apache.http.impl.client.InternalHttpClient.doExecute(InternalHttpClient.java:184)
at org.apache.http.impl.client.CloseableHttpClient.execute(CloseableHttpClient.java:82)
```

As the exception says, I was confused and I thought that my private maven URL is not valid etc. And I tried following methods to solve this issue:
* Invalidating cache and restart Android studio (several times) but didn't worked for me.
* Changing proxy settings from SOCKS to HTTPS to HTTP but didn't worked for me.
* White listing Android Studio in Firewall for Windows 10. Again, didn't worked for me.
* Deleting ```.gradle``` folder etc.
* Tried to get username and password for maven repo from system variables, but didn't work.
* Tried to get username and password for maven repo from ```gradle.properties``` file but didn't work.
* Tried to put username and password for maven repo directly in the ```build.gradle``` file (which is highly not recommended) but still didn't work.

After 2-3 hours of troubleshooting and debugging, I found out that problem is not with the maven URL. The URL works great in browsers, the problem was Android Studio was not properly authenticating maven repo in right way as I missed a thing to put.

# Solution
So, here's the solution which worked for me.

First, you need to add your login credentials in ```gradle.properties``` file. **Please note that this is not recommended to share your credentials this way. I am looking for more optimal and secure solution. If you have any way, do let me know.**

```groovy
mavenUser = MY_USER_NAME
mavenPassword = MY_PASSWORD
```
