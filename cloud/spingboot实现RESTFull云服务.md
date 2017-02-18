# SpringBoot实现RESTFull的webservice

主要碰到的问题:

* 环境的搭建
* release时碰到的问题
* https的支持


## 环境的搭建

使用intellij开发，构建使用maven。在porm.xml文件添加以下配置
```xml
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>1.5.1.RELEASE</version>
    </parent>

    <dependencies>
        <dependency>
            <groupId>redis.clients</groupId>
            <artifactId>jedis</artifactId>
            <version>2.9.0</version>
            <type>jar</type>
            <scope>compile</scope>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

    </dependencies>

```


## releasee时碰到的问题

右键单击项目名称，Open Module Settings->Project Settings->Artifacts 添加一个新的artifact 。 选择 copy to the output directory and link via manifest。注意META-INF/MANIFEST.INF 应该放在src\main\resources下面，在添加artifact前保证该目下没有MANIFEST.INF文件。



## HTTPS的支持

在src/main/resources下创建application.properties文件，加入下面内容

```
server.port: 8443
server.ssl.key-store: keystore.p12
server.ssl.key-store-password: mediawin999
server.ssl.keyStoreType: PKCS12
server.ssl.keyAlias: tomcat
```

生成key的命令：

```
 keytool -genkey -alias tomcat -storetype PKCS12 -keyalg RSA -keysize 2048 -keystore keystore.p12 -validity 3650
```