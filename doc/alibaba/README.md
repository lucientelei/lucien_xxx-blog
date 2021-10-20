

# 使用微服务必需关注版本依赖的关系

<img src="https://gitee.com/telei/picgoimage/raw/master/img/image-20210808151232649.png" alt="image-20210808151232649" style="zoom:67%;" />

# 1.Nacos

- `Nacos 提供了一组简单易用的特性集，帮助您快速实现动态服务发现、服务配置、服务元数据及流量管理`

  

### 1.1nacos注册服务实现

- 新建springboot项目

- ~~~xml
  <!-- 添加依赖 -->
  
  <!-- nacos注册服务-->
          <dependency>
              <groupId>com.alibaba.cloud</groupId>
              <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
          </dependency>
  
  
  <dependencyManagement>
          <dependencies>
              <!-- spring cloud alibaba 版本管理-->
              <dependency>
                  <groupId>com.alibaba.cloud</groupId>
                  <artifactId>spring-cloud-alibaba-dependencies</artifactId>
                  <version>2.2.5.RELEASE</version>
                  <type>pom</type>
                  <scope>import</scope>
              </dependency>
  
              <dependency>
                  <groupId>org.springframework.boot</groupId>
                  <artifactId>spring-boot-starter-parent</artifactId>
                  <version>2.3.2.RELEASE</version>
                  <type>pom</type>
                  <scope>import</scope>
              </dependency>
  
              <dependency>
                  <groupId>org.springframework.cloud</groupId>
                  <artifactId>spring-cloud-dependencies</artifactId>
                  <version>Hoxton.SR8</version>
                  <type>pom</type>
                  <scope>import</scope>
              </dependency>
          </dependencies>
      </dependencyManagement>
  ~~~

- 启动类添加 @EnableDiscoveryClient 发现注册客户端

- ~~~yml
  server:
    port: 8721
  
  spring:
    main:
      #允许bean覆盖 名称相同的会覆盖
      allow-bean-definition-overriding: true
    application:
      name: order-nacos
    #nacos配置信息
    cloud:
      nacos:
        server-addr: 127.0.0.1:8848
        discovery:
          username: nacos
          password: nacos
          #命名空间
          namespace: alibaba-mall
          #永久实例
          ephemeral: false
  ~~~

- 如果console输出注册成功，服务页面没有 重新打开startup.cmd 重新进入页面

### 1.2nacos集群

linux.........

# 2.Ribbon

**<u>*自定义负载均衡策略*</u>**

- ~~~java
  import com.netflix.loadbalancer.IRule;
  import com.netflix.loadbalancer.RandomRule;
  import org.springframework.boot.web.client.RestTemplateBuilder;
  import org.springframework.cloud.client.loadbalancer.LoadBalanced;
  import org.springframework.context.annotation.Bean;
  import org.springframework.context.annotation.Configuration;
  import org.springframework.web.client.RestTemplate;
  
  /**
   * @Author Lucien
   * @Date 2021/7/29
   * 添加resttemplate 配置
   */
  @Configuration
  public class RestTemplateConfig {
  
      @Bean
      @LoadBalanced
      public RestTemplate restTemplate(RestTemplateBuilder builder){
          RestTemplate restTemplate = builder.build();
          return restTemplate;
      }
  
      /**
       * 修改负载策略
       * @return
       */
      @Bean
      public IRule iRule(){
          return new RandomRule();
      }
  }
  
  ~~~

