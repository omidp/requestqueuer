# requestqueuer
a POC to queue request calls to microservices with activemq artmis, spring boot, jms, eureka


please visit queuer here 

```
https://github.com/jedlab/springcloud-intro/tree/master/queuer
```

```
 @GetMapping("/pay")    
    public ResponseEntity<String> getUserList(HttpServletRequest req)
    {
//        return ResponseEntity.ok(userProxy.getUserList());
        qproxy.queue(new ApiValueHolder("user-service", "{\"username\" :\"omid\"}", "/api/v1/users", HttpMethod.POST));
        
        CompletableFuture.runAsync(()->{
            qproxy.queue(new ApiValueHolder("user-service", "{\"username\" :\"mehdi\"}", "/api/v1/users", HttpMethod.POST));
        });
        try
        {
            Thread.sleep(1000);
        }
        catch (InterruptedException e)
        {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
        qproxy.queue(new ApiValueHolder("user-service", "{\"username\" :\"aram\"}", "/api/v1/users", HttpMethod.POST));
        
        return ResponseEntity.ok("ok");
    }
```
