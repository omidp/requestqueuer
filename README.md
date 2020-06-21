# requestqueuer
a POC to queue request calls to microservices with activemq artmis, spring boot, jms, eureka


please visit queuer here 

```
https://github.com/jedlab/springcloud-intro/tree/master/queuer
```

```
@GetMapping("/pay")    
    @HystrixCommand(fallbackMethod="defaultList",
            commandProperties = {
                    @HystrixProperty(name="execution.isolation.strategy", value="SEMAPHORE")
                  })
    public ResponseEntity<String> getUserList(HttpServletRequest req)
    {
//        return ResponseEntity.ok(userProxy.getUserList());
        String queue2 = qproxy.queue(new ApiValueHolder("user-service", "{\"username\" :\"omid\"}", "/api/v1/users", HttpMethod.POST));
        log.info("omid rsp : {}", queue2);
        CompletableFuture.runAsync(()->{
            log.info("this operation takes 3 seconds, check artemis consumer");
            String queue = qproxy.queue(new ApiValueHolder("user-service", "{\"username\" :\"mehdi\"}", "/api/v1/users", HttpMethod.POST));
            log.info("mehdi rsp : {}", queue);
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
        String queue = qproxy.queue(new ApiValueHolder("user-service", "{\"username\" :\"aram\"}", "/api/v1/users", HttpMethod.POST));
        log.info("aram rsp : {}", queue);
        
        return ResponseEntity.ok("ok");
    }
```
