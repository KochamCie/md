[TOC]

### 介绍

##### Disconf

* Distributed Configuration Management Platf（分布式配置管理平台）专注于各种「分布式系统配置管理」的「通用组件」和「通用平台」, 提供统一的「配置管理服务」。
* 需要注意的是，disconf有自己的管理后台（服务端），使用disconf服务的为客户端。




__配置文件类需要交由spring托管，且为单例__

我们常用的作为配置的有：

- 配置项
- 配置文件
  - .properties文件
  - .xml文件

针对不同类型的，客户端的处理方式不同。配置文件可支持自动reload与不自动reload，自动reload时需要定义通知回调类来配合更新配置内容。而配置项的reload则是自动更新的，我们可以将配置项定义为__轻配置__。当然，如果你想在配置项改变后触发本地其他的事件，你也可以定义一个通知回调来做。在定义javabean时key就是相应disconf的item了

```java
    @DisconfItem(key = "staticItem")
    public static int getStaticItem() {
        return staticItem;
    }
```

在配置文件的更新中，我们根据业务场景有时候需要自动reload想实时生效，也会有让配置信息重启后生效(__重配置__调整后实时生效，当配置错ip或者端口号会导致线上业务异常。这时的场景是配置修改后，重启生效后进行测试确认能走通然后再放出客户端使用)



### 整合

##### 为什么用配置中心

在异构或同构的服务中，在业务量及用户量逐步提高时为了服务高可用进行分布式、集群等方案，我们的服务(或机器)会越来越多，不同的服务会有不同的配置文件。我们需要有一个统一的配置文件管理中心来集中管理这些上百个配置文件。

为什么要用配置中心？我们从开发的角度以及运维的角度去思考。

事实上，作为开发人员在开发的过程中只是一套local的环境代码，配置文件放在项目中开发很便捷，随时调整重启后便能玩耍。但是如果我们需要一个配置项去控制某个功能的开启关闭，比如下个月活动开始结束时间或者临时关闭这次活动，我们需要在<u>__生产环境__</u>去更改配置并<u>__重启服务__</u>。

从运维的角度，生产环境上我们每个项目或者服务为了保证功能的稳定性，通常会部署两套及以上。相应的，也就需要维护实例数*配置文件个数，50台机器每台机器至少4个配置文件，那么就是200个配置文件。当然，我们不会同时抑或频繁的去调整它们，但这无形中也增加了运维的成本。

需要有一个统一的配置中心，那么，这个配置中心需要满足我们如下要求：

* 支持多种格式的配置文件管理，如properties、xml、yml、conf等
* 安全性、高可用、稳定性
* 最好能提供配置更新时不重启服务
* 管理方便
* 有配置信息备份功能，能回退

##### 为什么用disconf

disconf技术背景

在一个分布式环境中，同类型的服务往往会部署很多实例。这些实例使用了一些配置，为了更好地维护这些配置就产生了配置管理服务。通过这个服务可以轻松地管理成千上百个服务实例的配置问题。

王阿晶提出了基于zooKeeper的配置信息存储方案的设计与实现, 它将所有配置存储在zookeeper上，这会导致配置的管理不那么方便，而且他们没有相关的源码实现。淘宝的diamond是淘宝内部使用的一个管理持久配置的系统，它具有完整的开源源码实现，它的特点是简单、可靠、易用，淘宝内部绝大多数系统的配置都采用diamond来进行统一管理。他将所有配置文件里的配置打散化进行存储，只支持KV结构，并且配置更新的推送是非实时的。百度内部的BJF配置中心服务采用了类似淘宝diamond的实现，也是配置打散化、只支持KV和非实时推送。

同构系统是市场的主流，特别地，在业界大量使用部署虚拟化（如JPAAS系统，SAE，BAE）的情况下，同一个系统使用同一个部署包的情景会越来越多。但是，异构系统也有一定的存在意义，譬如，对于“拉模式”的多个下游实例，同一时间点只能只有一个下游实例在运行。在这种情景下，就存在多台实例机器有“主备机”模式的问题。目前国内并没有很明显的解决方案来统一解决此问题。







以P2PSystem系统作为客户端为例，将redis配置文件redis.properties交由disconf托管。

首先，disconf为客户端接入提供了几种解决方案。注解式和XML配置形式。

其中XML配置的方式又分为会自动reload配置内容和不会自动reload配置内容。

用注解式的方式来托管redis.properties:

1.引入disconf客户端所需的jar包，disconf-client-2.6.36.jar，zookeeper-3.4.8.jar

  maven项目只用引入其下，在client依赖包中会有disconf-core.jar以及zookeeper.jar的依赖

```xml
<dependency>
   <groupId>com.baidu.disconf</groupId>
   <artifactId>disconf-client</artifactId>
   <version>2.6.36</version>
</dependency>
```



