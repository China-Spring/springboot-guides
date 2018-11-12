# Scheduled定时任务

本指南将指导您完成使用Spring定时任务的步骤。

> 你要构建什么

您将构建一个应用程序，使用Spring的**@Scheduled**注解每五秒打印一次当前时间。

> 你需要什么

- 大约15分钟
- 最喜欢的文本编辑器或IDE
- [JDK 1.8](http://www.oracle.com/technetwork/java/javase/downloads/index.html)或更高版本
- [Gradle 4+](http://www.gradle.org/downloads)或[Maven 3.2+](https://maven.apache.org/download.cgi)
- 您还可以将代码直接导入IDE：

    - [使用STS构建/导入入门指南](http://www.springall.com.cn/article/94)
    - [使用IntelliJ IDEA导入入门指南](http://www.springall.com.cn/article/97)

> 如何完成本指南

与大多数Spring[入门指南](https://spring.io/guides)一样，您可以从头开始并完成每个步骤，或者您可以绕过您已熟悉的基本设置步骤。无论哪种方式，您最终都会使用工作代码。

要**从头开始**，请继续使用Gradle构建。

要**跳过基础知识**，请执行以下操作：

- [下载](https://github.com/spring-guides/gs-scheduling-tasks/archive/master.zip)并解压缩本指南的源存储库，或使用Git克隆它：

```bash
git clone https://github.com/spring-guides/gs-scheduling-tasks.git
```

- 进入**gs-scheduling-tasks/initial**
- 跳转到创建定时任务。

**完成后**，可以根据**gs-scheduling-tasks/complete**中的代码检查结果。

### 使用Gradle构建

首先，设置一个基本的构建脚本。在使用Spring构建应用程序时，您可以使用任何您喜欢的构建系统，但此处包含了使用[Gradle](http://gradle.org/)和[Maven](https://maven.apache.org/)所需的代码。如果您不熟悉这两者，请参阅[使用Gradle构建Java项目](http://www.springall.com.cn/article/80)或[使用Maven构建Java项目](http://www.springall.com.cn/article/81)。

> 创建目录结构

在您选择的项目目录中，创建以下子目录结构;例如，在\*nix系统上使用**mkdir -p src/main/java/hello**:

```bash
└── src
    └── main
        └── java
            └── hello
```

> 创建Gradle构建文件

下面是[最初的Gradle构建文件](https://github.com/spring-guides/gs-scheduling-tasks/blob/master/initial/build.gradle)。

```bash
buildscript {
    repositories {
        mavenCentral()
    }
    dependencies {
        classpath("org.springframework.boot:spring-boot-gradle-plugin:2.0.3.RELEASE")
    }
}

apply plugin: 'java'
apply plugin: 'eclipse'
apply plugin: 'idea'
apply plugin: 'org.springframework.boot'
apply plugin: 'io.spring.dependency-management'

bootJar {
    baseName = 'gs-scheduling-tasks'
    version =  '0.1.0'
}

repositories {
    mavenCentral()
}

sourceCompatibility = 1.8
targetCompatibility = 1.8

dependencies {
    compile("org.springframework.boot:spring-boot-starter")
    testCompile("junit:junit")
}
```

在[Spring Boot gradle plugin](https://docs.spring.io/spring-boot/docs/current/gradle-plugin/reference/html)提供了许多便捷的功能：

- 它收集类路径上的所有jar并构建一个可运行的“über-jar”，这使得执行和传输服务更加方便。
- 它搜索**public static void main()**标记为可运行类的方法。
- 它提供了一个内置的依赖项解析器，它设置版本号以匹配[Spring Boot依赖项](https://github.com/spring-projects/spring-boot/blob/master/spring-boot-project/spring-boot-dependencies/pom.xml)。您可以覆盖任何您希望的版本，但它将默认为Boot的所选版本集。

### 使用Maven构建

首先，设置一个基本的构建脚本。在使用Spring构建应用程序时，您可以使用任何您喜欢的构建系统，但此处包含了使用[Maven](https://maven.apache.org/)所需的代码。如果您不熟悉Maven，请参阅[使用Maven构建Java项目](http://www.springall.com.cn/article/81)。

> 创建目录结构

在您选择的项目目录中，创建以下子目录结构;例如，在\*nix系统上使用**mkdir -p src/main/java/hello**:

```bash
└── src
    └── main
        └── java
            └── hello
```

**pom.xml**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>org.springframework</groupId>
    <artifactId>gs-scheduling-tasks</artifactId>
    <version>0.1.0</version>

    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.0.3.RELEASE</version>
    </parent>

    <properties>
        <java.version>1.8</java.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter</artifactId>
        </dependency>
    </dependencies>

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

在[Spring Boot gradle plugin](https://docs.spring.io/spring-boot/docs/current/gradle-plugin/reference/html)提供了许多便捷的功能：

- 它收集类路径上的所有jar并构建一个可运行的“über-jar”，这使得执行和传输服务更加方便。
- 它搜索**public static void main()**标记为可运行类的方法。
- 它提供了一个内置的依赖项解析器，它设置版本号以匹配[Spring Boot依赖项](https://github.com/spring-projects/spring-boot/blob/master/spring-boot-project/spring-boot-dependencies/pom.xml)。您可以覆盖任何您希望的版本，但它将默认为Boot的所选版本集。

### 使用IDE构建

现在您已经设置了项目，可以创建定时任务。

**src/main/java/hello/ScheduledTasks.java**

```java
package hello;

import java.text.SimpleDateFormat;
import java.util.Date;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.scheduling.annotation.Scheduled;
import org.springframework.stereotype.Component;

@Component
public class ScheduledTasks {

    private static final Logger log = LoggerFactory.getLogger(ScheduledTasks.class);

    private static final SimpleDateFormat dateFormat = new SimpleDateFormat("HH:mm:ss");

    @Scheduled(fixedRate = 5000)
    public void reportCurrentTime() {
        log.info("The time is now {}", dateFormat.format(new Date()));
    }
}
```

**Scheduled**注解定义了特定方法何时运行。注意:这个示例使用**fixedRate**，它指定从每次调用的开始时间开始测量的方法调用之间的间隔。还有[其他选项](https://docs.spring.io/spring/docs/current/spring-framework-reference/html/scheduling.html#scheduling-annotation-support-scheduled)，比如**fixedDelay**，它指定从完成任务开始测量的调用间隔。您还可以使用**@Scheduled(cron="…")**[表达式来实现更复杂的任务调度](https://docs.spring.io/spring/docs/current/javadoc-api/org/springframework/scheduling/support/CronSequenceGenerator.html)。

> 启用任务

虽然定时任务可以嵌入到Web应用程序和WAR文件中，但下面演示的更简单的方法创建了一个独立的应用程序。您将所有内容打包在一个可执行的JAR文件中，由一个好的旧Java main()方法驱动。

**src/main/java/hello/Application.java**

```java
package hello;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.scheduling.annotation.EnableScheduling;

@SpringBootApplication
@EnableScheduling
public class Application {

    public static void main(String[] args) throws Exception {
        SpringApplication.run(Application.class);
    }
}
```

@SpringBootApplication 是一个便利注释，添加了以下所有内容：

- **@Configuration** 标记该类作为应用程序上下文的bean定义的源。
- **@EnableAutoConfiguration** 告诉Spring Boot开始根据类路径设置，其他bean和各种属性设置添加bean。
- 通常你会添加**@EnableWebMvc**一个Spring MVC应用程序，但Spring Boot会在类路径上看到spring-webmvc时自动添加它。这会将应用程序标记为Web应用程序并激活关键行为，例如设置DispatcherServlet。
- **@ComponentScan**告诉Spring在包中寻找其他组件，配置和服务，允许它找到控制器。

**main()**方法使用Spring Boot的**SpringApplication.run()**方法来启动应用程序。您是否注意到没有一行XML？也没有web.xml文件。此Web应用程序是100％纯Java，您无需处理配置任何管道或基础结构。

main()方法使用Spring Boot的SpringApplication.run()方法来启动应用程序。您是否注意到没有一行XML？也没有web.xml文件。此Web应用程序是100％纯Java，您无需处理配置任何管道或基础结构。

[@EnableScheduling](https://docs.spring.io/spring/docs/current/spring-framework-reference/htmlsingle/#scheduling-enable-annotation-support)确保创建后台任务执行程序。没有它，没有任何安排。

> 构建可执行的JAR

您可以使用Gradle或Maven从命令行运行该应用程序。或者，您可以构建一个包含所有必需依赖项，类和资源的可执行JAR文件，并运行该文件。这使得在整个开发生命周期中，跨不同环境等将服务作为应用程序发布，版本和部署变得容易。

如果您使用的是Gradle，则可以使用运行该应用程序**./gradlew bootRun**。或者您可以使用构建JAR文件**./gradlew build**。然后你可以运行JAR文件：

```bash
java -jar build/libs/gs-scheduling-tasks-0.1.0.jar
```

如果您使用的是Maven，则可以使用该应用程序运行该应用程序**./mvnw spring-boot:run**。或者您可以使用构建JAR文件**./mvnw clean package**。然后你可以运行JAR文件：

```bash
java -jar target/gs-scheduling-tasks-0.1.0.jar
```

**上面的过程将创建一个可运行的JAR。您也可以选择[构建经典WAR文件](http://www.springall.com.cn/article/96)。**

显示日志输出，您可以从日志中看到它在后台线程上。您应该每隔5秒钟看到计划任务：

```bash
[...]
2016-08-25 13:10:00.143  INFO 31565 --- [pool-1-thread-1] hello.ScheduledTasks : The time is now 13:10:00
2016-08-25 13:10:05.143  INFO 31565 --- [pool-1-thread-1] hello.ScheduledTasks : The time is now 13:10:05
2016-08-25 13:10:10.143  INFO 31565 --- [pool-1-thread-1] hello.ScheduledTasks : The time is now 13:10:10
2016-08-25 13:10:15.143  INFO 31565 --- [pool-1-thread-1] hello.ScheduledTasks : The time is now 13:10:15
```

> 概要

恭喜！您使用计划任务创建了一个应用程序。哎，实际代码比构建文件短！此技术适用于任何类型的应用程序。

