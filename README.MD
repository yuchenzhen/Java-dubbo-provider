
### 服务提供方建立 Provider

1.0 使用Spring Boot 建立一个Spring 微服务 (可以参考)[https://yuchenzhen.github.io/2018/08/24/SpringBoot-IDE-initial/]

1.1 建立一个 Interface 

```java
// TestProviderService.java

package com.dubbo.learn.dubbo;

public interface TestProviderService {
    String Hello (String who);
}
```

1.2 实现这个TestProviderServiceImpl

```java
// TestProviderServiceImpl
package com.dubbo.learn.dubbo.impl;

import com.dubbo.learn.dubbo.TestProviderService;
import com.alibaba.dubbo.config.annotation.Service;

@Service(version="1.0.0")
public class TestProviderServiceImpl implements TestProviderService  {
    public String Hello(String who) {
        return "Hello world ! Dear Programer " + who ;
    }
}
```

1.3 启动程序主入口添加`@EnableDubbo`注解



```java
// ProviderApplication.java
package com.dubbo.learn;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import com.alibaba.dubbo.config.spring.context.annotation.EnableDubbo;

@SpringBootApplication
@EnableDubbo
public class ProviderApplication {

    public static void main(String[] args) {
        SpringApplication.run(ProviderApplication.class, args);
    }

}	
```

​	为了引入这个`@EnableDubbo`注解，需要我们引入`com.alibaba`的包

```xml
<!--pom.xml-->
<dependency>
  <groupId>com.alibaba.boot</groupId>
  <artifactId>dubbo-spring-boot-starter</artifactId> 
  <version>0.2.1.RELEASE</version>
</dependency>
<dependency>
  <groupId>com.alibaba</groupId>
  <artifactId>dubbo</artifactId>
  <version>2.6.5</version>
</dependency>
<dependency>
  <groupId>org.apache.curator</groupId>
  <artifactId>curator-framework</artifactId>
  <version>2.11.1</version>
</dependency>
```

​	`curator-framework`是dubbo所使用的消息中心`Zookeeper`所需要的包

​	`dubbo-spring-boot-starter`是 dubbo spring 的配置包

1.4 最后我们把dubbo 的启动配置写到`application.properties`里面

```properties
# application.properties
server.port = 8829
#
dubbo.application.name=provider
dubbo.registry.protocol=zookeeper
dubbo.registry.address=zookeeper://127.0.0.1:2181
dubbo.scan.base-packages=com.dubboo.learn
dubbo.protocol.name=dubbo
dubbo.protocol.port=20880
dubbo.consumer.check=false
```

整个项目结构如图

![](http://ww1.sinaimg.cn/large/6026faa6gy1g1ffq5j1mrj20a70jhab5.jpg)

然后，启动就好。

启动之后，通过 dubbo Admin 网页客户端可以看见Provider的接口在 Regitry 里面注册成功。



![](http://ww1.sinaimg.cn/large/6026faa6gy1g1ffrs83ubj227m0n8djl.jpg)