- ~~~java
  package com.lucien.alibaba.ribbon;
  
  import com.netflix.client.config.IClientConfig;
  import com.netflix.loadbalancer.AbstractLoadBalancer;
  import com.netflix.loadbalancer.AbstractLoadBalancerRule;
  import com.netflix.loadbalancer.ILoadBalancer;
  import com.netflix.loadbalancer.Server;
  
  import java.util.List;
  import java.util.concurrent.ThreadLocalRandom;
  
  /**
   * @Author Lucien
   * @Date 2021/7/29
   * 自定义ribbon负载策略
   */
  public class RuleConfig extends AbstractLoadBalancerRule {
  
      @Override
      public void initWithNiwsConfig(IClientConfig iClientConfig) {
  
      }
  
      @Override
      public Server choose(Object o) {
          ILoadBalancer loadBalancer = this.getLoadBalancer();
          //获取当前请求的服务实例
          List<Server> serverList = loadBalancer.getReachableServers();
  
          int random = ThreadLocalRandom.current().nextInt(serverList.size());
  
          Server server = serverList.get(random);
          System.out.println("调用端口："+server.getPort());
  //        if (server.isAlive()){
  //            return null;
  //        }
  
          return server;
      }
  }
  
  ~~~

- ~~~yml
  #配置自定义负载策略
  stock-nacos:
    ribbon:
    #指定配置文件
      NFLoadBalancerRuleClassName: com.lucien.alibaba.ribbon.RuleConfig
  
  #开启ribbon饥饿加载 解决第一次请求缓慢
  ribbon:
    eager-load:
      enabled: true
      #配置需要使用饥饿加载的服务端 多个服务使用,号分割
      clients: stock-nacos
  
  ~~~

# 3.Spring Cloud LoadBalancer

- ~~~xml
  <!-- nacos注册服务-->
          <dependency>
              <groupId>com.alibaba.cloud</groupId>
              <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
              <exclusions>
                  <!-- 剔除自带ribbon -->
                  <exclusion>
                      <groupId>org.springframework.cloud</groupId>
                      <artifactId>spring-cloud-starter-netflix-ribbon</artifactId>
                  </exclusion>
              </exclusions>
          </dependency>
  
  		<!-- 添加loadbalancer依赖，需要先有spring-cloud-dependencies依赖 -->
          <dependency>
              <groupId>org.springframework.cloud</groupId>
              <artifactId>spring-cloud-starter-loadbalancer</artifactId>
          </dependency>
  ~~~

- 使用@LoadBalanced 注解实现负载均衡

- ~~~yml
  server:
    port: 8725
  
  spring:
    main:
      #允许bean覆盖 名称相同的会覆盖
      allow-bean-definition-overriding: true
    application:
      name: loadbalancer-service
    cloud:
      nacos:
        server-addr: 127.0.0.1:8848
        discovery:
          username: nacos
          password: nacos
          #命名空间
          namespace: public
      loadbalancer:
        ribbon:
        	#关闭自带ribbon
          enabled: false
  ~~~

# 4.OpenFegin

**<u>`springcloud alibaba 直接使用openfegin 不使用fegin`</u>**

- ~~~xml
  <!-- 需要先有spring-cloud-dependencies依赖 -->	
  		<dependency>
              <groupId>org.springframework.cloud</groupId>
              <artifactId>spring-cloud-starter-openfeign</artifactId>
          </dependency>
  ~~~

- 启动类添加注解 @EnableFeignClients 启动fegin

- ~~~java
  package com.lucien.alibaba.fegin;
  
  import org.springframework.cloud.openfeign.FeignClient;
  import org.springframework.web.bind.annotation.RequestMapping;
  
  /**
   * @Author Lucien
   * @Date 2021/7/29
   */
  //name 指定rest接口对应的服务名称
  //path 指定rest接口所在的controller的@RequestMapping 没有则不写
  @FeignClient(name = "stock-nacos", path = "/stock")
  public interface StockFeginService {
  
      //与对应controller方法相同即可
      @RequestMapping("/reduct")
      public String reduct();
  
  
  }
  
  ~~~

