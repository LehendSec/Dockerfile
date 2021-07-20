# Dockerfile

## ForgeRock AM远程代码执行漏洞 CVE-2021-35464
### Dockerfile

```dockerfile
FROM lehend/forgerock-openam-cve-2021-35464-rce:latest
EXPOSE 8080
```

## [Apache NiFi API 远程代码执行漏洞](https://www.cnblogs.com/lehend/p/14923049.html)

### 需要环境：

```
nifi-1.12.1.tar.gz
Dockerfile
jdk1.8.0_181.tar.gz
```

### Dockerfile

```dockerfile
FROM ubuntu:18.04
ENV LANG=C.UTF-8
ENV DEBIAN_FRONTEND=noninteractive
RUN sed -i s@/archive.ubuntu.com/@/mirrors.aliyun.com/@g /etc/apt/sources.list \
    && apt-get clean \
    && apt-get update \
    && apt-get install -y netcat-traditional

ADD jdk1.8.0_181.tar.gz /usr/local/    
ADD nifi-1.12.1.tar.gz /usr/local/

ENV JAVA_HOME /usr/local/jdk1.8.0_181
ENV CLASSPATH $JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
ENV PATH $PATH:$JAVA_HOME/bin

WORKDIR /usr/local/nifi-1.12.1/
RUN chmod -R 777 /usr/local/jdk1.8.0_181/bin/ \
    && chmod -R 777 /usr/local/nifi-1.12.1/bin/
EXPOSE 8080
CMD /usr/local/nifi-1.12.1/bin/nifi.sh start && tail -f /dev/null
```

## [Apache Shiro权限绕过漏洞-CVE-2020-11989](https://www.cnblogs.com/lehend/p/14885304.html)

### 需要环境：

```
打包的漏洞环境：demo-0.0.1-SNAPSHOT.jar
Dockerfile
```

### Dockerfile

```Dockerfile
FROM java:8
# 服务器只有dockerfile和jar在同级目录
COPY *.jar /app.jar
# CMD ["--server.port=8080"]
# 指定容器内要暴露的端口
EXPOSE 8088
ENTRYPOINT ["java","-jar","/app.jar"]
```

## [CVE-2019-12409 Apache Solr 远程代码执行漏洞](https://www.cnblogs.com/lehend/p/14876222.html)

### 需要的环境

```
Dockerfile 
solr-8.2.0
jdk1.8.0_181
```

### Dockerfile

```Dockerfile
FROM vulhub/solr:8.2.0
EXPOSE 8983
EXPOSE 18983
ENTRYPOINT ["/opt/docker-solr/scripts/docker-entrypoint.sh", "solr-demo"]
```

## [Spring Security OAuth2 远程命令执行漏洞 CVE-2016-4977](https://www.cnblogs.com/lehend/p/14842411.html)

### Dockerfile

```dockerfile
FROM maven

WORKDIR /tmp/
RUN wget http://secalert.net/research/cve-2016-4977.zip
RUN unzip cve-2016-4977.zip
RUN mv spring-oauth2-sec-bug/* /usr/src/mymaven

WORKDIR /usr/src/mymaven
RUN mvn clean install

CMD ["java", "-jar", "./target/demo-0.0.1-SNAPSHOT.jar"]
```

## [apache拒绝服务漏洞复现-CVE-2020-13935](https://www.cnblogs.com/lehend/p/14842416.html)

### 需要的环境

```bash
Dockerfile
apache-tomcat-8.5.51.tar.gz
jdk1.8.0_181.tar.gz
```

### **Dockerfile**

