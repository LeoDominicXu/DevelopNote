Spring cloud Eureka
-----------

# Eureka 服务注册中心
* 新建项目
	选择Spring Initializr工具新建项目，然后选择Initializr Url：https://start.spring.io 填好项目基本信息，然后选择Cloud Discovery的Eureka Server即可新建Eureka Server的项目。
* 修改启动程序类
```bash
@SpringBootApplication
@EnableEurekaServer
public class EurekaServerApplication {

	public static void main(String[] args) {
		SpringApplication.run(EurekaServerApplication.class, args);
	}
}
```

* 修改配置文件application.properties

```bash
server.port=1111

eureka.instance.hostname=localhost
eureka.client.register-with-eureka=false
eureka.client.fetch-registry=false
eureka.client.serviceUrl.defaultZone=http://${eureka.instance.hostname}:${server.port}/eureka/

```
>eureka.client.register-with-eureka 由于该应用为注册中心，所以不需要向自己注册自己。
>eureka.client.fetch-registry 由于注册中心的职责是维护服务实例，他并不需要去检查服务，所以也设置为false

* 运行程序
	一个Eureka 服务端服务端程序便已创建完成了，接下来便可以通过浏览器访问http://localhost:1111/ 访问服务，查看注册表信息。

# Eureka 服务提供者
* 新建项目
	选择Spring Initializr工具新建项目，然后选择Initializr Url：https://start.spring.io 填好项目基本信息，然后选择Web的web模块和Cloud Discovery的Eureka Discovery模块即可新建Eureka 提供者的项目。
* 修改启动程序类
```bash
@SpringBootApplication
@EnableEurekaClient
public class EurekaServerApplication {

	public static void main(String[] args) {
		SpringApplication.run(EurekaServerApplication.class, args);
	}
}
```
```bash
@RestController
public class HelloController {
    @RequestMapping(value = "/hello", method = RequestMethod.GET)
    public String home(){
        return "Hello World";
    }
}
```
这里@EnableEurekaClient也可以用：@EnableDiscoveryClient


* 修改配置文件application.properties

```bash
spring.application.name=hello-service
eureka.client.serviceUrl.defaultZone=http://localhost:1111/eureka/

```

* 运行程序
	一个Eureka 服务提供者程序便已创建完成了，接下来便可以通过浏览器访问http://localhost:1111/ 访问服务，查看注册表信息。看服务有没有被注册上

# Eureka 服务消费者
* 新建项目
	和服务提供者一样
* 修改启动程序类
```bash
@SpringBootApplication
@EnableDiscoveryClient
public class EurekaConsumerApplication {
	@Bean
	@LoadBalanced
	RestTemplate restTemplate(){
		return new RestTemplate();
	}

	public static void main(String[] args) {
		SpringApplication.run(EurekaConsumerApplication.class, args);
	}
}
```
```bash
@RestController
public class ConsumerController {
    @Resource
    RestTemplate restTemplate;

    @RequestMapping(value = "/ribbon-consumer",method = RequestMethod.GET)
    public String helloConsumer(){
        return restTemplate.getForEntity("http://HELLO-SERVICE/hello", String.class).getBody();
    }
}
```
这里@EnableEurekaClient也可以用：@EnableDiscoveryClient


* 修改配置文件application.properties

```bash
spring.application.name=ribbon-consumer
server.port=9000
eureka.client.serviceUrl.defaultZone=http://localhost:1111/eureka/

```
> 由于8080端口已经被提供者占用，所以这里将端口改为9000

* 运行程序
	一个Eureka 服务提供者程序便已创建完成了，接下来便可以通过浏览器访问http://localhost:1111/ 访问服务，查看注册表信息。看服务有没有被注册上
* 消费者调用提供者
	在浏览器输入http://localhost/ribbon-consumer 看结果，如果页面显示hello world说明程序运行没有问题。

# 结语
	到这里一个简单的Eureka 注册中心，服务提供者，服务消费者demo就完成了，感谢浏览。