- ~~~java
  package com.lucien.alibaba.controller;
  
  import com.lucien.alibaba.fegin.StockFeginService;
  import org.springframework.beans.factory.annotation.Autowired;
  import org.springframework.web.bind.annotation.RequestMapping;
  import org.springframework.web.bind.annotation.RestController;
  import org.springframework.web.client.RestTemplate;
  
  /**
   * @Author Lucien
   * @Date 2021/7/28
   */
  @RestController
  @RequestMapping("/order")
  public class OrderController {
  
      @Autowired
      private StockFeginService stockFeginService;
  
      @RequestMapping("/add")
      public String add(){
          System.out.println("下单成功");
  		//注入后直接调用方法
          String msg = stockFeginService.reduct();
  
          return "HELLO FEGIN" + msg;
      }
  }
  
  ~~~

### 4.1Fegin日志配置

1. 日志级别
   1. `**NONE：性能最佳，适用于生成，不记录任何日志(默认值)**`
   2. `**BASIC：适用于生成环境追踪问题，记录请求方法、URL、响应状态代码及执行时间**`
   3. `**HEADERS：在BSASIC基础上，记录请求和响应的header**`
   4. `**FULL：适用于开发及测试环境定位问题， 记录请求和响应的body、header、元数据**`

#### 全局配置

（fengin配置文件中添加@Configuration注解）

- ~~~java
  package com.lucien.alibaba.config;
  
  import feign.Logger;
  import org.springframework.context.annotation.Bean;
  import org.springframework.context.annotation.Configuration;
  
  
  /**
   * @Author Lucien
   * @Date 2021/7/29
   *  全局配置添加@Configuration
   *  局部配置不能添加@Configuration
   */
  //@Configuration
  public class FeginConfig {
  
      /**
       * 日志级别
       * @return
       * NONE：性能最佳，适用于生成，不记录任何日志(默认值)
       * BASIC：适用于生成环境追踪问题，记录请求方法、URL、响应状态代码及执行时间
       * HEADERS：在BSASIC基础上，记录请求和响应的header
       * FULL：适用于开发及测试环境定位问题， 记录请求和响应的body、header、元数据
       *
       */
      @Bean
      public Logger.Level feginLoggerLevel(){
          return Logger.Level.FULL;
      }
  }
  ~~~

- ~~~yml
  #springboot默认的日志级别为infom，fegin的debug日志级别就不会输出
  logging:
    level:
      com:
        lucien:
          alibaba:
            fegin: debug #对应配置文件所在位置
  ~~~

#### 局部配置

1. 第一种

   1. 取消配置文件@Configuration注解
   2. fegin接口中 @FeignClient(name = "stock-nacos", path = "/stock", configuration = FeginConfig.class)

2. 第二种

   1. ~~~yml
      #fegin日志局部配置
      feign:
        client:
          config:
            #服务名称
            stock-nacos:
              loggerLevel: BASIC
      ~~~

### 4.2超时配置

选其一即可

~~~java
/**
     * 超时时间配置
     * @return
     */
    @Bean
    public Request.Options options(){
        return  new Request.Options(5000, 10000);
    }
~~~

~~~yml
#fegin日志局部配置
feign:
  client:
    config:
      #服务名称
      stock-nacos:
        #连接超时时间
        connect-timeout: 5000
        #请求处理超时时间
        read-timeout: 10000
        loggerLevel: BASIC
~~~

### 4.3自定义拦截器

- **`拦截的是消费端向提供方的请求`**
- **`springmvc拦截的是客户端向服务消费端的请求`**

~~~java
import com.lucien.alibaba.config.FeginConfig;
import feign.RequestInterceptor;
import feign.RequestTemplate;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import java.util.UUID;

/**
 * @Author Lucien
 * @Date 2021/7/29
 * fegin自定义拦截器
 */
public class FeginInterceptor implements RequestInterceptor {

    private static final Logger log = LoggerFactory.getLogger(FeginConfig.class);

