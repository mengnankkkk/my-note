---
title: Spring boot(2)
categories:
  - 技术栈
  
abbrlink: 2701
date: 2024.7.4
tags: 
   - java 
   - spring boot

---

# Spring boot 

## 第一个简单的spring boot应用

spring boot简单的Maven目录结构

```ascii
springboot-hello
├── pom.xml
├── src
│   └── main
│       ├── java
│       └── resources
│           ├── application.yml
│           ├── logback-spring.xml
│           ├── static
│           └── templates
└── target
```

其中，在`src/main/resources`目录下，注意到几个文件：

### application.yml



这是Spring Boot默认的配置文件，它采用[YAML](https://yaml.org/)格式而不是`.properties`格式，文件名必须是`application.yml`而不是其他名称。

```
# application.yml

spring:
  application:
    name: ${APP_NAME:unnamed}
  datasource:
    url: jdbc:hsqldb:file:testdb
    username: sa
    password:
    driver-class-name: org.hsqldb.jdbc.JDBCDriver
    hikari:
      auto-commit: false
      connection-timeout: 3000
      validation-timeout: 3000
      max-lifetime: 60000
      maximum-pool-size: 20
      minimum-idle: 1
```

可见，YAML是一种层级格式，它和`.properties`很容易互相转换，它的优点是去掉了大量重复的前缀，并且更加易读。

也可以使用application.properties作为配置文件，但不如YAML格式简单。

### 使用环境变量

在配置文件中，我们经常使用如下的格式对某个key进行配置：

```
app:
  db:
    host: ${DB_HOST:localhost}
    user: ${DB_USER:root}
    password: ${DB_PASSWORD:password}
    #简单的数据库配置
```



这种`${DB_HOST:localhost}`意思是，首先从环境变量查找`DB_HOST`，如果环境变量定义了，那么使用环境变量的值，否则，使用默认值`localhost`。

这使得我们在开发和部署时更加方便，因为开发时无需设定任何环境变量，直接使用默认值即本地数据库，而实际线上运行的时候，只需要传入环境变量即可：

```
$ DB_HOST=10.0.1.123 DB_USER=prod DB_PASSWORD=xxxx java -jar xxx.jar
```

### logback-spring.xml

这是Spring Boot的logback配置文件名称（也可以使用`logback.xml`），一个标准的写法如下：

```
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
    <include resource="org/springframework/boot/logging/logback/defaults.xml" />

    <appender name="CONSOLE" class="ch.qos.logback.core.ConsoleAppender">
        <encoder>
            <pattern>${CONSOLE_LOG_PATTERN}</pattern>
            <charset>utf8</charset>
        </encoder>
    </appender>

    <appender name="APP_LOG" class="ch.qos.logback.core.rolling.RollingFileAppender">
        <encoder>
            <pattern>${FILE_LOG_PATTERN}</pattern>
            <charset>utf8</charset>
        </encoder>
          <file>app.log</file>
        <rollingPolicy class="ch.qos.logback.core.rolling.FixedWindowRollingPolicy">
            <maxIndex>1</maxIndex>
            <fileNamePattern>app.log.%i</fileNamePattern>
        </rollingPolicy>
        <triggeringPolicy class="ch.qos.logback.core.rolling.SizeBasedTriggeringPolicy">
            <MaxFileSize>1MB</MaxFileSize>
        </triggeringPolicy>
    </appender>

    <root level="INFO">
        <appender-ref ref="CONSOLE" />
        <appender-ref ref="APP_LOG" />
    </root>
</configuration>
```

它主要通过`<include resource="..." />`引入了Spring Boot的一个缺省配置，这样我们就可以引用类似`${CONSOLE_LOG_PATTERN}`这样的变量。上述配置定义了一个控制台输出和文件输出，可根据需要修改。

`static`是静态文件目录，`templates`是模板文件目录，注意它们不再存放在`src/main/webapp`下，而是直接放到`src/main/resources`这个classpath目录，因为在Spring Boot中已经不需要专门的webapp目录了。

### 源码目录结构：

#### 启动主类

```ascii
src/main/java
└── com
    └── itranswarp
        └── learnjava
            ├── Application.java
            ├── entity
            │   └── User.java
            ├── service
            │   └── UserService.java
            └── web
                └── UserController.java
```

在存放源码的`src/main/java`目录中，Spring Boot对Java包的层级结构有一个要求。注意到我们的根package是`com.itranswarp.learnjava`，下面还有`entity`、`service`、`web`等子package。Spring Boot要求`main()`方法所在的启动类必须放到根package下，命名不做要求，这里我们以`Application.java`命名，它的内容如下：

```
@SpringBootApplication
public class Application {
    public static void main(String[] args) throws Exception {
        SpringApplication.run(Application.class, args);
    }
}
```

启动Spring Boot应用程序只需要一行代码加上一个注解`@SpringBootApplication`，该注解实际上又包含了：

- @SpringBootConfiguration
  - @Configuration
- @EnableAutoConfiguration
  - @AutoConfigurationPackage
- @ComponentScan

#### pom.xml

```
<project ...>
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>3.0.0</version>
    </parent>

    <modelVersion>4.0.0</modelVersion>
    <groupId>com.itranswarp.learnjava</groupId>
    <artifactId>springboot-hello</artifactId>
    <version>1.0-SNAPSHOT</version>

    <properties>
        <maven.compiler.source>17</maven.compiler.source>
        <maven.compiler.target>17</maven.compiler.target>
        <java.version>17</java.version>
        <pebble.version>3.2.0</pebble.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-jdbc</artifactId>
        </dependency>

        <!-- 集成Pebble View -->
        <dependency>
            <groupId>io.pebbletemplates</groupId>
            <artifactId>pebble-spring-boot-starter</artifactId>
            <version>${pebble.version}</version>
        </dependency>

        <!-- JDBC驱动 -->
        <dependency>
            <groupId>org.hsqldb</groupId>
            <artifactId>hsqldb</artifactId>
        </dependency>
    </dependencies>
</project>
```

使用Spring Boot时，强烈推荐从`spring-boot-starter-parent`继承，因为这样就可以引入Spring Boot的预置配置。

紧接着，我们引入了依赖`spring-boot-starter-web`和`spring-boot-starter-jdbc`，它们分别引入了Spring MVC相关依赖和Spring JDBC相关依赖，无需指定版本号，因为引入的`<parent>`内已经指定了，只有我们自己引入的某些第三方jar包需要指定版本号。这里我们引入`pebble-spring-boot-starter`作为View，以及`hsqldb`作为嵌入式数据库。`hsqldb`已在`spring-boot-starter-jdbc`中预置了版本号`3.0.0`，因此此处无需指定版本号。

根据`pebble-spring-boot-starter`的[文档](https://pebbletemplates.io/wiki/guide/spring-boot-integration/)，加入如下配置到`application.yml`：

```
pebble:
  # 默认为".peb"，改为"":
  suffix:
  # 开发阶段禁用模板缓存:
  cache: false
```

对`Application`稍作改动，添加`WebMvcConfigurer`这个Bean：

```
@SpringBootApplication
public class Application {
    ...

    @Bean
    WebMvcConfigurer createWebMvcConfigurer(@Autowired HandlerInterceptor[] interceptors) {
        return new WebMvcConfigurer() {
            @Override
            public void addResourceHandlers(ResourceHandlerRegistry registry) {
                // 映射路径`/static/`到classpath路径:
                registry.addResourceHandler("/static/**")
                        .addResourceLocations("classpath:/static/");
            }
        };
   
```

#### 日志

启动后观察日志

```
  .   ____          _            __ _ _
 /\\ / ___'_ __ _ _(_)_ __  __ _ \ \ \ \
( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
 \\/  ___)| |_)| | | | | || (_| |  ) ) ) )
  '  |____| .__|_| |_|_| |_\__, | / / / /
 =========|_|==============|___/=/_/_/_/
 :: Spring Boot ::                (v3.0.0)

2022-11-25T10:49:31.100+08:00  INFO 13105 --- [           main] com.itranswarp.learnjava.Application     : Starting Application using Java 17 with PID 13105 (/Users/liaoxuefeng/Git/springboot-hello/target/classes started by liaoxuefeng in /Users/liaoxuefeng/Git/springboot-hello)
2022-11-25T10:49:31.107+08:00  INFO 13105 --- [           main] com.itranswarp.learnjava.Application     : No active profile set, falling back to 1 default profile: "default"
2022-11-25T10:49:32.404+08:00  INFO 13105 --- [           main] o.s.b.w.embedded.tomcat.TomcatWebServer  : Tomcat initialized with port(s): 8080 (http)
2022-11-25T10:49:32.423+08:00  INFO 13105 --- [           main] o.apache.catalina.core.StandardService   : Starting service [Tomcat]
2022-11-25T10:49:32.426+08:00  INFO 13105 --- [           main] o.apache.catalina.core.StandardEngine    : Starting Servlet engine: [Apache Tomcat/10.1.1]
2022-11-25T10:49:32.549+08:00  INFO 13105 --- [           main] o.a.c.c.C.[Tomcat].[localhost].[/]       : Initializing Spring embedded WebApplicationContext
2022-11-25T10:49:32.551+08:00  INFO 13105 --- [           main] w.s.c.ServletWebServerApplicationContext : Root WebApplicationContext: initialization completed in 1327 ms
2022-11-25T10:49:32.668+08:00  WARN 13105 --- [           main] com.zaxxer.hikari.HikariConfig           : HikariPool-1 - idleTimeout is close to or more than maxLifetime, disabling it.
2022-11-25T10:49:32.669+08:00  INFO 13105 --- [           main] com.zaxxer.hikari.HikariDataSource       : HikariPool-1 - Starting...
2022-11-25T10:49:32.996+08:00  INFO 13105 --- [           main] com.zaxxer.hikari.pool.PoolBase          : HikariPool-1 - Driver does not support get/set network timeout for connections. (feature not supported)
2022-11-25T10:49:32.998+08:00  INFO 13105 --- [           main] com.zaxxer.hikari.pool.HikariPool        : HikariPool-1 - Added connection org.hsqldb.jdbc.JDBCConnection@31a2a9fa
2022-11-25T10:49:33.002+08:00  INFO 13105 --- [           main] com.zaxxer.hikari.HikariDataSource       : HikariPool-1 - Start completed.
2022-11-25T10:49:33.391+08:00  WARN 13105 --- [           main] ocalVariableTableParameterNameDiscoverer : Using deprecated '-debug' fallback for parameter name resolution. Compile the affected code with '-parameters' instead or avoid its introspection: io.pebbletemplates.boot.autoconfigure.PebbleServletWebConfiguration
2022-11-25T10:49:33.398+08:00  WARN 13105 --- [           main] ocalVariableTableParameterNameDiscoverer : Using deprecated '-debug' fallback for parameter name resolution. Compile the affected code with '-parameters' instead or avoid its introspection: io.pebbletemplates.boot.autoconfigure.PebbleAutoConfiguration
2022-11-25T10:49:33.619+08:00  INFO 13105 --- [           main] o.s.b.w.embedded.tomcat.TomcatWebServer  : Tomcat started on port(s): 8080 (http) with context path ''
2022-11-25T10:49:33.637+08:00  INFO 13105 --- [           main] com.itranswarp.learnjava.Application     : Started Application in 3.151 seconds (process running for 3.835)
```

Spring Boot自动启动了嵌入式Tomcat，当看到`Started Application in xxx seconds`时，Spring Boot应用启动成功。

我们在浏览器输入`localhost:8080`就可以直接访问页面。

Spring Boot的特色：AutoConfiguration

当我们引入`spring-boot-starter-jdbc`时，启动时会自动扫描所有的`XxxAutoConfiguration`：

- `DataSourceAutoConfiguration`：自动创建一个`DataSource`，其中配置项从`application.yml`的`spring.datasource`读取；
- `DataSourceTransactionManagerAutoConfiguration`：自动创建了一个基于JDBC的事务管理器；
- `JdbcTemplateAutoConfiguration`：自动创建了一个`JdbcTemplate`。

因此，我们自动得到了一个`DataSource`、一个`DataSourceTransactionManager`和一个`JdbcTemplate`。

类似的，当我们引入`spring-boot-starter-web`时，自动创建了：

- `ServletWebServerFactoryAutoConfiguration`：自动创建一个嵌入式Web服务器，默认是Tomcat；
- `DispatcherServletAutoConfiguration`：自动创建一个`DispatcherServlet`；
- `HttpEncodingAutoConfiguration`：自动创建一个`CharacterEncodingFilter`；
- `WebMvcAutoConfiguration`：自动创建若干与MVC相关的Bean。
- ...

引入第三方`pebble-spring-boot-starter`时，自动创建了：

- `PebbleAutoConfiguration`：自动创建了一个`PebbleViewResolver`。

Spring Boot大量使用`XxxAutoConfiguration`来使得许多组件被自动化配置并创建，而这些创建过程又大量使用了Spring的Conditional功能。例如，我们观察`JdbcTemplateAutoConfiguration`

如果我们自己创建了一个`JdbcTemplate`，例如，在`Application`中自己写个方法：

```
@SpringBootApplication
public class Application {
    ...
    @Bean
    JdbcTemplate createJdbcTemplate(@Autowired DataSource dataSource) {
        return new JdbcTemplate(dataSource);
    }
}
```

Spring Boot就不会再创建一个重复的`JdbcTemplate`

## 开发者工具

Spring Boot提供了一个开发者工具，可以监控classpath路径上的文件。只要源码或配置文件发生修改，Spring Boot应用可以自动重启。在开发阶段，这个功能比较有用。

要使用这一开发者功能，我们只需添加如下依赖到`pom.xml`：

```
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-devtools</artifactId>
</dependency>
```

默认配置下，针对`/static`、`/public`和`/templates`目录中的文件修改，不会自动重启，因为禁用缓存后，这些文件的修改可以实时更新。

## 打包spring boot

在Spring Boot应用中，打包更加简单，因为Spring Boot自带一个更简单的`spring-boot-maven-plugin`插件用来打包，我们只需要在`pom.xml`中加入以下配置：

```
<project ...>
    ...
    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>
</project>
```

```
 mvn clean package
```

输入maven命令即可自动打包

之后可以在target看到

```
classes
generated-sources
maven-archiver
maven-status
springboot-exec-jar-1.0-SNAPSHOT.jar
springboot-exec-jar-1.0-SNAPSHOT.jar.original
```

其中，`springboot-exec-jar-1.0-SNAPSHOT.jar.original`是Maven标准打包插件打的jar包，它只包含我们自己的Class，不包含依赖，而`springboot-exec-jar-1.0-SNAPSHOT.jar`是Spring Boot打包插件创建的包含依赖的jar，可以直接运行：

## 瘦身spring boot

如何只打包我们自己编写的代码，同时又自动把依赖包下载到某处，并自动引入到classpath中。解决方案就是使用`spring-boot-thin-launcher`。

首先复制一份上一节的Maven项目，并重命名为`springboot-thin-jar`：

```
<project ...>
    ...
    <groupId>com.itranswarp.learnjava</groupId>
    <artifactId>springboot-thin-jar</artifactId>
    <version>1.0-SNAPSHOT</version>
    ...
```

然后，修改`<build>`-`<plugins>`-`<plugin>`，给原来的`spring-boot-maven-plugin`增加一个`<dependency>`如下：

```
<project ...>
    ...
    <build>
        <finalName>awesome-app</finalName>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
                <dependencies>
                    <dependency>
                        <groupId>org.springframework.boot.experimental</groupId>
                        <artifactId>spring-boot-thin-layout</artifactId>
                        <version>1.0.27.RELEASE</version>
                    </dependency>
                </dependencies>
            </plugin>
        </plugins>
    </build>
</project>
```

不需要任何其他改动了，我们直接按正常的流程打包，执行`mvn clean package`，观察`target`目录最终生成的可执行`awesome-app.jar`，只有79KB左右。****