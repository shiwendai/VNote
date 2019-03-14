# 把Solr工程注册到Eureka服务中心

## 在非Spring Boot环境下配置Eureka Service Provider

对于一些老系统而言，进行Spring Boot的改造可能牵涉甚广。但是当周边系统被改造成基于Spring Cloud的微服务后，这些老系统也存在集成Eureka去提供和调用REST服务的诉求。所以我们简单研究了一下如何在非Spring Boot应用中集成原生的Eureka Client。

首先，添加Maven依赖：

```
<dependency>
    <groupId>com.netflix.eureka</groupId>
    <artifactId>eureka-client</artifactId>
    <version>1.4.12</version>
</dependency>
<dependency>
    <groupId>com.netflix.archaius</groupId>
    <artifactId>archaius-core</artifactId>
    <version>0.7.3</version>
</dependency>
```
然后是配置文件（必须起名config.properties，放在classpath中才能被eureka感知到）：
```
eureka.region=default

#应用名称(Name of the application to be identified by other services)
eureka.name=sampleserviceSOLR1

#Virtual IP Address，这也是一个应用的标识符，类似于域名
eureka.vipAddress=sampleservice.mydomain.net

#服务端口
eureka.port=1935
#Eureka Server的地址（对应zone的名称为default）
eureka.serviceUrl.default=http://localhost:7001/eureka

#For eureka clients running in eureka server, it needs to connect to servers in other zones
eureka.preferSameZone=false

#Change this if you want to use a DNS based lookup for determining other eureka servers. For example
#of specifying the DNS entries, check the eureka-client-test.properties, eureka-client-prod.properties
eureka.shouldUseDns=false

eureka.us-east-1.availabilityZones=default
```
代码如下：

```
public class EurekaListener implements	ServletContextListener {
	
	private static final Logger LOG = LoggerFactory.getLogger(EurekaListener.class);

	private ApplicationInfoManager applicationInfoManager;
	private EurekaClient eurekaClient;


	@Override
	public void contextDestroyed(ServletContextEvent arg0) {
		// TODO Auto-generated method stub
		if (eurekaClient != null)
		{
			LOG.info("Shutting down eureka service.");
			eurekaClient.shutdown();
		}
	}

	@Override
	public void contextInitialized(ServletContextEvent arg0) {
		
		// TODO Auto-generated method stub
		//初始化应用信息管理器，设置其状态为STAERTING
        applicationInfoManager = initializeApplicationInfoManager(new MyDataCenterInstanceConfig());
        applicationInfoManager.setInstanceStatus(InstanceInfo.InstanceStatus.STARTING);
        LOG.info("Registering service to eureka with STARTING status");

        //读取配置文件，初始化eurekaClient，并设置应用信息管理器的状态为UP
		DynamicPropertyFactory.getInstance();
		eurekaClient = initializeEurekaClient(applicationInfoManager, new DefaultEurekaClientConfig());
		applicationInfoManager.setInstanceStatus(InstanceInfo.InstanceStatus.UP);
		LOG.info("Initialization finished, now changing eureka client status to UP");
		
	}
	
	//初始化应用信息管理器
    private synchronized ApplicationInfoManager initializeApplicationInfoManager(EurekaInstanceConfig instanceConfig)
	{
		if (applicationInfoManager == null)
		{
			InstanceInfo instanceInfo = new EurekaConfigBasedInstanceInfoProvider(instanceConfig).get();
			applicationInfoManager = new ApplicationInfoManager(instanceConfig, instanceInfo);
		}
		return applicationInfoManager;
	}

	//初始化EurekaClient
    private synchronized EurekaClient initializeEurekaClient(ApplicationInfoManager applicationInfoManager, EurekaClientConfig clientConfig)
	{
		if (eurekaClient == null)
			eurekaClient = new DiscoveryClient(applicationInfoManager, clientConfig);
		return eurekaClient;
	}
	
}
```

其实很简单，就是初始化一个ApplicationInfoManager和EurekaClient，剩下的注册和续约细节会由EurekaClient接管。

然后再web.xml上进行注册listener监听器
```
  <listener>  
	 <listener-class>com.intellif.smm.solr.listener.EurekaListener</listener-class>  
  </listener>  
```
大功告成！！！！