```dockerfile
FROM centos
ADD jdk1.8.0_181.tar.gz /usr/local/
ADD apache-tomcat-8.5.51.tar.gz /usr/local/
ENV MYPATH /usr/local
WORKDIR $MYPATH
#配置java与tomcat环境变量
ENV JAVA_HOME /usr/local/jdk1.8.0_181
ENV CLASSPATH $JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
ENV CATALINA_HOME /usr/local/apache-tomcat-8.5.51
ENV CATALINA_BASE /usr/local/apache-tomcat-8.5.51
ENV PATH $PATH:$JAVA_HOME/bin:$CATALINA_HOME/lib:$CATALINA_HOME/bin
#容器运行时监听的端口
EXPOSE 8080
#启动时运行tomcat
# ENTRYPOINT ["/usr/local/apache-tomcat-8.5.31/bin/startup.sh" ]
# CMD ["/usr/local/apache-tomcat-8.5.31/bin/catalina.sh","run"]
RUN chmod -R 777 /usr/local/jdk1.8.0_181/bin/ \
    && chmod -R 777 /usr/local/apache-tomcat-8.5.51/bin/
CMD /usr/local/apache-tomcat-8.5.51/bin/startup.sh && tail -F /usr/local/apache-tomcat-8.5.51/bin/logs/catalina.out
```

## [Spring Framework 反射型文件下载漏洞 CVE-2020-5421](https://www.cnblogs.com/lehend/p/14842417.html)

### **需要环境**

```bash
CVE-2020-5398-master/ 
#下载地址 https://github.com/motikan2010/CVE-2020-5398.git
```

### **Dockerfile**

```dockerfile
FROM java:8
# 由于网络原因，不建议在Dockerfile中使用
# git clone https://github.com/motikan2010/CVE-2020-5398.git

ADD CVE-2020-5398-master/ /CVE-2020-5398-master/
EXPOSE 8080
WORKDIR /CVE-2020-5398-master/
CMD ./gradlew bootrun && tail -f /dev/null
```

## [GhostScript 沙箱绕过 命令执行漏洞](https://www.cnblogs.com/lehend/p/14842418.html)

### 需要的环境

```bash
start.sh
Dockerfile
```

### Dockerfile

```dockerfile
FROM vulhub/imagemagick:7.0.8-27-php
# CMD ["php -t /var/www/html -S 0.0.0.0:8080"]
WORKDIR /var/www/html
ADD start.sh /start.sh
# RUN php -t /var/www/html -S 0.0.0.0:8080
ADD index.php /var/www/html/index.php
CMD ["/bin/bash","/start.sh"]
EXPOSE 8080
```

### **start.sh**

```bash
#!/bin/bash
php -t /var/www/html -S 0.0.0.0:8080
```

## [GhostScript 沙箱绕过 命令执行漏洞 CVE-2018-19475](https://www.cnblogs.com/lehend/p/14842419.html)

### 需要的环境

```bash
Dockerfile 
start.sh
```

### **Dockerfile**

```bash
FROM vulhub/imagemagick:7.0.8-20-php
# CMD ["php -t /var/www/html -S 0.0.0.0:8080"]
WORKDIR /var/www/html
ADD start.sh /start.sh
# RUN php -t /var/www/html -S 0.0.0.0:8080
ADD index.php /var/www/html/index.php
CMD ["/bin/bash","/start.sh"]
EXPOSE 8080
```

### **start.sh**

```bash
#!/bin/bash
php -t /var/www/html -S 0.0.0.0:8080
```

## [ntopng权限绕过 CVE-2021-28073](https://www.cnblogs.com/lehend/p/14842422.html)

### **环境**

```bash
nDPI 
#https://github.com/ntop/nDPI/archive/refs/tags/3.4.tar.gz 
ntopng 
# https://github.com/ntop/ntopng/archive/2154003f683e6483d0f5bd23780581ba5668ef49.tar.gz
start.sh #启动文件
Dockerfile
```

### **Dockerfile**