    @Override
    public void apply(RequestTemplate requestTemplate) {
        log.info("fegin拦截器");
        System.out.println("fegin拦截~~~");
        String token = UUID.randomUUID().toString();
        requestTemplate.header("TOKEN", token);
    }
}
//feginconfig------------- 第一种
	/**
     * 自定义拦截器
     * @return
     */
    @Bean
    public FeginInterceptor feginInterceptor(){
        return new FeginInterceptor();
    }
//yml 第二种
#fegin日志局部配置
feign:
  client:
    config:
      #服务名称
      stock-nacos:
        #连接超时时间
        connect-timeout: 5000
        #请求处理超时时间
        read-timeout: 10000
        loggerLevel: FULL
        #指定拦截器
        requestInterceptors[0]: com.lucien.alibaba.interceptor.FeginInterceptor
~~~

# 5.Nacos配置中心

- **`nacos客户端默认的是Property的文件拓展名`**
- **`维护性、时效性、安全性`**

~~~xml
<!-- nacos config 依赖 -->
        <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-starter-alibaba-nacos-config</artifactId>
        </dependency>
~~~

添加bootstrap.yml

~~~yml
spring:
  application:
    #根据服务名称自动拉去对应dataid的配置信息
    name: nacos-config-service
  cloud:
    nacos:
      server-addr: 127.0.0.1:8848
      username: nacos
      password: nacos
~~~

