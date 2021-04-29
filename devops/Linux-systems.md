# Linux systems 

- [ ] [tomcat configure](#tomcat-configure)

## tomcat configure

tomcat 实例配置，开启多实例，开启apr  

目录结构：
    JAVA_HOME=/usr/local/jdk1.8.0_191
    CATALINA_HOME=/data/apache-tomcat-9.0.43
    CATALINA_BASE=/data/tomcat
        /data/tomcat
        ├── bin
        │   ├── catalina.sh
        │   ├── setenv.sh
        │   ├── shutdown.sh
        │   └── startup.sh
        ├── conf
        │   ├── server.xml
        │   └── web.xml
        ├── logs
        ├── temp
        ├── webapps
        └── work

启用apr

配置实例环境变量：

`cat /data/tomcat/bin/setenv.sh`

```text
JAVA_HOME=/usr/local/jdk1.8.0_191
CATALINA_HOME=/data/apache-tomcat-9.0.43
CATALINA_BASE=/data/jenkins
export LD_LIBRARY_PATH=/usr/local/apr/lib
```