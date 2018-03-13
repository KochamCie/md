## ejb部署

1. 新建4g内存-64位-centos7-linux系统

2. 连无线网=桥接模式 ping www.baidu.com

3. 安装yum install lrzsz,安装 yum -y install vim*    yum install -y unzip zip

4. 使用xshell来操作比在虚拟鸡上操作方便。，

5. 安装jre

   - cd /usr/local      
   - rz  ***.rpm
   - rpm -ivh ***.rpm

6. 安装glassfish

   - cd /app上传glassfish.zip并unzip

   - 设置服务

     ``````bash
     [root@linuxidc ~]# cd /etc/init.d
     [root@linuxidc init.d]# vi glassfish
     #!/bin/bash
     # description: Glassfish Start Stop Restart
     # processname: glassfish
     # chkconfig: 234 20 80
     GLASSFISH_HOME=/app/glassfish4/glassfish
      
     case $1 in
     start)
     sh $GLASSFISH_HOME/bin/asadmin start-domain domain1
     ;;
     stop)
     sh $GLASSFISH_HOME/bin/asadmin stop-domain domain1
     ;;
     restart)
     sh $GLASSFISH_HOME/bin/asadmin stop-domain domain1
     sh $GLASSFISH_HOME/bin/asadmin start-domain domain1
     ;;
     esac
     exit 0 
     [root@linuxidc init.d]# chmod 755 glassfish

     ``````

7. 重置密码，关闭防火墙，启动，远程访问，ok