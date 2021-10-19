# 1.微服务架构概述

- 集中式系统（集中为一个项目）
- 分布式系统（拆分为多个项目）
- 微服务架构属于分布式架构的范畴
- 微服务是系统架构上的一种设计风格，将原本独立的系统拆分为多个小型服务，服务都在各自的进程中独立进行，服务之间通过HTTP的TESTful APL进行通信协作。

# 2.优缺点

优点：

- 每一个服务都很easy，仅仅关注于一个业务功能。
- 每一个微服务能够由不同的团队独立开发。
- 微服务是松散耦合的。
- 微服务能够通过不同的编程语言与工具进行开发。

缺点：

- **运维成本过高**
- **DevOps是必须的**
- **接口不匹配//服务依赖于彼此间的接口进行通信**
- **代码反复**
- **分布式系统的复杂性**
- **异步**
- **增加集群测试的难度**

# 3.微服务架构

整个微服务架构是由大量的技术框架和方案构成

| 服务基础开发   | Spring MVC、Spring、SpringBoot                               |
| -------------- | ------------------------------------------------------------ |
| 服务注册与发现 | Netflix 的 Eureka、Apache 的 ZooKeeper 等                    |
| 服务调用       | RPC 调用有阿里巴巴的 Dubbo，Rest 方式 调用有当当网 Dubbo 基础上扩展的 Dubbox、还有其他方式实现的 Rest，比如 Ribbon、Feign |
| 分布式配置管理 | RPC 调用有阿里巴巴的 Dubbo，Rest 方式 调用有当当网 Dubbo 基础上扩展的 Dubbox、还有其他方式实现的 Rest，比如 Ribbon、Feign |
| 负载均衡       | Ribbon                                                       |
| 服务熔断       | Ribbon                                                       |
| API 网关       | Zuul                                                         |
| 批量任务       | 当当网的 Elastic-Job、Linkedln 的 Azkaban                    |
| 服务跟踪       | 当当网的 Elastic-Job、Linkedln 的 Azkaban                    |

# 4.Spring Cloud

### 4.1Spring Cloud是什么

1. Spring Cloud 是一个一站式的开发分布式系统的框架，为开发者提供了一系 列的构建分布式系统的工具集； 
2. Spring Cloud 为开发人员提供了快速构建分布式系统中一些常见模式的工具 （比如：配置管理，服务发现，断路器，智能路由、微代理、控制总线、全局锁、 决策竞选、分布式会话和集群状态管理等）； 
3. 开发分布式系统都需要解决一系列共同关心的问题，而使用 Spring Cloud 可以快速地实现这些分布式开发共同关心的问题，并能方便地在任何分布式环境 中部署与运行。
4.  Spring Cloud 这个**一站式地分布式开发框架**，被近年来流行的“微服务”架 构所大力推崇，成为目前进行微服务架构的优先选择工具； 
5. Spring Cloud **基于 Spring Boot 框架构建微服务架构**



### 4.2Spring Cloud 与 Spring Boot 版本匹配关系 

| Finchley | 兼容 Spring Boot 2.0.x， 不兼容 Spring Boot 1.5.x |
| -------- | ------------------------------------------------- |
| Edgware  | 兼容 Spring Boot 1.5.x， 不兼容 Spring Boot 2.0.x |
| Dalston  | 兼容 Spring Boot 1.5.x， 不兼容 Spring Boot 2.0.x |
| Camden   | 兼容 Spring Boot 1.5.x， 不兼容 Spring Boot 2.0.x |
| Brixton  | 兼容 Spring Boot 1.5.x， 不兼容 Spring Boot 2.0.x |
| Angel    | 兼容 Spring Boot 1.5.x， 不兼容 Spring Boot 2.0.x |

Spring Cloud 并不是从 0 开始开发一整套微服务解决方案，而是集成各个开源 软件，构成一整套的微服务解决方案，这其中有非常著名的 Netflix 公司的开源 产品；

# 5.Spring Cloud 整体架构

