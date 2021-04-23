Dubbo Rest协议支持
--------------------
# 简介
 通常我们知道dubbo支持Dubbo，Rmi，Hessian协议等协议，但有时候我们需要让服务支持rest协议，dubbo 2.8.4正好解决这个问题。

# 依赖
* spring
* dubbo
* netty

# 配置
```bash
<dubbo:application name="xxx" owner="xx" organization="xx"/>
<dubbo:registry address="multicast://224.5.6.7:1234"/>
<dubbo:protocol id="xx" name="rest" port="xx" keepalive="true" server="netty"  
iothreads="5" threads="100" chartset="utf-8"/>
<dubbo:annotation package="xx.xx.xx"/>
```

# 接口（没有接口会报错）
```bash
public interface EatServer {
	/**
	*@Valid 采用hibernate验证参数
	*/
	String eat(@Valid Food food);
}
```

# 提供服务（即接口实现）
```bash
@Service #(spring)
@Service(
	version="0.0.1",
	timeout=5000,
	retries=2,
	validation="true"
)
@Path("/")
public class EatServer implements EatServer {
	
	@Path("eat")
	@Post
	@Produces({ContentType.APPLICATION_JSON_UTF_8})
	@Consumes({ContentType.APPLICATION_JSON_UTF_8})
	String eat(@Valid Food food){
		....
	}
}
```