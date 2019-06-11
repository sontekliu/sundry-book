# HttpClient 超时说明

Apache HttpClient 是开发中最常用的工具，使用方便，但是对其细节了解的却是不多。本文是基于 HttpClient 4.5.8 版本的。

我们使用 HttpClient 可能经常会看到如下代码，但是能说清楚每个超时的含义吗？

```java
   RequestConfig requestConfig = RequestConfig.custom()
                .setSocketTimeout(1000)
                .setConnectionRequestTimeout(5000)
                .setConnectTimeout(5).build();
```

### 1. ConnectTimeout 连接超时

* setConnectTimeout 是指与目标主机建立连接超时
请看如下示例：

```java
    @Test
    public void testConnectionTimeout(){
        CloseableHttpClient client = HttpClients.createDefault();
        HttpGet httpGet = new HttpGet("http://www.baidu.com");
        RequestConfig requestConfig = RequestConfig.custom()
                    .setConnectTimeout(5).build();
        httpGet.setConfig(requestConfig);
        try {
            CloseableHttpResponse response = client.execute(httpGet);
            System.out.println(EntityUtils.toString(response.getEntity()));
        } catch (Exception e){
            e.printStackTrace();
        }
    }
```
访问 `http://www.baidu.com` 并设置超时时间为 5 毫秒或者更小的值(为了显示效果)，运行此代码，会出现如下异常：
```java
org.apache.http.conn.ConnectTimeoutException: Connect to www.baidu.com:80 [www.baidu.com/220.181.38.150, www.baidu.com/220.181.38.149] failed: connect timed out
```
也就是说在 5 毫秒的时间内不能与 `http://www.baidu.com` 建立连接，所以报了连接超时的异常。

### 2. SocketTimeout 等待数据的超时时间

* setSocketTimeout 等待数据的超时时间，与目标主机建立连接后，两个数据包之间不活动的最长时间

为了说明超时效果，分两种情况：  
2.1 第一种情况
服务端代码

```java
    @ResponseBody
    @RequestMapping(value = "testSocketTimeout", method = RequestMethod.GET)
    public String testSocketTimeout() throws InterruptedException {
        log.info("receive request ===============");
        // 为了演示业务执行的耗时时间 3 秒
        Thread.sleep(3000);
        return "testSocketTimeout";
    }
```
客户端代码：
```java
    @Test
    public void testSocketTimeout(){
        CloseableHttpClient client = HttpClients.createDefault();
        HttpGet httpGet = new HttpGet("http://192.168.1.100:8080/http/testSocketTimeout");
        RequestConfig requestConfig = RequestConfig.custom().setSocketTimeout(3500).build();
        httpGet.setConfig(requestConfig);
        try {
            CloseableHttpResponse response = client.execute(httpGet);
            System.out.println(EntityUtils.toString(response.getEntity()));
        } catch (Exception e){
            e.printStackTrace();
        }
    }
```
如果 SocketTimeout 的时间理论上设置大于 3000 毫秒就不会报错，如果是小于 3000 毫秒就会抛出异常，异常如下：
```java
java.net.SocketTimeoutException: Read timed out
```
也就是说在 3 秒的时间内，客户端没能从服务端获取任何数据。

2.2 第二种情况

服务端代码：

```java
    @ResponseBody
    @RequestMapping(value = "testSocketTimeout2", method = RequestMethod.GET)
    public void testSocketTimeout2(HttpServletResponse response) throws Exception {
        log.info("receive request ===============");
        for (int i=0;i<10;i++){
            response.getWriter().println("This is number : " + i);
            response.flushBuffer();
            // 每隔 800 毫秒向客户端发送数据,共发送 10 次
            Thread.sleep(800);
        }
    }
```
客户端代码

```java
    @Test
    public void testSocketTimeout2(){
        CloseableHttpClient client = HttpClients.createDefault();
        HttpGet httpGet = new HttpGet("http://192.168.1.100:8080/http/testSocketTimeout2");
        RequestConfig requestConfig = RequestConfig.custom().setSocketTimeout(1000).build(); 
        httpGet.setConfig(requestConfig);
        try {
            CloseableHttpResponse response = client.execute(httpGet);
            System.out.println(EntityUtils.toString(response.getEntity()));
        } catch (Exception e){
            e.printStackTrace();
        }
    }
```
如果 SocketTimeout 的时间理论上设置大于 800 毫秒就不会报错，如果是小于 800 毫秒就会抛出异常，异常如下：
```java
java.net.SocketTimeoutException: Read timed out
```
也就是说客户端在 800 毫秒左右就会收到服务端的数据。设置超时时间为 1000，所以不会超时。

### 3. ConnectionRequestTimeout 从连接池中取得连接的等待时间

* setConnectionRequestTimeout  从连接池中取得连接的等待时间

HttpClient 获取连接时是从连接池中获得的，如果连接池中的连接已满并且都处在连接中，如果等待某段时间获取
不到连接，则就会出现连接超时的情况。

客户端代码如下：

```java
    @Test
    public void testConnectionRequestTimeout() throws InterruptedException{
        PoolingHttpClientConnectionManager manager = new PoolingHttpClientConnectionManager();
        // 为了演示效果，设置连接池的最大连接数是 3 或者更少 (默认是20)
        manager.setMaxTotal(3);
        CloseableHttpClient client = HttpClients.custom().setConnectionManager(manager).build();
        // 使用线程将连接耗尽
        for (int i=0;i<10;i++){
            HttpGet httpGet = new HttpGet(URL + "http/testSocketTimeout");
            HttpThread thread = new HttpThread(client, httpGet);
            thread.start();
        }
        HttpGet httpGet = new HttpGet(URL + "http/testSocketTimeout");
        // 设置超时时间是 1000 毫秒
        RequestConfig requestConfig = RequestConfig.custom().setConnectionRequestTimeout(1000).build();
        httpGet.setConfig(requestConfig);
        try {
            CloseableHttpResponse response = client.execute(httpGet);
            System.out.println(EntityUtils.toString(response.getEntity()));
        } catch (Exception e){
            e.printStackTrace();
        }
    }
```
设置连接池最大连接数是 3，然后启用多个线程，将此连接进行耗尽，当再次发起调用时， 从连接池中获取不到连接，此时抛出异常：

```java
org.apache.http.conn.ConnectionPoolTimeoutException: Timeout waiting for connection from pool
```

附 HttpThread 代码
```java
public class HttpThread extends Thread{
    
    private CloseableHttpClient closeableHttpClient;
    private HttpGet httpGet;

    public HttpThread(CloseableHttpClient closeableHttpClient, HttpGet httpGet){
        this.closeableHttpClient = closeableHttpClient;
        this.httpGet = httpGet;
    }


    @Override
    public void run() {
        try {
            CloseableHttpResponse response = closeableHttpClient.execute(httpGet);
            System.out.println(EntityUtils.toString(response.getEntity()));
            EntityUtils.consume(response.getEntity());
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```






