![](https://gitee.com/telei/picgoimage/raw/master/img/image-20210719161008810.png)

1. Service Provider： 暴露服务的服务提供方。 
2. Service Consumer：调用远程服务的服务消费方。 
3. EureKa Server： 服务注册中心和服务发现中心。

# 6.服务注册中心 Eureka

- Spring Cloud 提供了多种服务注册与发现的实现方式，例如：Eureka、 Consul、Zookeeper
- Spring Cloud 支持得最好的是 Eureka，其次是 Consul，再次是 Zookeeper

### 6.1什么是服务注册？

服务注册：将服务所在主机、端口、版本号、通信协议等信息登记到注册中心上；

### 6.2什么是服务发现？

服务发现：服务消费者向注册中心请求已经登记的服务列表，然后得到某个服务 的主机、端口、版本号、通信协议等信息，从而实现对具体服务的调用；

### 6.3Eureka 是什么？

- Eureka 是一个服务治理组件，它主要包括服务注册和服务发现，主要用来搭建 服务注册中心。
- Eureka 是一个基于 REST 的服务，用来定位服务，进行中间层服务器的负载均 衡和故障转移；
- Eureka 采用了 C-S（客户端/服务端）的设计架构，也就是 Eureka 由两个组件 组成：Eureka 服务端和 Eureka 客户端

# 7.Eureka 与 Zookeeper 的比较 

著名的 CAP 理论指出，一个分布式系统不可能同时满足 C(一致性)、A(可用性) 和 P(分区容错性)。 由于分区容错性在是分布式系统中必须要保证的，因此我们只能在 A 和 C 之间 进行权衡，在此 Zookeeper 保证的是 CP, 而 Eureka 则是 AP。

### 7.1Zookeeper 保证 CP 

- 在 ZooKeeper 中，当 master 节点因为网络故障与其他节点失去联系时，剩余 节点会重新进行 leader 选举，但是问题在于，选举 leader 需要一定时间, 且选 举期间整个 ZooKeeper 集群都是不可用的，这就导致在选举期间注册服务瘫痪。 在云部署的环境下，因网络问题使得 ZooKeeper 集群失去 master 节点是大概 率事件，虽然服务最终能够恢复，但是在选举时间内导致服务注册长期不可用是 难以容忍的。 

### 7.2Eureka 保证 AP 

- Eureka 优先保证可用性，Eureka 各个节点是平等的，某几个节点挂掉不会影响 正常节点的工作，剩余的节点依然可以提供注册和查询服务。而 Eureka 的客户 端在向某个 Eureka 注册或时如果发现连接失败，则会自动切换至其它节点，只 要有一台 Eureka 还在，就能保证注册服务可用(保证可用性)，只不过查到的信 息可能不是最新的(不保证强一致性)。
-  Eureka 在网络故障导致部分节点失去联系的情况下，只要有一个节点可用， 那么注册和查询服务就可以正常使用，而不会像 zookeeper 那样使整个注册服 务瘫痪，Eureka 优先保证了可用性

# 8.Eureka 客户端是什么？

- Eureka 客户端是一个 Java 客户端，用来连接 Eureka 服务端，与服务端进行交 互、负载均衡，服务的故障切换等；
- 注意springcloud与springboot兼容版本关系！！！

~~~xml
<!-- 添加springcloud依赖管理-->
    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>org.springframework.cloud</groupId>
                <artifactId>spring-cloud-dependencies</artifactId>
                <version>${spring.cloud-version}</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
        </dependencies>
    </dependencyManagement>

<!--Spring Cloud 的 eureka-server 起步依赖-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
        </dependency>

<!--SpringCloud 集成 eureka 客户端的起步依赖-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
        </dependency>
~~~



# 9.Ribbon 是什么？

- Ribbon 是一个基于 HTTP 和 TCP 的客户端负载均衡器，当使用 Ribbon 对服 务进行访问的时候，它会扩展 Eureka 客户端的服务发现功能，实现从 Eureka 注册中心中获取服务端列表，并通过 Eureka 客户端来确定服务端是否己经启动。 Ribbon 在 Eureka 客户端服务发现的基础上，实现了对服务实例的选择策略， 从而实现对服务的负载均衡消费。

# 10.Eureka 注册中心高可用集群

~~~
	在微服务架构的这种分布式系统中，我们要充分考虑各个微服务组件的高可用性
问题，不能有单点故障，由于注册中心 eureka 本身也是一个服务，如果它只有
一个节点，那么它有可能发生故障，这样我们就不能注册与查询服务了，所以我
们需要一个高可用的服务注册中心，这就需要通过注册中心集群来解决。
	eureka 服务注册中心它本身也是一个服务，它也可以看做是一个提供者，又可
以看做是一个消费者，我们之前通过配置：
eureka.client.register-with-eureka=false 让注册中心不注册自己，但是我们
可以向其他注册中心注册自己；
	Eureka Server 的高可用实际上就是将自己作为服务向其他服务注册中心注册
自己，这样就会形成一组互相注册的服务注册中心，进而实现服务清单的互相同
步，往注册中心 A 上注册的服务，可以被复制同步到注册中心 B 上，所以从任
何一台注册中心上都能查询到已经注册的服务，从而达到高可用的效果。
~~~

![](https://gitee.com/telei/picgoimage/raw/master/img/image-20210719211734071.png)

### 10.1实现

![](https://gitee.com/telei/picgoimage/raw/master/img/image-20210719222335441.png)

在 8610 的配置文件中，让它的 service-url 指向 8611，在 8611 的配置文件 中让它的 service-url 指向 8610

~~~java
server:
  port: 8610

eureka:
  instance:
    hostname: eureka8610
  client:
    register-with-eureka: false
    fetch-registry: false
    service-url:
      defaultZone: http://eureka8611:8611/eureka
~~~

~~~java
server:
  port: 8611

eureka:
  instance:
    hostname: eureka8611
  client:
    register-with-eureka: false
    fetch-registry: false
    service-url:
      defaultZone: http://eureka8610:8610/eureka
~~~

由于 8610 和 8611 互相指向对方，实际上我们构建了一个双节点的服务注册 中心集群

然后在本地 hosts 文件配置：C:\Windows\System32\drivers\etc 修改hosts文件

~~~
127.0.0.1 eureka8610
127.0.0.1 eureka8611
127.0.0.1 ${eureka-instance-hostname}

如果没有权限修改
管理员权限打开power shell，
cd C:\Windows\System32\drivers\etc，
notepad hosts 在文末添加即可
~~~

在启动类加载对应的配置文件

environment中的Program Arguments 中配置： 

1. --spring.profiles.active=eureka8610 
2. --spring.profiles.active=eureka8611
3. --spring.profiles.active=对应的application-？.yml 的？

在provider和consumer中修改defaulturl

1. ```yml
   defaultZone: http://eureka8610:8610/eureka, http://eureka8611:8611/eureka
   ```

# 11.Eureka自我保护机制

~~~
	自我保护机制是 Eureka 注册中心的重要特性，当 Eureka 注册中心进入自我保
护模式时，在 Eureka Server 首页会输出如下警告信息：
	EMERGENCY! EUREKA MAY BE INCORRECTLY CLAIMING INSTANCES ARE UP WHEN THEY'RE NOT. 
RENEWALS ARE LESSER THAN THRESHOLD AND HENCE THE INSTANCES ARE NOT BEING EXPIRED 
JUST TO BE SAFE.
~~~

~~~
	在没有 Eureka 自我保护的情况下，如果 Eureka Server 在一定时间内没有接收
到某个微服务实例的心跳，Eureka Server 将会注销该实例
	自我保护模式解决：
当 Eureka Server 节点在短
时间内丢失过多客户端时（可能发生了网络分区故障），那么就会把这个微服务
节点进行保护。一旦进入自我保护模式，Eureka Server 就会保护服务注册表中
的信息，不删除服务注册表中的数据（也就是不会注销任何微服务）。当网络故
障恢复后，该 Eureka Server 节点会再自动退出自我保护模式。
~~~

自我保护模式是一种**<u>*应对网络异常的安全保护措施*</u>**，它的架构哲学是宁可同时保留所有微服务（健康的微服务和不健康的微服务都会保留），也不盲目注 销任何健康的微服务，使用自我保护模式，可以让 Eureka 集群更加的健壮、稳 定。

~~~yml
#服务器端配置：禁用自我保护机制
eureka: 
	server: 
		enable-self-preservation: false
#服务器端配置：		
eureka: 
	instance: 
	#每间隔 2s，向服务端发送一次心跳，证明自己依然"存活"
	lease-renewal-interval-in-seconds: 2
	#告诉服务端，如果我 10s 之内没有给你发心跳，就代表我故障了，将我踢出掉
	lease-expiration-duration-in-seconds: 10
~~~

# 12.客户端负载均衡 Ribbon

- 负载均衡是指将一个请求均匀地分摊到不同的节点单元上执行，负 载均和分为硬件负载均衡和软件负载均衡
- 硬件负载均衡：比如 F5、深信服、Array 等；
-  软件负载均衡：比如 Nginx、LVS、HAProxy 等；
- Ribbon 是 Netflix 发布的开源项目 主要功能是提供客户端的软件负载均衡算 法，是一个基于 HTTP 和 TCP 的客户端负载均衡工具。
- Spring Cloud 对 Ribbon 做了二次封装，可以让我们使用 **<u>RestTemplate</u>** 的服 务请求，自动转换成客户端负载均衡的服务调用
- Ribbon 支持多种负载均衡算法，还支持自定义的负载均衡算法。

### 12.1Ribbon 与 Nginx 的区别

- Ribbon 是客户端的负载均衡工具，而客户端负载均衡和服务端负载均衡最大的 区别在于**<u>*服务清单所存储的位置不同*</u>**，在客户端负载均衡中，所有客户端节点下 的服务端清单，需要自己从服务注册中心上获取，比如 Eureka 服务注册中心。 同服务端负载均衡的架构类似，在客户端负载均衡中也需要心跳去维护服务端清 单的健康性，只是这个步骤需要与服务注册中心配合完成。在 Spring Cloud 中， 由于 Spring Cloud 对 Ribbon 做了二次封装，所以默认会创建针对 Ribbon 的 自动化整合配置；
- 在 Spring Cloud 中，Ribbon 主要与 RestTemplate 对象配合起来使用，Ribbon 会自动化配置 RestTemplate 对象，通过**<u>*@LoadBalanced*</u>** 开启 RestTemplate 对象调用时的负载均衡。
- ![](https://gitee.com/telei/picgoimage/raw/master/img/image-20210720094030581.png)

### 12.2 Ribbon 实现客户端负载均衡

- 由于 Spring Cloud Ribbon 的封装， 我们在微服务架构中使用客户端负载均衡 调用非常简单， 只需要如下两步：
- 1. 启动多个服务提供者实例并注册到一个服务注册中心或是服务注册中心集群
  2. 服务消费者通过被**<u>*＠LoadBalanced*</u>** 注解修饰过的 RestTemplate 来调用服 务提供者
  3. <img src="https://gitee.com/telei/picgoimage/raw/master/img/image-20210720094248950.png" style="zoom:50%;" />
  4. 多个服务提供者以相同名称注册到eureka中![](https://gitee.com/telei/picgoimage/raw/master/img/image-20210720100041881.png)
  5. 调用接口实现负载均衡

### 12.3负载均衡策略

- <img src="https://gitee.com/telei/picgoimage/raw/master/img/image-20210720100636271.png" style="zoom:80%;" />

- | RandomRule                | 随机                                                         |
  | ------------------------- | ------------------------------------------------------------ |
  | RoundRobinRule            | 轮询                                                         |
  | AvailabilityFilteringRule | 先过滤掉由于多次访问故障的服务，以及并 发连接数超过阈值的服务，然后对剩下的服 务按照轮询策略进行访问 |
  | WeightedResponseTimeRule  | 根据平均响应时间计算所有服务的权重，响 应时间越快服务权重就越大被选中的概率即 越高，如果服务刚启动时统计信息不足，则 使用 RoundRobinRule 策略，待统计信息足够 会切换到该 WeightedResponseTimeRule 策 略； |
  | WeightedResponseTimeRule  | 根据平均响应时间计算所有服务的权重，响 应时间越快服务权重就越大被选中的概率即 越高，如果服务刚启动时统计信息不足，则 使用 RoundRobinRule 策略，待统计信息足够 会切换到该 WeightedResponseTimeRule 策 略； |
  | BestAvailableRule         | 根据平均响应时间计算所有服务的权重，响 应时间越快服务权重就越大被选中的概率即 越高，如果服务刚启动时统计信息不足，则 使用 RoundRobinRule 策略，待统计信息足够 会切换到该 WeightedResponseTimeRule 策 略； |
  | BestAvailableRule         | 根据平均响应时间计算所有服务的权重，响 应时间越快服务权重就越大被选中的概率即 越高，如果服务刚启动时统计信息不足，则 使用 RoundRobinRule 策略，待统计信息足够 会切换到该 WeightedResponseTimeRule 策 略； |


# 13.RestTemplate常见请求方式

### 13.1 GET 请求

#### 13.1.1getForEntity

~~~java
		/**
         * getForEntity(url,responseType)
         * url:服务提供者提供的接口地址、通过服务名调用而不是服务地址
         * responseType:返回的 body 类型
         */
ResponseEntity<String> entity = restTemplate.getForEntity("http://SPRINGCLOUD-SERVICE-PROVIDER/service/test", String.class);
        String body = entity.getBody();
        HttpStatus statusCode = entity.getStatusCode();
        int statusCodeValue = entity.getStatusCodeValue();
        HttpHeaders headers = entity.getHeaders();
~~~

#### 13.1.2getForObject

- 在 getForEntity 基础上进行了再次封装，可以将 http 的响应体 body 信息转化成指定的对象

- 不需要返回响应中的其他信息，只需要 body 体信息的时候，可以 使用这个更方便

- ~~~java
  String object = restTemplate.getForObject("http://SPRINGCLOUD-SERVICE-PROVIDER/service/test", String.class);
          
  ~~~

### 13.2POST请求

~~~java
restTemplate.postForObject()
restTemplate.postForEntity()
restTemplate.postForLocation()
~~~

### 13.3PUT请求

~~~java
restTemplate.put()
~~~

### 13.4DELETE请求

~~~java
restTemplate.delete()
~~~

# 14服务熔断 Hystrix

- 在微服务架构中，我们是将一个单体应用拆分成多个服务单元，各个服务单元之
- 间通过注册中心彼此发现和消费对方提供的服务，每个服务单元都是单独部署，
  在各自的服务进程中运行，服务之间通过远程调用实现信息交互，那么当某个服
  务的响应太慢或者故障，又或者因为网络波动或故障，则会造成调用者延迟或调
  用失败，当大量请求到达，则会造成请求的堆积，导致调用者的线程挂起，从而
  引发调用者也无法响应，调用者也发生故障
- ​	微服务架构中引入了一种叫熔断器的服务保护机制,熔断器也有叫断路器
- ​	微服务架构中的熔断器，就是当被调用方没有响应，调用方直接返回一个错误响
  应即可，而不是长时间的等待，这样避免调用时因为等待而线程一直得不到释放，
  避免故障在分布式系统间蔓延；
- ​	Hystrix 具备服务降级、服务熔断、线程和信号隔离、请求缓存、请求合并
  以及服务监控等强大功能

### 14.1Hystrix 快速入门

服务消费者中添加熔断器

1. 添加依赖 

2. ~~~xml
   <!--Spring Cloud 熔断器起步依赖-->
   <dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-hystrix</artifactId>
    <version>1.4.4.RELEASE</version>
   </dependency>
   ~~~

3. 在入口类中使用@EnableCircuitBreaker 注解开启断路器功能

4. 在调用远程服务的方法上添加注解 

5. @HystrixCommand(fallbackMethod="error") error为发生错误时调用的方法

6. ~~~java
       @RequestMapping("/web/hystrix")
       @HystrixCommand(fallbackMethod = "error")
       public String hystrix(){
           return restTemplate.getForEntity
                   ("http://SPRINGCLOUD-SERVICE-PROVIDER/service/test",String.class)
                   .getBody();
       }
   	
   	//服务调用错误处理
       public String error(){
           return "hystrix--error";
       }
   ~~~

7. ~~~java
   	@SpringCloudApplication 
   可代替下列三个注解
           @SpringBootApplication
           @EnableEurekaClient//开启eureka客户端支持
           @EnableCircuitBreaker//开启断路器功能
   ----------------------------------------------------------    
   @Target({ElementType.TYPE})
   @Retention(RetentionPolicy.RUNTIME)
   @Documented
   @Inherited
   @SpringBootApplication
   @EnableDiscoveryClient //与 @EnableEurekaClient 相似 发现注册服务中心
   @EnableCircuitBreaker
   public @interface SpringCloudApplication {
   }    
   ~~~

8. ```java
   /**
    * 修改 hystrix 的默认超时时间.
    * hystrix 默认超时时间是 1000 毫秒
    * @return
    */
   @RequestMapping("/web/hystrix1")
   @HystrixCommand(fallbackMethod = "error", commandProperties = {
           @HystrixProperty(name = "execution.isolation.thread.timeoutInMilliseconds",
                   value = "1500")})
   public String hystrix1(){
       return restTemplate.getForEntity
               ("http://SPRINGCLOUD-SERVICE-PROVIDER/service/test",String.class)
               .getBody();
   }
   ```

### 14.2Hystrix 的服务降级

- `有了服务的熔断，随之就会有服务的降级，所谓服务降级，就是当某个服务熔断 之后，服务端提供的服务将不再被调用，此时由客户端自己准备一个本地的 fallback 回调，返回一个默认值来代表服务端的返回； 这种做法，虽然不能得到正确的返回结果，但至少保证了服务的可用，比直接抛 出错误或服务不可用要好很多`

### 14.3Hystrix异常处理

~~~java
//要在服务降级方法中添加一个 Throwable 类型的参数就能够获取到抛出的异常的类型
public String error(Throwable throwable) {
     System.out.println(throwable.getMessage());
     return "error";
}
~~~

- `如果远程服务有一个异常抛出后我们不希望进入到服务降级方法中去处理，而是 直接将异常抛给用户以在@HystrixCommand 注解中添加忽略异常`

~~~java
//ignoreExceptions不触发熔断异常
@HystrixCommand(fallbackMethod="error", ignoreExceptions = Exception.class)
~~~

#### 14.3.1自定义异常处理

~~~java
import com.netflix.hystrix.HystrixCommand;
import org.springframework.web.client.RestTemplate;

/**
 * hystrix自定义请求服务熔断
 */
public class MyHystrixHandler extends HystrixCommand<String> {

    private RestTemplate restTemplate;

    /**
     * 构造对象
     * @param setter
     * @param restTemplate
     */
    public MyHystrixHandler(Setter setter, RestTemplate restTemplate){
        super(setter);
        this.restTemplate = restTemplate;
    }

    @Override
    protected String run() throws Exception {
        return restTemplate.getForEntity
                ("http://SPRINGCLOUD-SERVICE-PROVIDER/service/test",String.class)
                .getBody();
    }

    /**
     * 实现服务熔断/降级逻辑
     * @return
     */
    @Override
    protected String getFallback() {
        return "自定义服务熔断处理";
    }
}
----------------------------------------
//请求接口中
/**
     * 测试自定义熔断
     * @return
     */
public String hystrix2(){
        MyHystrixHandler myHystrixHandler = new MyHystrixHandler(com.netflix.hystrix.HystrixCommand.Setter.withGroupKey(HystrixCommandGroupKey.Factory.asKey(""))
                , restTemplate);

        //execute同步调用(等待返回结果后在往下走)
//        String result = myHystrixHandler.execute();

        String result = null;
        try {
            //queue异步调用 不会立刻有结果
            Future<String> future = myHystrixHandler.queue();
            //阻塞的方法，直到拿到结果
            result = future.get();
        } catch (InterruptedException e) {
            e.printStackTrace();
        } catch (ExecutionException e) {
            e.printStackTrace();
        }

        return result;
    }  
~~~

# 15同步请求和异步请求的区别

1. `同步是指：发送方发出数据后，等接收方发回响应以后才发下一个数据包的通讯方式。`
2. `异步是指：发送方发出数据后，不等接收方发回响应，接着发送下个数据包的通讯方式。`

**<u>*同步通信方式与异步通信的概念*</u>**

1. `同步通信方式要求通信双方以相同的时钟频率进行，而且准确协调，通过共享一个单个时钟或定时脉冲源保证发送方和接收方的准确同步，效率较高；`

2. `异步通信方式不要求双方同步，收发方可采用各自的时钟源，双方遵循异步的通信协议，以字符为数据传输单位，发送方传送字符的时间间隔不确定，发送效率比同步传送效率低`

从使用者的观点来看：

- `同步——使用者通过单个线程调用服务；该线程发送请求，在服务运行时阻塞，并且等待响应。如果使用者在服务运行的过程中阻塞时崩溃了，当它重新启动时，将无法重新连接到正在进行的调用，所以响应丢失了`
- `异步——使用者通过两个线程调用服务；一个线程发送请求，而另一个单独的线程接收响应。如果使用者在发送了请求之后等待响应时崩溃了，当它重新启动时，可以继续等待响应，所以响应不会丢失`

# 16Hystrix 仪表盘监控

### 16.1hystrix服务类

1. 搭建新的springboot项目

2. ~~~xml
   <!-->添加依赖<-->
   <dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-hystrix-dashboard</artifactId>
    <version>1.4.5.RELEASE</version>
   </dependency>
   ~~~

3. 入口类添加注解：@EnableHystrixDashboard 

4. 配置启动端口号，启动服务打开 http://localhost:端口号/hystrix

5. <img src="https://gitee.com/telei/picgoimage/raw/master/img/image-20210721162955885.png" style="zoom:80%;" />

### 16.2消费者搭建

1. ~~~xml
   <!--Spring Cloud 熔断器起步依赖-->
           <dependency>
               <groupId>org.springframework.cloud</groupId>
               <artifactId>spring-cloud-starter-hystrix</artifactId>
               <version>1.4.5.RELEASE</version>
           </dependency>
   
           <!-- spring boot服务监控-->
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-starter-actuator</artifactId>
           </dependency>
   ~~~

2. 配置文件中添加服务监控端点的访问权限

3. ~~~yml
   #springboot的监控端点访问权限，*表示所有的访问端点都允许
   management:
     endpoints:
       web:
         exposure:
           include: '*'
   ~~~

4. 启动消费者服务，需要先访问带有hystrix熔断器的接口 然后再访问

5. http://localhost:端口号/actuator/hystrix.stream

6. 在hystrixDashboard中添加消费者地址，进入hystrix监控界面

7. <img src="https://gitee.com/telei/picgoimage/raw/master/img/image-20210721163818553.png" style="zoom:80%;" />

# 17声明式服务消费 Feign

- `Feign 是 Netflix 公司开发的一个声明式的 REST 调用客户端`
- `Spring Cloud Feign 对 Ribbon 负载均衡、Hystrix 服务熔断进行简化，在其基础上进行了进一步的 封装，不仅在配置上大大简化了开发工作，同时还提供了一种声明式的 Web 服 务客户端定义方式`

### 17.1使用 Feign 实现消费者

1. 新建speingboot项目

2. ~~~xml
   <!-- 添加依赖-->
   		<dependency>
               <groupId>org.springframework.cloud</groupId>
               <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
           </dependency>
   
           <dependency>
               <groupId>org.springframework.cloud</groupId>
               <artifactId>spring-cloud-starter-feign</artifactId>
               <version>1.4.5.RELEASE</version>
           </dependency>
   
           <dependency>
               <groupId>org.springframework.cloud</groupId>
               <artifactId>spring-cloud-starter-hystrix</artifactId>
               <version>1.4.5.RELEASE</version>
           </dependency>
   ~~~

3. 启动类添加 @EnableFeignClients  注解 开启fegin的支持功能

4. 定义一个 HelloService 接口，通过@FeignClient 注解来指定服务名称定义一个 HelloService 接口，通过@FeignClient 注解来指定服务名称

5. ~~~java
   import org.springframework.cloud.openfeign.FeignClient;
   import org.springframework.web.bind.annotation.RequestMapping;
   
   /**
    * @Author Lucien
    * @Date 2021/7/26
    */
   @FeignClient("SPRINGCLOUD-SERVICE-PROVIDER")
   public interface HelloService {
   
       @RequestMapping("/service/hello")
       public String hello();
   
   }
   
   ~~~

6. 在提供者添加接口

7. ~~~java
   @GetMapping("/service/hello")
       public String hello(){
           System.out.println("提供者1......");
           return "HELLO";
       }
   ~~~

8. fegin消费者添加控制器 接口

9. ~~~java
   import com.lucien.springcloud.service.HelloService;
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.web.bind.annotation.RequestMapping;
   import org.springframework.web.bind.annotation.RestController;
   
   /**
    * @Author Lucien
    * @Date 2021/7/26
    */
   @RestController
   public class HelloConttler {
   
       @Autowired
       private HelloService helloService;
   
       @RequestMapping("/web/hello")
       public String hello(){
           return helloService.hello();
       }
   }
   
   ~~~

10. application配置中添加连接eureka注册中心

11. 访问fegin消费者 接口地址查看结果

### 17.2使用 Feign 实现消费者的测试

- 负载均衡

  - ~~~
    在 Spring Cloud 下，使用 Feign 也是直接可以实现负载均衡的，定义一个注解
    有@FeignClient 注解的接口，然后使用@RequestMapping 注解到方法上映
    射远程的 REST 服务，此方法也是做好负责均衡配置的
    ~~~



- 服务熔断

  1. 在 application.yml 文件开启 hystrix 功能

  2. ~~~yml
     #开启hystrix功能
     feign:
       hystrix:
         enabled: true
     ~~~

  3. 指定熔断回调逻辑

  4. ~~~java
     import org.springframework.cloud.openfeign.FeignClient;
     import org.springframework.web.bind.annotation.RequestMapping;
     
     /**
      * @Author Lucien
      * @Date 2021/7/26
      *  "" 内大小写均可
      */
     //@FeignClient("SPRINGCLOUD-SERVICE-PROVIDER")
     //指定熔断回调逻辑
     @FeignClient(name = "SPRINGCLOUD-SERVICE-PROVIDER",fallback = MyFallBack.class)
     public interface HelloService {
     
         @RequestMapping("/service/hello")
         public String hello();
     }
     ---------------------------------------------------------------
         
     import org.springframework.cloud.openfeign.FeignClient;
     import org.springframework.stereotype.Component;
     
     /**
      * @Author Lucien
      * @Date 2021/7/26
      */
     @Component
     public class MyFallBack implements HelloService{
         @Override
         public String hello() {
            return "发生了异常";
         }
     }
     
     
     ~~~

### 17.3服务熔断获取异常信息

1. 修改接口注解

2. ~~~java
   @FeignClient(name = "SPRINGCLOUD-SERVICE-PROVIDER",fallbackFactory = MyFallbackFactory.class)
   ~~~

3. ~~~Java
   import com.lucien.springcloud.service.HelloService;
   import feign.hystrix.FallbackFactory;
   import org.springframework.stereotype.Component;
   
   /**
    * @Author Lucien
    * @Date 2021/7/26
    * 服务熔断获取异常信息
    */
   @Component
   public class MyFallbackFactory implements FallbackFactory<HelloService> {
   
   
       @Override
       public HelloService create(Throwable throwable) {
           return new HelloService() {
               @Override
               public String hello() {
                   System.out.println(throwable.getMessage());
                   return throwable.getMessage();
               }
           };
       }
   }
   ~~~

# 18API网关Zuul

- `一个安检站 一样，所有外部的请求都需要经过它的调度与过滤，然后 API 网关来实现请 求路由、负载均衡、权限验证等功能`

### 18.1使用 Zuul 构建 API 网关

1. 创建是springboot项目 添加依赖

2. ~~~xml
   <!--添加 spring cloud 的 zuul 的起步依赖-->
   <dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-zuul</artifactId>
   </dependency>
   <!--添加 spring cloud 的 eureka 的客户端依赖-->
   <dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
   </dependency>
   ~~~

3. 启动类添加zuul支持 @EnableZuulProxy 

4. 编写配置文件

5. ~~~yml
   server:
     port: 8080
   
   spring:
     application:
       name: springcloud-api-gateway
   
   #配置路由规则
   #路由规则中配置的 gate-way 是路由的名字，可以任意定义，但是一组 path 和serviceId 映射关系的路由名要相同
   zuul:
     routes:
       gate-way:
         path: /gate-way/**
         service-id: springcloud-service-fegin
   
   #配置 API 网关到注册中心上，API 网关也将作为一个服务注册到 eureka-server 上
   eureka:
     client:
       service-url:
         defaultZone: http://eureka8610:8610/eureka, http://eureka8611:8611/eureka
   ~~~

6. 请求接口 携带path里的参数

7. 列如：

   1. 访问：http://localhost:8080/gate-way/web/test
   2. 实际访问：http://localhost:8905/web/test

### 18.2用 Zuul 进行请求过滤

- 定义一个过滤器类并继承自 ZuulFilter，并将该 Filter 作为一个 Bean

- ~~~java
  package com.lucien.springcloud.filter;
  
  import com.netflix.zuul.ZuulFilter;
  import com.netflix.zuul.context.RequestContext;
  import com.netflix.zuul.exception.ZuulException;
  import org.springframework.stereotype.Component;
  
  import javax.servlet.http.HttpServletRequest;
  
  /**
   * @Author Lucien
   * @Date 2021/7/26
   * Zuul 进行请求过滤
   */
  @Component
  public class AuthFilter extends ZuulFilter {
  
      /**
       * filterType 方法的返回值为过滤器的类型，过滤器的类型决定了过滤器在
       * 哪个生命周期执行，pre 表示在路由之前执行过滤器，其他值还有 post、error、
       * route 和 static，当然也可以自定义
       * @return
       */
      @Override
      public String filterType() {
          return "pre";
      }
  
      /**
       * filterOrder 方法表示过滤器的执行顺序，当过滤器很多时，我们可以通过
       * 该方法的返回值来指定过滤器的执行顺序
       * @return
       */
      @Override
      public int filterOrder() {
          return 0;
      }
  
      /**
       * shouldFilter 方法用来判断过滤器是否执行，
       * true 表示执行，false 表示不执行
       * @return
       */
      @Override
      public boolean shouldFilter() {
          return true;
      }
  
      /**
       * run 方法则表示过滤的具体逻辑，如果请求地址中携带了 token 参数的话，
       * 则认为是合法请求，否则为非法请求，如果是非法请求的话，首先设置
       * ctx.setSendZuulResponse(false); 表示不对该请求进行路由，然后设置响应码
       * 和响应值。这个 run 方法的返回值目前暂时没有任何意义，可以返回任意值
       * @return
       * @throws ZuulException
       */
      @Override
      public Object run() throws ZuulException {
          RequestContext ctx = RequestContext.getCurrentContext();
          HttpServletRequest request = ctx.getRequest();
          String token = request.getParameter("token");
          if (token == null) {
              ctx.setSendZuulResponse(false);
              ctx.setResponseStatusCode(401);
  
              ctx.addZuulResponseHeader("content-type","text/html;charset=utf-8");
              ctx.setResponseBody("非法访问");
          }
          return null;
      }
  }
  
  ~~~

### 18.3zuul其他配置

~~~yml
#配置路由规则
zuul:
  routes:
    gate-way:
      path: /gate-way/**
      service-id: springcloud-service-fegin
  #忽略掉服务提供者的默认规则
  ignored-services: springcloud-service-provider
  #忽略掉某一些接口路径
  ignored-patterns: /**/test/**
  #配置网关路由前缀
  prefix: /gateway
~~~

- **`一般情况下 API 网关只是作为各个微服务的统一入口，但是有时候我们可能 也需要在 API 网关服务上做一些特殊的业务逻辑处理，那么我们可以让请求到 达 API 网关后，再转发给自己本身，由 API 网关自己来处理`**

- ~~~java
  //添加拦截控制器
  import org.springframework.web.bind.annotation.RequestMapping;
  import org.springframework.web.bind.annotation.RestController;
  
  /**
   * @Author Lucien
   * @Date 2021/7/27
   */
  @RestController
  public class GateWayController {
  
      @RequestMapping("/api/local")
      public String hello() {
          //业务逻辑处理
          return "exec the api gateway.";
      }
  
  }
  
  ~~~

- ~~~yml
  server:
    port: 8080
  
  spring:
    application:
      name: springcloud-api-gateway
  
  #配置路由规则
  zuul:
    routes:
      gateway:
        path: /gateway/**
        url: forward:/api/local
  
  #配置 API 网关到注册中心上，API 网关也将作为一个服务注册到 eureka-server 上
  eureka:
    client:
      service-url:
        defaultZone: http://eureka8610:8610/eureka, http://eureka8611:8611/eureka
  ~~~

- 访问 localhost:8080/gateway 跳转到GateWayController的hello方法中

### 18.4Zuul 的异常处理

- zuul请求的生命周期图![](https://gitee.com/telei/picgoimage/raw/master/img/image-20210727163309195.png)

- ~~~
  	正常情况下所有的请求都是按照 pre、route、post 的顺序来执行，然后由 post返回 response
  	在 pre 阶段，如果有自定义的过滤器则执行自定义的过滤器
  	pre、routing、post 的任意一个阶段如果抛异常了，则执行 error 过滤器
  ~~~

- **<u>`两种方式统一处理异常`</u>**

  1. ~~~java
     //1.禁用 zuul 默认的异常处理 SendErrorFilter 过滤器，然后自定义我们自己的Errorfilter 过滤器
     /**
     #配置路由规则
     zuul:
       SendErrorFilter:
         error:
           disable: true
     */
     
     import com.netflix.zuul.ZuulFilter;
     import com.netflix.zuul.context.RequestContext;
     import com.netflix.zuul.exception.ZuulException;
     import org.slf4j.Logger;
     import org.slf4j.LoggerFactory;
     import org.springframework.stereotype.Component;
     import org.springframework.util.ReflectionUtils;
     
     import javax.servlet.http.HttpServletResponse;
     import java.io.IOException;
     import java.io.PrintWriter;
     
     /**
      * @Author Lucien
      * @Date 2021/7/27
      */
     @Component
     public class ErrorFilter extends ZuulFilter {
     
         private static final Logger log = LoggerFactory.getLogger(ErrorFilter.class);
     
         @Override
         public String filterType() {
             return "error";
         }
     
         @Override
         public int filterOrder() {
             return 1;
         }
     
         @Override
         public boolean shouldFilter() {
             return true;
         }
     
         @Override
         public Object run() throws ZuulException {
             try {
                 RequestContext context = RequestContext.getCurrentContext();
                 ZuulException exception = (ZuulException) context.getThrowable();
                 log.error("进入系统异常拦截", exception);
                 HttpServletResponse response = context.getResponse();
                 response.setContentType("application/json; charset=utf8");
                 response.setStatus(exception.nStatusCode);
                 PrintWriter writer = null;
                 try {
                     writer = response.getWriter();
                     writer.print("{code:" + exception.nStatusCode + ",message:\"" +
                             exception.getMessage() + "\"}");
                 } catch (IOException e) {
                     e.printStackTrace();
                 } finally {
                     if (writer != null) {
                         writer.close();
                     }
                 }
             } catch (Exception var5) {
                 ReflectionUtils.rethrowRuntimeException(var5);
     
             }
             return null;
         }
     }
     
     ~~~

  2. ~~~java
     //自定义全局 error 错误页面
     
     import com.netflix.zuul.context.RequestContext;
     import com.netflix.zuul.exception.ZuulException;
     import org.springframework.boot.web.servlet.error.ErrorController;
     import org.springframework.web.bind.annotation.RequestMapping;
     import org.springframework.web.bind.annotation.RestController;
     
     /**
      * @Author Lucien
      * @Date 2021/7/27
      */
     @RestController
     public class ErrorHandlerController implements ErrorController {
     
         /**
          * 出异常后进入该方法，交由下面的方法处理
          */
         @Override
         public String getErrorPath() {
             return "/error";
         }
     
         @RequestMapping("/error")
         public Object error(){
             RequestContext ctx = RequestContext.getCurrentContext();
             ZuulException exception = (ZuulException)ctx.getThrowable();
             return exception.nStatusCode + "--" + exception.getMessage();
         }
     
     }
     
     ~~~

# 19Spring Cloud Config

- `在分布式系统中，尤其是当我们的分布式项目越来越多，每个项目都有自己的配 置文件，对配置文件的统一管理就成了一种需要，而 Spring Cloud Config 就 提供了对各个分布式项目配置文件的统一管理支持Spring Cloud Config 也叫 分布式配置中心`
- `Spring Cloud Config 是一个解决分布式系统的配置管理方案。它包含 Client和 Server 两个部分，Server 提供配置文件的存储、以接口的形式将配置文件的 内容提供出去，Client 通过接口获取数据、并依据此数据初始化自己的应用。 Spring cloud 使用 git 或 svn 存放配置文件，默认情况下使用 git。`

### 19.1构建 Springcloud config 配置中心

- **<u>`1.配置中心服务端`</u>**

- ~~~xml
  <!-- 添加依赖 -->
  		<dependency>
              <groupId>org.springframework.cloud</groupId>
              <artifactId>spring-cloud-config-server</artifactId>
          </dependency>
  ~~~

- 启动类添加注解 @EnableConfigServer

- ~~~yml
  server:
    port: 8910
  
  spring:
    application:
      name: springcloud-config-server
    cloud:
      config:
        server:
          git:
            #uri 表示配置中心所在仓库的位置
            uri: https://gitee.com/telei/spring-cloud-config.git
            #search-paths 表示仓库下的子目录
            search-paths: config-center
            username: 799774821@qq.com
            passphrase: Qq13556787083
  ~~~

- **<u>`2.构建 Springcloud config 配置中心仓库`</u>**

- 