2.依赖添加完毕，启动时disconf会默认读取客户端所需要订阅的配置空间

   我们需要在classpath下新建一个disconf.properties文件，来声明作为客户端使用disconf服务

```properties
# 是否使用远程配置文件 true(默认)会从远程获取配置 false则直接获取本地配置
enable.remote.conf=true

# 配置服务器的 HOST,用逗号分隔  127.0.0.1:8000,127.0.0.1:8000
conf_server_host=disconf.xwjrfw.cn:8080

# 版本, 统一风格,请采用 X.X.X.X 格式   
version=1.0.1

# APP 请采用 产品线_服务名 格式
app=p2papi

# 环境
env=rd

# debug
debug=true

# 忽略哪些分布式配置，用逗号分隔
ignore=

# 获取远程配置 重试次数，默认是3次
conf_server_url_retry_times=1
# 获取远程配置 重试时休眠时间，默认是5秒
conf_server_url_retry_sleep_seconds=1
```



3.如何自动reload配置信息

当配置更新时，disconf服务器会发一个通知到客户端，因此我们需要有一个回调类来响应此消息。

更新完配置信息后，调用redisService时就会使用新的配置信息了

```java
/*
 *  @DisconfUpdateService可以指定多个，是指去更新哪些target内容
*/
@Service
@Scope("singleton")
@DisconfUpdateService(classes = {DisconfJedisConfig.class})
public class SimpleRedisServiceUpdateCallback implements IDisconfUpdate {

    protected static final Logger LOGGER = LoggerFactory.getLogger(SimpleRedisServiceUpdateCallback.class);

    @Autowired
    private SimpleRedisService simpleRedisService;

    /**
     * 调用jedis连接更改host port auth的方法
     */
    public void reload() throws Exception {
        LOGGER.info("reload =====================");
        simpleRedisService.changeJedis();
    }
}
```



当然，还支持XML配置的方式来使用

基于XML的分布式配置文件管理,不会自动reload

- 非注解式（托管式）：配置文件没有相应的配置注解类，此配置文件不会被注入到配置类中。disconf只是简单的对其进行“托管”。 启动时下载配置文件；配置文件变化时，负责动态推送。注意，此种方式，程序不会自动reload配置，需要自己写回调函数。

**由于此方法不会自动reload，因此，对于那些比较重的资源，比如jdbc等，是比较好的托管方式。**

```xml
 <!--使用托管方式的disconf配置(无代码侵入, 配置更改会自动reload)-->
 <!-- 列表下的配置文件将全部托管在disconf服务器，这些配置文件是没有相应的配置文件类 -->
<bean id="configproperties_disconf"
      class="com.baidu.disconf.client.addons.properties.ReloadablePropertiesFactoryBean">
    <property name="locations">
        <list>
            <value>classpath*:autoconfig.properties</value>
        </list>
    </property>
</bean>
<!-- 不会自动reload的class配置为：class="org.springframework.beans.factory.config.PropertyPlaceholderConfigurer" 
     会自动reload的class配置为：
class="com.baidu.disconf.client.addons.properties.ReloadingPropertyPlaceholderConfigurer"
-->
<bean id="propertyConfigurer"
      class="com.baidu.disconf.client.addons.properties.ReloadingPropertyPlaceholderConfigurer">
    <property name="ignoreResourceNotFound" value="true"/>
    <property name="ignoreUnresolvablePlaceholders" value="true"/>
    <property name="propertiesArray">
        <list>
            <ref bean="configproperties_disconf"/>
        </list>
    </property>
</bean>
```





 第一次部署p2papi

__所有文件__均可托管维护,但分为以下两种定义：

- 客户端托管文件，__只需要__在disconf服务器上对配置文件维护
  - reboot：修改后，重启客户端生效
  - reload：修改后，自动生效
- 脚本托管文件，即需要在disconf上__维护__，还需要使用脚本进行__同步__



__针对disconf.properties,applicationContext-disconf.xml__

1. 备份升级节点之前的生产p2papi运行包(主要是备份源码包中的配置文件)
2. 脚本拿去升级包
   - 脚本排除上述文件：则需要手动加这两个文件到指定目录。再修改disconf.properties指向生产的disconf
   - 不排除，则下次升级脚本需要加上排除这两个文件。再修改disconf.properties指向生产的disconf
   - 注：修改指向成生产后，无特殊情况是不会进行更改的。
3. 这两个文件无法成为客户端托管文件，但能做成脚本托管文件。所以第2步中的修改，可以通过脚本管理的方式进行维护
4. 升级中涉及到配置的更改，都可以用到此方式，但需分清楚托管文件的类型，然后选取合适的方式进行维护。