```bash
FROM ubuntu:18.04

ENV LANG=C.UTF-8
ENV DEBIAN_FRONTEND=noninteractive

RUN  sed -i s@/archive.ubuntu.com/@/mirrors.aliyun.com/@g /etc/apt/sources.list
RUN  apt-get clean
ADD nDPI /nDPI
ADD ntopng /ntopng
ADD start.sh /start.sh

RUN set -ex \
    && apt-get update \
    && apt-get install -y --no-install-recommends -y ca-certificates wget rrdtool \
    && apt-get install -y redis-server --no-install-recommends -y build-essential git bison flex libxml2-dev libpcap-dev libtool libtool-bin librrd-dev autoconf pkg-config automake autogen redis-server libsqlite3-dev libhiredis-dev libmaxminddb-dev libcurl4-openssl-dev libpango1.0-dev libcairo2-dev libnetfilter-queue-dev zlib1g-dev libssl-dev libcap-dev libnetfilter-conntrack-dev libreadline-dev libjson-c-dev libldap2-dev rename libsnmp-dev libpng-dev libzmq5-dev default-libmysqlclient-dev --fix-missing
WORKDIR /
RUN cd /nDPI \
    && ./autogen.sh \
    && ./configure \
    && make \
    && cd /ntopng \
    && ./autogen.sh \
    && ./configure \
    && make \
    && make install \
    && apt-get purge -y --auto-remove build-essential git bison flex autoconf pkg-config automake autogen redis-server libtool libtool-bin \
    && apt-get -y install redis-server

RUN ["chmod", "+x", "start.sh"]
# CMD ["bash"]
EXPOSE 3000
ENTRYPOINT "/start.sh"
```

### **start.sh**

```bash
#!/bin/bash
# start database
nohup redis-server >myoutfile1.log 2>&1 &
# start ntopng
# 后台运行服务
# nohup ntopng --http-port=0.0.0.0:3000 --redis=127.0.0.1:6379 >myoutfile.log 2>&1 &
ntopng --http-port=0.0.0.0:3000 --redis=127.0.0.1:6379
```

## [Webmin CVE-2021-31760 CSRF-RCE](https://www.cnblogs.com/lehend/p/14842423.html)

### 环境

```plaintext
Dockerfile
updatePasswd.sh
config
webmin_1.973_all.deb  
下载地址：http://prdownloads.sourceforge.net/webadmin/webmin_1.973_all.deb
readme.txt
```

### **Dockerfile**

```bash
FROM ubuntu:18.04
ENV LANG=C.UTF-8
ENV DEBIAN_FRONTEND=noninteractive
ADD webmin_1.973_all.deb /webmin_1.973_all.deb
ADD updatePasswd.sh /updatePasswd.sh
ADD config /config
WORKDIR /
RUN  sed -i s@/archive.ubuntu.com/@/mirrors.aliyun.com/@g /etc/apt/sources.list \
     && apt-get clean \
     && chmod 777 updatePasswd.sh
#镜像的操作指令
RUN chmod +x webmin_1.973_all.deb \
    && apt-get update \
    && apt-get install -y vim netcat lsof curl tcl tk expect curl perl libnet-ssleay-perl libauthen-pam-perl libio-pty-perl unzip shared-mime-info && apt --fix-broken install -y
RUN dpkg -i webmin_1.973_all.deb

ENV LC_ALL C.UTF-8

EXPOSE 10000

VOLUME ["/etc/webmin"]
CMD ./updatePasswd.sh && /usr/bin/touch /var/webmin/miniserv.log && /usr/sbin/service webmin restart && rm /etc/webmin/config && cp config /etc/webmin/config && /usr/bin/tail -f /var/webmin/miniserv.log
```

### **updatePasswd.sh**

```bash
#!/usr/bin/expect
# set timeout 30
spawn passwd
expect "*password:"
send "toor\r"
expect "*password:"
send "toor\r"
```

### **config**

```bash
passwd_pindex=1
passwd_file=/etc/shadow
tempdelete_days=7
passwd_mindex=4
passwd_cindex=2
path=/bin:/usr/bin:/sbin:/usr/sbin:/usr/local/bin
passwd_uindex=0
ld_env=LD_LIBRARY_PATH
find_pid_command=ps auwwwx | grep NAME | grep -v grep | awk '{ print $2 }'
by_view=0
os_type=debian-linux
os_version=9.0
real_os_type=Ubuntu Linux
real_os_version=18.04.5
lang=en
log=1
referers_none=0
md5pass=1
theme=authentic-theme
product=webmin
```

### **readme.txt**

```bash
账号密码：
root  toor
访问需要带上 https://
```

## [apache-solr-CVE-2020-13957-unacc-fileupload](https://www.cnblogs.com/lehend/p/14842425.html)

### **需要的环境**

```bash
solr-7.7.0.tar.gz
jdk1.8.0_181.tar.gz
/start.sh # 自动化启动脚本
```

