## upjob升级文档



### 0.备份生产代码



### 1. 获取源码以及本版本的改动说明

从测试服务器上获取最新代码。这次新增及修改的内容如下：

- 新增applicationContext-disconf.xml
  - disconf的spring配置文件
  - 路径：WEB-INF/
- 新增disconf.properties
  - 配置作为disconf客户端所对应disconf服务器上的app及环境等信息
  - 路径：WEB-INF/classes/


- 新增jar包如下,均为disconf必须的
  - aspectjtools-1.8.5.jar
  - commmons-lang3-3.0.jar
  - disconf-client-2.6.36.jar
  - disconf-core-2.6.36.jar
  - gson-2.0.jar
  - httpclient-4.4.1.jar  (<u>**此jar包需要替换旧的4.2.1版本的**</u>)
  - httpcore-4.4.1.jar
  - reflections-0.9.9-RC1.jar
  - zookeeper-3.4.8.jar
- 修改原有的代码，增加关键日志输出
- 修改获取【用户登陆状态在upjob上的过期时间】对接到disconf的托管配置项
  - 新增配置项对应的javabean类RemoveUseridConfig.class，路径WEB-INF/classes/com/xwjr/rest/disconf/

### 2.在disconf服务器上新增配置文件

1. disconf服务器上新建app：upjob

2. 新建配置文件：remove-userid.properties

   - app	:upjob
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

升级成功后，在disconf服务器上编辑remove-userid.properties文件中的配置项的值，可以在upjob上实时生效。





