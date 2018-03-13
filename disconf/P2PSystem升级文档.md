## P2PSystem升级文档

### 0.备份生产代码

### 1.获取源码以及本版本的改动说明

<u>接入disconf及配置redis cluster</u>

从测试服务器上获取最新代码。这次新增及修改的内容如下：

- <u>**修改**</u>applicationContext.xml

  - 调整加载properties文件的配置，最好整个<bean>标签替换

    ```xml
    <bean id="propertyConfigurer" class="org.springframework.beans.factory.config.PropertyPlaceholderConfigurer">
        <property name="order" value="1" />
        <property name="ignoreUnresolvablePlaceholders" value="true" />
        <property name="locations">
            <list>
                <value>/config/resource.properties</value>
                <value>classpath*:*.properties</value>
            </list>
        </property>
    </bean>
    ```

- <u>**新增**</u>applicationContext-disconf.xml
  - disconf的spring配置文件
  - 路径：WEB-INF/
- **<u>新增</u>**disconf.properties
  - 配置作为disconf客户端所对应disconf服务器上的app及环境等信息
  - 路径：WEB-INF/classes/


- <u>**新增**</u>jar包如下,均为disconf必须的

  - disconf-client-2.6.36.jar
  - disconf-core-2.6.36.jar
  - reflections-0.9.9-RC1.jar
  - zookeeper-3.4.8.jar
  - jedis-2.9.0.jar (替换原来的jedis-2.4.1.jar）

- **<u>修改</u>**applicationContextRedis.xml

  - 建议直接使用测试上的applicationContextRedis.xml文件

  ​

### 2.在disconf服务器上新增配置文件

1. disconf服务器上新建app：p2papi

2. 新建配置文件：init.properties    redis.properties

   - init.properties为数据库连接配置，可以选择将生产环境的此文件上传到disconf
   - redis.properties，主要参考测试上的配置相应调整为生产的集群地址

3. 新建时对应一下选项

   - app	:p2papi
   - 版本：1.0.1
   - 环境：online
   - java托管：是
   - 实时更新：是
   - 上传方式：文件上传。文件可以从测试服disconf服务器上获取(环境为local)

   ​

### 3.关于disconf.properties

1. ```properties
   # 需要调整为生产的disconf服务器地址
   conf_server_host=disconf.xwjrfw.cn:8080
   ```

2. ```properties
   # 需要调整为上一步所选择的环境 ： online
   env=local
   ```

### 4.关于applicationContext-disconf.xml

无改动直接使用，放到WEB-INF/

### 5.关于jar包

jar包在测试服上已经处理，运行顺利。可以直接使用。

### 6.运行tomcat

观察日志是否启动正常，及时反馈沟通。

升级成功后，调用如下地址进行确认，内网调用的话url地址进行相应调整

localhost:8080/P2PSystem/hope/redis	// 存入5个过期时间为15秒的key

localhost:8080/P2PSystem/hope/redis/redis1	// 获取上面存入的

localhost:8080/P2PSystem/hope/codis

localhost:8080/P2PSystem/hope/codis/codis1

localhost:8080/P2PSystem/hope/setnxex	// 检测setnxex	指令