### **自动化启动脚本内容**

```shell
#!/usr/bin/expect
set timeout 30
spawn solr start -e cloud -force
expect "*1-4"
send "\r"
expect "*8983"
send "\r"
expect "*7574"
send "\r"
expect "*gettingstarted"
send "\r"
expect "*split"
send "\r"
expect "*replicas"
send "\r"
expect "*_default"
send "\r"
```

### **Dockerfile**

```bash
FROM centos
ADD solr-7.7.0.tar.gz /
ADD jdk1.8.0_181.tar.gz /usr/local/
ADD start.sh /start.sh
ENV MYPATH /usr/local
WORKDIR $MYPATH
ENV JAVA_HOME /usr/local/jdk1.8.0_181
ENV CLASSPATH $JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
ENV PATH $PATH:$JAVA_HOME/bin
EXPOSE 8983
RUN chmod +x /start.sh \
    && chmod -R 777 /usr/local/jdk1.8.0_181/bin/ \
    && yum -y install lsof curl tcl tk expect \
    && ln -s /solr-7.7.0/bin/solr /usr/local/bin/
WORKDIR /
# ENTRYPOINT [/start.sh]
CMD ./start.sh && tail -f /dev/null
# CMD /solr-7.7.0/bin/solr start -force && tail -f /dev/null
# 下面的命令涉及到交互模式安装 solr start -e cloud -force
# CMD /solr-7.7.0/bin/solr start -e cloud -force && tail -f /dev/null
# CMD /usr/local/nacos/bin/startup.sh -m standalone && tail -f /dev/null
# CMD /usr/local/apache-tomcat-8.5.31/bin/startup.sh && tail -F /usr/local/apache-tomcat-8.5.31/bin/logs/catalina.out
```

## [Apache Solr 远程命令执行漏洞 CVE-2019-0193](https://www.cnblogs.com/lehend/p/14842428.html)

### **需要的环境 solr 8.6.2**

```plaintext
jdk1.8.0_181.tar.gz
solr-8.1.1.tar.gz
```

### **Dockerfile**

```bash
FROM centos
ADD  solr-8.1.1.tar.gz /
ADD jdk1.8.0_181.tar.gz /usr/local/
ENV MYPATH /usr/local
WORKDIR $MYPATH
ENV JAVA_HOME /usr/local/jdk1.8.0_181
ENV CLASSPATH $JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
ENV PATH $PATH:$JAVA_HOME/bin
EXPOSE 8983
RUN chmod -R 777 /usr/local/jdk1.8.0_181/bin/ \
    && yum -y install lsof curl \
    && ln -s /solr-8.1.1/bin/solr /usr/local/bin/
CMD /solr-8.1.1/bin/solr -e dih -force && tail -f /dev/null
```

## [Apache Solr任意文件读取漏洞 Dockerfile编写](https://www.cnblogs.com/lehend/p/14842429.html)

### **需要的环境 **

```
solr 8.6.2
jdk1.8.0_181.tar.gz
solr-8.6.2.tar.gz
Dockerfile
```

### Dockerfile

```dockerfile
FROM centos
ADD solr-8.6.2.tar.gz /
ADD jdk1.8.0_181.tar.gz /usr/local/
ENV MYPATH /usr/local
WORKDIR $MYPATH
ENV JAVA_HOME /usr/local/jdk1.8.0_181
ENV CLASSPATH $JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
ENV PATH $PATH:$JAVA_HOME/bin
EXPOSE 8983
RUN chmod -R 777 /usr/local/jdk1.8.0_181/bin/ \
    && yum -y install lsof curl \
    && ln -s /solr-8.6.2/bin/solr /usr/local/bin/
# CMD /solr-8.6.2/bin/solr create_core -c Ik_core && tail -f /dev/null
CMD /solr-8.6.2/bin/solr start -force && /solr-8.6.2/bin/solr create_core -c Ik_core -force && tail -f /dev/null
```

### **build**

```
docker build -t lehend/apache-solr-file-read:v8.6.2
```

### **运行**

```
docker run --name t2 -P -itd lehend/apache-solr-file-read:v8.6.2
```