![](https://gitee.com/telei/picgoimage/raw/master/img/image-20210730160251126.png)

读取

~~~java
package com.lucien.nacos;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.ConfigurableApplicationContext;

import java.util.concurrent.TimeUnit;

/**
 * @Author Lucien
 * @Date 2021/7/30
 nacos客户端 每10秒去注册中心判断 配置文件是否变化 拉取到的信息为MD5加密的
 */
@SpringBootApplication
public class ConfigApplication {
    public static void main(String[] args) throws InterruptedException {
        ConfigurableApplicationContext applicationContext = SpringApplication.run(ConfigApplication.class, args);
        while (true) {
            String username = applicationContext.getEnvironment().getProperty("user.name");
            String userage = applicationContext.getEnvironment().getProperty("user.age");
            System.out.println(username + "==" + userage);
            TimeUnit.SECONDS.sleep(1);
        }
    }
}

~~~

### 5.1使用类型配置拓展名

~~~yml
#bootstrap.yml中添加
  cloud:
    nacos:
      #指定配置文件拓展名 除properties
      config:
        file-extension: yaml

~~~

### 5.2配置文件优先级

`优先级大的会覆盖优先级小的，并且形成互补。profile > 默认配置文件`

### 5.3获取配置信息

@value方式

~~~java
package com.lucien.nacos.controller;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.cloud.context.config.annotation.RefreshScope;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

/**
 * @Author Lucien
 * @Date 2021/7/30
 */
@RestController
@RequestMapping("/config")
//添加该注解 实现实时更新配置信息
@RefreshScope
public class ConfigController {

    @Value("${user.name}")
    private String username;

    @RequestMapping("/show")
    public String show(){
        return username;
    }

}

~~~

# 6.Sentinel

**`面向分布式服务的高可用防护组件 解决服务雪崩问题`**

- 常见的容错机制
  - 超时机制
  - 服务限流
  - 隔离
  - 服务熔断

### 6.1代码控制流控

- ~~~xml
  <!-- sentinel核心库 -->
          <dependency>
              <groupId>com.alibaba.csp</groupId>
              <artifactId>sentinel-core</artifactId>
              <version>1.8.0</version>
          </dependency>
  
  <!-- SentinelResource注解 -->
  <dependency>
              <groupId>com.alibaba.csp</groupId>
              <artifactId>sentinel-annotation-aspectj</artifactId>
              <version>1.8.0</version>
          </dependency>
  ~~~

- ~~~java
  //使用@SentinelResource注解需要添加bean
  @Bean
      public SentinelResourceAspect sentinelResourceAspect(){
          return new SentinelResourceAspect();
      }
  ~~~

- ~~~java
  /**
   * @Author Lucien
   * @Date 2021/8/1
   */
  @RestController
  @Slf4j
  public class SentinelController {
  
      private static final String RESOURCE_NAME = "Hello";
  
      private static final String ASPECTJ_RESOURCE_NAME = "aspectj";
  
      private static final String DEGRADE_RESOURCE_NAME = "degrade";
  
      //进行sentinel流控
      @RequestMapping("/hello")
      public String hello(){
          Entry entry = null;
          try{
              //sentinel针对资源进行限制
              entry = SphU.entry(RESOURCE_NAME);
              //被保护的业务逻辑
              String str = "Hello sentinel";
              log.info("=======" + str + "========");
              return str;
          } catch (BlockException e) {
              log.info("block");
              return "被流控了";
          } catch (Exception e){
              Tracer.traceEntry(e, entry);
          } finally{
              if(entry != null){
                  entry.exit();
              }
          }
          return null;
      }
  
      @PostConstruct
      private static void initFlowRule(){
          //流控规则
          List<FlowRule> rules = new ArrayList<>();
          //流控
          FlowRule flowRule = new FlowRule();
          //设置受保护的资源
          flowRule.setRefResource(RESOURCE_NAME);
          //设置流控规则QPS
          flowRule.setGrade(RuleConstant.FLOW_GRADE_QPS);
          //设置受保护的资源阈值
          flowRule.setCount(1);
          rules.add(flowRule);
          FlowRule aspectjRule = new FlowRule();
          aspectjRule.setRefResource(ASPECTJ_RESOURCE_NAME);
          aspectjRule.setGrade(RuleConstant.FLOW_GRADE_QPS);
          aspectjRule.setCount(1);
          rules.add(aspectjRule);
          //加载配置好的规则
          FlowRuleManager.loadRules(rules);
  
      }
      
      @PostConstruct
      public static void initDegradeRule(){
          //降级规则
          List<DegradeRule> degradeRules = new ArrayList<>();
          DegradeRule degradeRule = new DegradeRule();
          degradeRule.setResource(DEGRADE_RESOURCE_NAME);
          degradeRule.setGrade(RuleConstant.DEGRADE_GRADE_EXCEPTION_COUNT);
          degradeRule.setCount(2);
          degradeRule.setTimeWindow(10);
          degradeRule.setMinRequestAmount(2);
  
          degradeRules.add(degradeRule);
          DegradeRuleManager.loadRules(degradeRules);
      }
  
      /**
       * 通过注解解决 sentinel耦合接口的弊端
       * 1.添加依赖 2.配置bean
       * value: 配置资源
       * blockHandler：设置流控降级后的处理方法(默认该方法必需声明在同一个类中)
       * blockHandlerClass：声明在类中的方法需要为static
       * fallback：当接口出现异常就交给fallback的方法处理
       * @return
       */
      @RequestMapping("/aspectj")
      @SentinelResource(value = ASPECTJ_RESOURCE_NAME, blockHandler = "blockHandlerForAspectj")
      public String aspectj(){
          return "Hello Aspectj";
      }
  
      /**
       * 注意：
       * 1.必须为public
       * 2.返回一需要一致 且要包含原方法参数
       * BlockException 可区分是什么规则的处理方法
       * @param e
       * @return
       */
      public String blockHandlerForAspectj(BlockException e){
          e.printStackTrace();
          return "aspectj被限流";
      }
  }
  
  ~~~


### 6.2Alibaba整合sentinel控制台

- ~~~xml
  <dependency>
              <groupId>com.alibaba.cloud</groupId>
              <artifactId>spring-cloud-starter-alibaba-sentinel</artifactId>
          </dependency>
  ~~~

- ~~~yml
  server:
    port: 8810
  
  spring:
    application:
      name: sentinel-dashborad
    cloud:
      nacos:
        server-addr: 127.0.0.1:8848
      sentinel:
        transport:
          # 添加sentinel的控制台地址
          dashboard: 127.0.0.1:8080
          # 指定应用与Sentinel控制台交互的端口，应用本地会起一个该端口占用的HttpServer
  ~~~

- 下载运行sentinel jar包

- cmd中运行： java -jar XXX

### 6.3统一异常处理

~~~java
@Slf4j
@Component
public class MyBlockExceptionHandler implements BlockExceptionHandler {
  @Override
 public void handle(HttpServletRequest request, HttpServletResponse response, BlockException e) throws Exception {
  log.info("BlockExceptionHandler BlockException================"+e.getRule());

 R r = null;

 if (e instanceof FlowException) {
  r = R.error(100,"接口限流了");

 } else if (e instanceof DegradeException) {
  r = R.error(101,"服务降级了");

 } else if (e instanceof ParamFlowException) {
  r = R.error(102,"热点参数限流了");

 } else if (e instanceof SystemBlockException) {
  r = R.error(103,"触发系统保护规则了");

 } else if (e instanceof AuthorityException) {
  r = R.error(104,"授权规则不通过");
 }

 //返回json数据
  response.setStatus(500);
  response.setCharacterEncoding("utf‐8");
  response.setContentType(MediaType.APPLICATION_JSON_VALUE);
 new ObjectMapper().writeValue(response.getWriter(), r);

 }
 }
~~~







# 7.Seata

**`Alibaba微服务分布式事务组件`**

- **`Seata 是一款开源的分布式事务解决方案，致力于提供高性能和简单易用的分布式事务服务。Seata 将为用户提供了 	AT、TCC、SAGA 和 
  XA 事务模式，为用户打造一站式的分布式解决方案。AT模式是阿里首推的模式,阿里云上有商用版本的GTS（Global Transaction 
  Service 全局事务服务）`**
- Seata的三大角色 在 Seata 的架构中，一共有三个角色： 
  - TC (Transaction Coordinator) - 事务协调者 维护全局和分支事务的状态，驱动全局事务提交或回滚。 
  - TM (Transaction Manager) - 事务管理器 定义全局事务的范围：开始全局事务、提交或回滚全局事务。 
  - RM (Resource Manager) - 资源管理器 管理分支事务处理的资源，与TC交谈以注册分支事务和报告分支事务的状态，并驱动分支事务提交或回滚。 
  - Seata分TC、TM和RM三个角色，TC（Server端）为单独服务端部署，TM和RM（Client端）由业务系统集成
- 在 Seata 中，一个分布式事务的生命周期如下：
- <img src="https://gitee.com/telei/picgoimage/raw/master/img/image-20210805150941794.png" alt="image-20210805150941794" style="zoom:67%;" />

### 7.1事务简介

`事务(Transaction)是访问并可能更新数据库中各种数据项的一个程序执行单元(unit)。在关系数据 库中，一个事务由一组SQL语句组成。事务应该具有4个属性：原子性、一致性、隔离性、持久性。 这四个属性通常称为ACID特性`

-  **原子性（atomicity**）：个事务是一个不可分割的工作单位，事务中包括的诸操作要么都做，要么 都不做。   
- **一致性（consistency）**：事务必须是使数据库从一个一致性状态变到另一个一致性状态，事务 的中间状态不能被观察到的。   
- **隔离性（isolation）**：一个事务的执行不能被其他事务干扰。即一个事务内部的操作及使用的数 据对并发的其他事务是隔离的，并发执行的各个事务之间不能互相干扰。隔离性又分为四个级别：读 未提交(read uncommitted)、读已提交(read committed，解决脏读)、可重复读(repeatable read，解决虚读)、串行化(serializable，解决幻读)。   
- **持久性（durability）**：持久性也称永久性（permanence），指一个事务一旦提交，它对数据库 中数据的改变就应该是永久性的。接下来的其他操作或故障不应该对其有任何影响

#### 7.1.1本地事务

`大多数场景下，我们的应用都只需要操作单一的数据库，这种情况下的事务称之为本地事务 本地事务的ACID特性是数据库直接提供支持`

![image-20210805145232048](https://gitee.com/telei/picgoimage/raw/master/img/image-20210805145232048.png)

#### 7.1.2分布式事务

`分布式事务就是为了保证不同资源服务器的数据一致性`

- 跨库事务
- ![image-20210805145335121](https://gitee.com/telei/picgoimage/raw/master/img/image-20210805145335121.png)
-  分库分表
- ![image-20210805145355941](https://gitee.com/telei/picgoimage/raw/master/img/image-20210805145355941.png)

### 7.2服务化

<img src="https://gitee.com/telei/picgoimage/raw/master/img/image-20210805145448394.png" alt="image-20210805145448394" style="zoom:80%;" />

常见分布式事务解决方案 

1. seata 阿里分布式事务框架 
2. 消息队列
3. saga
4. XA

`他们有一个共同点，都是两阶段(2PC) “两阶段”是指完成整个分布式事务，划分成两个步骤 完成`

### 7.3分布式事务理论基础

`解决分布式事务，也有相应的规范和协议。分布式事务相关的协议有2PC、3PC。
由于三阶段提交协议3PC非常难实现，目前市面主流的分布式事务解决方案都是2PC协议。这就是文章
开始提及的常见分布式事务解决方案里面，那些列举的都有一个共同点“两阶段”的内在原因。
有些文章分析2PC时，几乎都会用TCC两阶段的例子，第一阶段try，第二阶段完成confirm或
cancel。其实2PC并不是专为实现TCC设计的，2PC具有普适性——协议一样的存在，`

`目前绝大多数 分布式解决方案都是以两阶段提交协议2PC为基础的`

### 7.4 2PC两阶段提交协议

#### 1.Prepare：提交事务请求

<img src="https://gitee.com/telei/picgoimage/raw/master/img/image-20210805145903141.png" alt="image-20210805145903141" style="zoom: 50%;" />

#### 2.Commit：执行事务提交

<img src="https://gitee.com/telei/picgoimage/raw/master/img/image-20210805150022833.png" alt="image-20210805150022833" style="zoom: 67%;" />

<img src="https://gitee.com/telei/picgoimage/raw/master/img/image-20210805150039925.png" alt="image-20210805150039925" style="zoom:67%;" />

### 7.5 Seata Server（TC）环境搭建

部署高可用集群模式 seata使用 db + nacos

[seata下载地址](https://github.com/seata/seata/releases)

- Server端存储模式（store.mode）支持三种：
  - file：(默认)单机模式，全局事务会话信息内存中读写并持久化本地文件root.data，性能较高(默认）
  - db：（5.7+）高可用模式，全局事务会话信息通过db共享，相应性能差些
    - 打开config/file.conf 
    - 修改mode="db"
    - 修改数据库连接信息
    - 创建数据库seata_server
    - 新建表：  可以去seata提供的资源信息中下载 [sql文件](https://github.com/seata/seata/blob/1.4.0/script/server/db/mysql.sql)
  - redis：Seata-Server 1.3及以上版本支持,性能较高,存在事务信息丢失风险,请提前配置适合当前场景的redis持久化配置

#### **<u>7.5.1db存储模式+Nacos(注册&配置中心)部署</u>**

- Seata Server注册到Nacos，修改conf目录下的registry.conf配置
- <img src="https://gitee.com/telei/picgoimage/raw/master/img/image-20210805154129252.png" alt="image-20210805154129252" style="zoom:50%;" />
- 配置Nacos配置中心
- <img src="https://gitee.com/telei/picgoimage/raw/master/img/image-20210805154401612.png" alt="image-20210805154401612" style="zoom:50%;" />
- seata-server-1.3.0\script\config-center\config.txt 修改  
  - store.mode=db 数据库连接信息
- seata-server-1.3.0\script\config-center\nacos  运行 nacos-config.sh 注册配置到nacos中
- bin目录下启动 seata-server.bat

#### 7.5.2微服务整合seata







# 8.GateWay

### 8.1 什么是Spring Cloud Gateway

- `网关作为流量的入口，常用的功能包括路由转发，权限校验，限流等。 Spring Cloud Gateway 是Spring Cloud官方推出的第二代网关框架，定位于取代 Netflix Zuul1.0。相比 Zuul 来说，Spring Cloud Gateway 提供更优秀的性能，更强大的有功能。 Spring Cloud Gateway 是由 WebFlux + Netty + Reactor 实现的响应式的 API 网关。它不能在传统的 servlet 容器中工作，也不能构 建成 war 包。 Spring Cloud Gateway 旨在为微服务架构提供一种简单且有效的 API 路由的管理方式，并基于 Filter 的方式提供网关的基本功能，例如 说安全认证、监控、限流等等`

### 8.2Spring Cloud Gateway 功能特征

1. 基于Spring Framework 5, Project Reactor 和 Spring Boot 2.0 进行构建； 
2. 动态路由：能够匹配任何请求属性； 
3. 支持路径重写; 集成 Spring Cloud 服务发现功能（Nacos、Eruka）；
4.  可集成流控降级功能（Sentinel、Hystrix）； 
5. 可以对路由指定易于编写的 Predicate（断言）和 Filter（过滤器）；

### 8.3核心概念

- 路由（route) 
  - 路由是网关中最基础的部分，路由信息包括一个ID、一个目的URI、一组断言工厂、一组Filter组成。如果断言为真，则说明请求的URL和 配置的路由匹配。 
- 断言(predicates)
  -  Java8中的断言函数，SpringCloud Gateway中的断言函数类型是Spring5.0框架中的ServerWebExchange。断言函数允许开发者去定义 匹配Http request中的任何信息，比如请求头和参数等。 
- 过滤器（Filter) 
  - SpringCloud Gateway中的filter分为Gateway FilIer和Global Filter。Filter可以对请求和响应进行处理。

### 8.4执行流程

<img src="https://gitee.com/telei/picgoimage/raw/master/img/image-20210808151811252.png" alt="image-20210808151811252" style="zoom:67%;" />

### 8.5Nacos整合GateWay

- ~~~xml
  <dependency>
              <groupId>com.alibaba.cloud</groupId>
              <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
          </dependency>
  
          <dependency>
              <groupId>org.springframework.cloud</groupId>
              <artifactId>spring-cloud-starter-gateway</artifactId>
          </dependency>
  ~~~

- ~~~yml
  spring:
    cloud:
      gateway:
        discovery:
          locator:
            enabled: true
            # 请求服务是小写的时候改成true（默认为false轻微服务必须是大写）
            lower-case-service-id: true
        routes:
            # 自定义的路由 ID，保持唯一
          - id: gateway-nacos-provider
            # 目标服务地址,以lb://开头（lb代表从注册中心获取服务）
            uri: lb://gateway-provider
            # 断言（就是路由转发要满足的条件）
            predicates:
            - Path=/gateway-provider/**
            - Method=GET
            # 过滤器，请求在传递过程中可以通过过滤器对其进行一定的修改
            filters:
            - StripPrefix=1 # 转发之前去掉1层路径
  
  ~~~

- ~~~yml
  #bootstrap.yml
  server:
    port: 9100
  spring:
    application:
      name: gateway-nacos
    cloud:
      nacos:
        discovery:
          server-addr: 127.0.0.1:8848
  ~~~

  

- 启动类添加 @EnableDiscoveryClient 发现注册中心

- 通过gateway端口访问提供者的接口 访问成功即可

