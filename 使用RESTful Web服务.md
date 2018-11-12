# 使用RESTful Web服务

本指南将指导您完成创建使用RESTful Web服务的应用程序的过程。

> 你要构建什么

您将构建一个使用Spring RestTemplate的应用程序，以便在[http://gturnquist-quoters.cfapps.io/api/random](http://gturnquist-quoters.cfapps.io/api/random)中检索随机的Spring Boot引用。

> 你需要什么

- 大约15分钟
- 最喜欢的文本编辑器或IDE
- [JDK 1.8](http://www.oracle.com/technetwork/java/javase/downloads/index.html)或更高版本
- [Gradle 4+](http://www.gradle.org/downloads)或[Maven 3.2+](https://maven.apache.org/download.cgi)
- 您还可以将代码直接导入IDE：

    - [Spring Tool Suite (STS)](https://spring.io/guides/gs/sts)
    - [IntelliJ IDEA](https://spring.io/guides/gs/intellij-idea/)

> 如何完成本指南

与大多数Spring[入门指南](https://spring.io/guides)一样，您可以从头开始并完成每个步骤，或者您可以绕过您已熟悉的基本设置步骤。无论哪种方式，您最终都会使用工作代码。

要**从头开始**，请继续使用Gradle构建。

要**跳过基础知识**，请执行以下操作：

- [下载](https://github.com/spring-guides/gs-consuming-rest/archive/master.zip)并解压缩本指南的源存储库，或者使用[Git](https://spring.io/understanding/Git): 

```bash
git clone https://github.com/spring-guides/gs-consuming-rest.git
```

- 进入**gs-consuming-rest/initial**目录
- 跳转到获取REST资源。

**完成后**，可以根据**gs-consuming-rest/complete**中的代码检查结果。

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

下面是[最初的Gradle构建文件](https://github.com/spring-guides/gs-rest-service/blob/master/initial/build.gradle)。

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
    baseName = 'gs-consuming-rest'
    version =  '0.1.0'
}

repositories {
    mavenCentral()
}

sourceCompatibility = 1.8
targetCompatibility = 1.8

dependencies {
    compile("org.springframework.boot:spring-boot-starter")
    compile("org.springframework:spring-web")
    compile("com.fasterxml.jackson.core:jackson-databind")
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
    <artifactId>gs-consuming-rest</artifactId>
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
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-web</artifactId>
        </dependency>
        <dependency>
            <groupId>com.fasterxml.jackson.core</groupId>
            <artifactId>jackson-databind</artifactId>
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

- 阅读如何将本指南直接导入[Spring Tool Suite](http://www.springall.com.cn/article/94)。
- 阅读[IntelliJ IDEA](http://www.springall.com.cn/article/97)中如何使用本指南。

> 获取REST资源

完成项目设置后，您可以创建一个使用RESTful服务的简单应用程序。

RESTful服务已经在[http://gturnquist-quoters.cfapps.io/api/random](http://gturnquist-quoters.cfapps.io/api/random)启动。它随机获取有关Spring Boot的引用并将它们作为JSON文档返回。

如果您通过Web浏览器或curl请求该URL，您将收到如下所示的JSON文档：

```json
{
   type: "success",
   value: {
      id: 10,
      quote: "Really loving Spring Boot, makes stand alone Spring apps easy."
   }
}
```

很简单，但通过浏览器或curl获取时并不十分有用。

以编程方式使用REST Web服务是更有用的方法。为了帮助您完成该任务，Spring提供了一个方便的模板类RestTemplate。**RestTemplate**使与大多数RESTful服务的交互成为一行咒语。它甚至可以将数据绑定到自定义域类型。

首先，创建一个包含所需数据的域类。

**src/main/java/hello/Quote.java**

```java
package hello;

import com.fasterxml.jackson.annotation.JsonIgnoreProperties;

@JsonIgnoreProperties(ignoreUnknown = true)
public class Quote {

    private String type;
    private Value value;

    public Quote() {
    }

    public String getType() {
        return type;
    }

    public void setType(String type) {
        this.type = type;
    }

    public Value getValue() {
        return value;
    }

    public void setValue(Value value) {
        this.value = value;
    }

    @Override
    public String toString() {
        return "Quote{" +
                "type='" + type + '\'' +
                ", value=" + value +
                '}';
    }
}
```

如您所见，这是一个简单的Java类，具有一些属性和匹配的getter方法。它使用Jackson JSON处理库进行**@JsonIgnoreProperties**注解，以指示应忽略此类型中未绑定的任何属性。

为了直接将数据绑定到自定义类型，您需要指定与从API返回的JSON文档中的键完全相同的变量名称。如果您的JSON doc中的变量名称和键不匹配，则需要使用**@JsonProperty**注解指定JSON文档的确切键。

嵌入内部引用本身需要一个额外的类。

**src/main/java/hello/Value.java**

```java
package hello;

import com.fasterxml.jackson.annotation.JsonIgnoreProperties;

@JsonIgnoreProperties(ignoreUnknown = true)
public class Value {

    private Long id;
    private String quote;

    public Value() {
    }

    public Long getId() {
        return this.id;
    }

    public String getQuote() {
        return this.quote;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public void setQuote(String quote) {
        this.quote = quote;
    }

    @Override
    public String toString() {
        return "Value{" +
                "id=" + id +
                ", quote='" + quote + '\'' +
                '}';
    }
}
```

它使用相同的注解，但只是映射到其他数据字段。

> 使应用程序可执行

虽然可以将此服务打包为传统的[WAR](https://spring.io/understanding/WAR)文件以部署到外部应用程序服务器，但下面演示的更简单的方法创建了一个独立的应用程序。您将所有内容打包在一个可执行的JAR文件中，由一个好的旧Java main()方法驱动。在此过程中，您使用Spring的支持将[Tomcat](https://spring.io/understanding/Tomcat) servlet容器嵌入为HTTP运行时，而不是部署到外部实例。

现在，您可以编写Application用于**RestTemplate**从Spring Boot报价服务中获取数据的类。

**src/main/java/hello/Application.java**

```java
package hello;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.web.client.RestTemplate;

public class Application {

    private static final Logger log = LoggerFactory.getLogger(Application.class);

    public static void main(String args[]) {
        RestTemplate restTemplate = new RestTemplate();
        Quote quote = restTemplate.getForObject("http://gturnquist-quoters.cfapps.io/api/random", Quote.class);
        log.info(quote.toString());
    }

}
```

由于Jackson JSON处理库位于类路径中，因此RestTemplate将使用它（通过[消息转换器](https://docs.spring.io/spring/docs/current/javadoc-api/org/springframework/http/converter/HttpMessageConverter.html)）将传入的JSON数据转换为**Quote**对象。从那里，Quote对象的内容将被记录到控制台。

在这里，您只用于RestTemplate发出HTTP GET请求。但RestTemplate也支持其他HTTP请求，如POST，PUT和DELETE。

> 使用Spring Boot管理应用程序生命周期

到目前为止，我们还没有在我们的应用程序中使用Spring Boot，但这样做有一些优点，并且不难做到。其中一个优点是我们可能希望让Spring Boot管理消息转换器**RestTemplate**，以便易于以声明方式添加自定义。为此，我们**@SpringBootApplication**在主类上使用并转换main方法以启动它，就像在任何Spring Boot应用程序中一样。最后，我们将其RestTemplate移至**CommandLineRunner**回调，以便在启动时由Spring Boot执行：

**src/main/java/hello/Application.java**

```java
package hello;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.boot.CommandLineRunner;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.boot.web.client.RestTemplateBuilder;
import org.springframework.context.annotation.Bean;
import org.springframework.web.client.RestTemplate;

@SpringBootApplication
public class Application {

	private static final Logger log = LoggerFactory.getLogger(Application.class);

	public static void main(String args[]) {
		SpringApplication.run(Application.class);
	}

	@Bean
	public RestTemplate restTemplate(RestTemplateBuilder builder) {
		return builder.build();
	}

	@Bean
	public CommandLineRunner run(RestTemplate restTemplate) throws Exception {
		return args -> {
			Quote quote = restTemplate.getForObject(
					"http://gturnquist-quoters.cfapps.io/api/random", Quote.class);
			log.info(quote.toString());
		};
	}
}
```

**RestTemplateBuilder**是由Spring注入的，如果您使用它创建RestTemplate，那么您将从SpringBoot中使用消息转换器和请求工厂进行的所有自动配置中获益。我们还将**RestTemplate**提取到**@Bean**中，使其更易于测试(这样就可以更容易地对其进行模拟)。

> 构建可执行的JAR

您可以使用Gradle或Maven从命令行运行该应用程序。或者，您可以构建一个包含所有必需依赖项，类和资源的可执行JAR文件，并运行该文件。这使得在整个开发生命周期中，跨不同环境等将服务作为应用程序发布，版本和部署变得容易。

如果您使用的是Gradle，则可以使用运行该应用程序**./gradlew bootRun**。或者您可以使用构建JAR文件**./gradlew build**。然后你可以运行JAR文件：

```bash
java -jar build/libs/gs-consuming-rest-0.1.0.jar
```

如果您使用的是Maven，则可以使用该应用程序运行该应用程序**./mvnw spring-boot:run**。或者您可以使用构建JAR文件**./mvnw clean package**。然后你可以运行JAR文件：

```bash
java -jar target/gs-consuming-rest-0.1.0.jar
```

上面的过程将创建一个可运行的JAR。您也可以选择[构建经典WAR文件](http://www.springall.com.cn/article/96)。

你应该看到如下的输出，随机引用：

```bash
2015-09-23 14:22:26.415  INFO 23613 --- [main] hello.Application  : Quote{type='success', value=Value{id=12, quote='@springboot with @springframework is pure productivity! Who said in #java one has to write double the code than in other langs? #newFavLib'}}
```

**如果您看到错误，Could not extract response: no suitable HttpMessageConverter found for response type [class hello.Quote]那么您可能处于无法连接到后端服务的环境中（如果您可以访问它，则会发送JSON）。也许公司代理问题？尝试设置标准系统属性http.proxyHost和http.proxyPort适合您环境。**

> 概要

恭喜！您刚刚使用Spring开发了一个简单的REST客户端。


