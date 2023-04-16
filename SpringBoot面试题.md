# 一、SpringBoot 是什么？

Spring Boot 是一个开源的 Java 框架，它简化了基于 Spring 框架的应用程序的开发和部署过程。Spring Boot 旨在使创建独立、生产级的基于 Spring 的应用程序变得更快、更简单。它实现了约定优于配置的原则，提供了许多默认的配置和内置功能，从而减少了开发者的配置工作。

Spring Boot 的主要特性包括：

1. 创建可执行的独立 Java 应用程序：可以将 Spring Boot 应用程序打包成一个独立的 JAR 文件，内嵌了一个 Web 服务器（如 Tomcat、Jetty 或 Undertow），使得应用程序可以直接运行，而无需额外的容器部署。
2. 自动配置：Spring Boot 会根据项目中的依赖关系自动配置所需的组件。这使得开发过程更简单、更快速，因为开发者不需要手动配置许多底层组件。
3. 起步依赖：通过起步依赖（Starter Dependencies），Spring Boot 提供了一种简单的方式来管理项目中的依赖关系。这些起步依赖将常用库和框架进行了预定义和封装，使得在开发过程中添加和管理这些库变得更加方便。
4. Actuator：Spring Boot Actuator 提供了生产级的功能，如监控、度量和应用程序的运行状况检查。这有助于管理和维护部署在生产环境中的应用程序。
5. 外部配置：Spring Boot 允许使用外部配置文件（如 YAML 或 properties 文件）来轻松管理应用程序的配置信息，使得在不修改代码的情况下，轻松更改应用程序的行为。

总之，Spring Boot 通过提供自动配置、简化依赖管理和易于部署的应用程序，使得基于 Spring 的应用程序开发变得更加高效和简单。



## 1. 约定优于配置的原则，举例说明

"约定优于配置"（Convention over Configuration）是一种软件设计原则，意味着框架或库在很大程度上通过预定义的约定来提供默认行为，从而减少开发者需要进行的配置工作。这种原则的核心思想是，只要遵循一定的约定和规范，开发者就能够使用预设的默认配置，而不需要编写大量的配置代码。这有助于简化应用程序的开发过程，提高开发效率。

Spring Boot 实现了约定优于配置的原则，提供了许多默认配置和内置功能，降低了开发者的配置负担。以下是一些具体的例子：

1. 内嵌的 Web 服务器：Spring Boot 根据项目中的依赖关系自动选择合适的内嵌 Web 服务器（如 Tomcat、Jetty 或 Undertow），并使用默认配置启动它。开发者不需要在应用程序中手动配置 Web 服务器。
2. 数据源自动配置：如果在项目中添加了相关依赖（如 H2、MySQL 或 PostgreSQL 数据库驱动），Spring Boot 会自动配置一个数据源。开发者只需要在配置文件中指定数据库连接信息，无需编写额外的数据源配置代码。
3. JPA 实体管理器和事务管理器：如果在项目中添加了 JPA 相关的依赖，Spring Boot 会自动配置一个实体管理器工厂（EntityManagerFactory）和事务管理器（TransactionManager）。开发者只需遵循 Spring Data JPA 的命名约定，就可以轻松实现数据访问层的功能，而无需编写大量的配置代码。
4. Spring MVC：当项目中包含 Spring MVC 相关依赖时，Spring Boot 会自动配置一个 DispatcherServlet，并设置默认的视图解析器、消息转换器等。开发者只需按照约定编写控制器和视图模板，即可快速构建 Web 应用程序。

以上这些例子展示了 Spring Boot 如何通过实现约定优于配置的原则，为开发者提供许多预设的默认配置和内置功能，减轻了开发者的配置工作，使得整个开发过程更加简单、高效。

## 2. Spring Boot是如何根据约定找到默认配置的，请已内置tomcat、jetty为例说明

Spring Boot 根据项目中的依赖关系自动选择合适的内嵌 Web 服务器，主要是通过 Maven 或 Gradle 构建系统中的依赖声明实现的。Spring Boot 通过检查项目的 classpath（类路径）来确定应该使用哪个 Web 服务器。当它发现特定的 Web 服务器依赖时，会自动配置该 Web 服务器并使用默认设置启动它。

以下是使用 Maven 配置默认使用 Tomcat 和 Jetty 的两个案例：

### 1. 默认使用 Tomcat

在 Maven 的 pom.xml 文件中，需要添加 `spring-boot-starter-web` 依赖，该依赖默认包含了 Tomcat 作为内嵌 Web 服务器。

```xml
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
</dependencies>
```

在这个例子中，由于 `spring-boot-starter-web` 默认包含了 Tomcat 依赖，因此 Spring Boot 会自动选择 Tomcat 作为内嵌 Web 服务器并使用默认设置启动它。

### 2. 默认使用 Jetty

如果希望使用 Jetty 作为内嵌 Web 服务器，需要在 Maven 的 pom.xml 文件中添加 `spring-boot-starter-web` 依赖，但排除默认的 Tomcat 依赖，然后添加 `spring-boot-starter-jetty` 依赖。

```xml
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
        <exclusions>
            <exclusion>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-tomcat</artifactId>
            </exclusion>
        </exclusions>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-jetty</artifactId>
    </dependency>
</dependencies>
```

在这个例子中，通过添加 `spring-boot-starter-jetty` 依赖并排除默认的 Tomcat 依赖，Spring Boot 会自动选择 Jetty 作为内嵌 Web 服务器并使用默认设置启动它。

总之，Spring Boot 通过检查项目的 classpath 来自动选择合适的内嵌 Web 服务器。根据项目中的依赖关系，可以轻松地切换使用不同的内嵌 Web 服务器。



# 二、Tomcat和Jetty的区别是什么？

以下是 Tomcat 和 Jetty 的对比表格，包括它们的区别、优缺点和性能方面的特点：

| 特性     | Tomcat                                               | Jetty                                                 |
| -------- | ---------------------------------------------------- | ----------------------------------------------------- |
| 类别     | Servlet 容器                                         | Servlet 容器和 Web 服务器                             |
| 设计目标 | 提供简单、高效的 Servlet 和 JSP 容器                 | 提供可扩展、高性能的嵌入式 Web 服务器和 Servlet 容器  |
| 社区支持 | 拥有庞大的社区和丰富的文档，由 Apache 软件基金会支持 | 拥有活跃的社区和良好的文档，由 Eclipse 基金会支持     |
| 配置     | 使用 XML 配置文件，可读性较好但扩展性较低            | 使用 XML 和 Java API 进行配置，扩展性较强             |
| 性能     | 性能优秀，适用于大部分应用场景                       | 性能非常优秀，特别是在高并发、低延迟的场景下          |
| 内存占用 | 相对较高，因为默认集成了许多组件                     | 相对较低，模块化设计使得内存占用更小                  |
| 启动时间 | 启动时间相对较长，因为需要加载许多默认组件           | 启动时间较短，模块化设计使得启动更快                  |
| 扩展性   | 可通过扩展和定制满足特定需求，但扩展性相对较低       | 模块化设计，通过添加或移除模块来轻松扩展和定制功能    |
| 使用场景 | 适用于大部分 Web 应用程序，特别是中小型应用程序      | 适用于需要高性能、高并发或需要灵活扩展的 Web 应用程序 |

**在选择 Tomcat 和 Jetty 时，可以根据以下几点进行考虑：**

1. 社区支持与文档：如果希望获得更广泛的社区支持和丰富的文档，Tomcat 可能是一个更好的选择。
2. 性能需求：如果应用程序对性能和并发有较高要求，尤其是在高并发、低延迟的场景下，Jetty 可能是更合适的选择。
3. 内存占用与启动时间：如果对内存占用和启动时间有较高要求，可以考虑使用 Jetty，因为它的模块化设计可以减少内存占用并缩短启动时间。
4. 扩展性：如果需要更灵活的扩展和定制功能，Jetty 的模块
5. 化设计可能是一个更好的选择，因为它允许通过添加或移除模块来轻松扩展和定制功能。

**根据不同的应用场景和需求，以下是一些建议：**

1. 对于中小型 Web 应用程序，Tomcat 可能是一个更好的选择，因为它提供了丰富的文档和广泛的社区支持，能满足大部分应用场景的性能需求。
2. 对于需要高性能、高并发的 Web 应用程序，例如在线游戏、金融交易平台等，Jetty 可能是一个更合适的选择，因为它在高并发、低延迟的场景下表现出更优秀的性能。
3. 如果应用程序需要快速启动和低内存占用，例如在微服务架构中，Jetty 可能是一个更好的选择，因为它的模块化设计可以降低内存占用并缩短启动时间。
4. 对于需要灵活扩展和定制功能的 Web 应用程序，Jetty 可能是一个更好的选择，因为它的模块化设计允许开发者通过添加或移除模块来轻松扩展和定制功能。

总的来说，Tomcat 和 Jetty 都是优秀的 Web 服务器和 Servlet 容器，选择哪一个取决于项目的具体需求和场景。在实际项目中，建议评估两者的性能、扩展性、内存占用等因素，并根据应用程序的特点进行权衡。在某些情况下，也可以考虑使用其他 Web 服务器，如 Undertow，以满足特定需求。



# 三、Spring Boot的自动配置

Spring Boot 的自动配置是一种便捷的功能，它根据应用程序的依赖和配置自动为开发者提供合理的默认设置。这样可以减少手动配置的工作量，让开发者更快地搭建并运行应用程序。自动配置的核心原理是约定优于配置（Convention over Configuration），即在没有指定配置的情况下，Spring Boot 会根据一组预定义的规则自动选择合适的默认配置。

**自动配置的实现主要基于以下几个方面：**

1. 条件注解：Spring Boot 使用一些特殊的注解，如 `@ConditionalOnClass`, `@ConditionalOnMissingClass`, `@ConditionalOnProperty` 等，来根据特定条件启用或禁用配置。这些注解可以检查类路径中是否存在某个类、某个属性是否具有特定的值等。
2. 配置类：自动配置是通过一系列预定义的配置类实现的，这些配置类包含了组件（如 beans）的定义以及与之相关的配置。这些配置类通常使用 `@Configuration` 注解标记，并在类路径下的 `META-INF/spring.factories` 文件中进行注册。
3. 启动器（Starter）：启动器是一组用于简化依赖管理的便捷模块。它们包含了一组预定义的依赖，这些依赖是针对特定功能（如 Web 开发、数据库访问等）的。启动器使得开发者可以通过添加单个依赖来获取所需的功能及其自动配置。

## 示例说明：

假设我们要使用 Spring Boot 开发一个简单的 Web 应用程序。首先，我们需要在 Maven 的 pom.xml 文件中添加 `spring-boot-starter-web` 启动器：

```xml
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
</dependencies>
```

在添加了 `spring-boot-starter-web` 启动器后，Spring Boot 会自动执行以下配置：

1. 根据类路径中的 Servlet API，自动配置嵌入式 Web 服务器（如 Tomcat）。
2. 自动配置 Spring MVC，包括设置 DispatcherServlet、配置视图解析器、注册静态资源处理器等。
3. 自动配置 Jackson，以支持 JSON 数据的序列化和反序列化。

这样，开发者无需手动配置这些组件，就可以开始编写 Web 应用程序的业务代码。当然，如果需要自定义配置，可以在应用程序的配置文件（如 `application.properties` 或 `application.yml`）中提供相应的设置。这些设置会覆盖自动配置提供的默认值。

###优点：

自动配置的优点是显而易见的：

1. 简化配置：自动配置减少了手动配置的工作量，使得开发者可以更快地搭建
2. 并运行应用程序。对于许多常见的场景，自动配置提供的默认设置已经足够满足需求，无需额外的配置。
3. 易于使用：自动配置使得开发者无需深入了解底层技术细节，即可快速上手 Spring Boot。通过简单地添加启动器依赖，开发者就可以获得一整套功能及其相关的自动配置。
4. 高度可定制：虽然自动配置提供了很多默认设置，但它仍然允许开发者在需要时进行自定义配置。这样，开发者可以根据项目的实际需求进行灵活地调整配置。

###局限性：

1. 隐藏了底层实现：自动配置隐藏了许多底层实现的细节，可能导致开发者在遇到问题时难以诊断。为了解决这个问题，开发者需要对 Spring Boot 的自动配置机制有一定的了解，以便在需要时进行调整和排查。
2. 默认配置不一定适用于所有场景：虽然自动配置为开发者提供了合理的默认设置，但这些设置可能不适用于所有场景。在这种情况下，开发者需要深入了解相关技术，并进行适当的自定义配置。

**总之，Spring Boot 的自动配置为开发者提供了很多便利，使得在许多常见场景下可以快速搭建并运行应用程序。然而，开发者仍然需要了解自动配置的原理及其局限性，以便在需要时进行调整和故障排查。**

# 四、Spring Boot Web项目默认的web服务器为何是Tomcat？如何确定的？

要找到 Spring Boot 中默认的 Web 服务器，可以按照以下步骤查看 `spring-boot-starter-web` 依赖及其相关依赖：

1. 首先，打开 Maven 项目的 `pom.xml` 文件，确保已经添加了 `spring-boot-starter-web` 依赖。

```xml
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
</dependencies>
```

2. 在项目的根目录下运行以下 Maven 命令，查看项目的依赖树：

```shell
mvn dependency:tree
```

此命令会输出项目的依赖树，包括所有直接和传递依赖。

3. 在输出的依赖树中，找到 `spring-boot-starter-web` 依赖，以及与之相关的 `spring-boot-starter-tomcat` 依赖。例如：

```csharp
[INFO] +- org.springframework.boot:spring-boot-starter-web:jar:2.5.5:compile
[INFO] |  +- org.springframework.boot:spring-boot-starter:jar:2.5.5:compile
[INFO] |  |  ...
[INFO] |  +- org.springframework.boot:spring-boot-starter-json:jar:2.5.5:compile
[INFO] |  |  ...
[INFO] |  +- org.springframework.boot:spring-boot-starter-tomcat:jar:2.5.5:compile
[INFO] |  |  +- org.apache.tomcat.embed:tomcat-embed-core:jar:9.0.53:compile
[INFO] |  |  +- org.apache.tomcat.embed:tomcat-embed-el:jar:9.0.53:compile
[INFO] |  |  \- org.apache.tomcat.embed:tomcat-embed-websocket:jar:9.0.53:compile
[INFO] |  ...
```

从上述输出中可以看出，`spring-boot-starter-web` 依赖包含了 `spring-boot-starter-tomcat` 依赖，而后者包含了 `org.apache.tomcat.embed:tomcat-embed-core` 等依赖。这意味着默认的 Web 服务器是 Tomcat。

## 详细说明：

- 当你将 `spring-boot-starter-web` 添加到项目中时，它会自动添加许多与 Web 开发相关的依赖，包括默认的 Web 服务器。
- 在这种情况下，`spring-boot-starter-web` 默认添加了 `spring-boot-starter-tomcat` 作为嵌入式 Web 服务器。
- `spring-boot-starter-tomcat` 包含了 Tomcat 的核心组件，如 `tomcat-embed-core`、`tomcat-embed-el` 和 `tomcat-embed-websocket`，它们允许将 Tomcat 作为嵌入式 Web 服务器运行在 Spring Boot 应用程序中。

**通过这些步骤，我们可以确认在添加 `spring-boot-starter-web` 依赖时，默认的嵌入式 Web 服务器是 Tomcat。**



# 五、起步依赖（Starters）组件优缺点

起步依赖（Starters）是 Spring Boot 提供的一组便捷模块，用于简化项目依赖管理。它们为常见的开发任务（如 Web 开发、数据访问等）提供了预定义的依赖组合，使得开发者可以通过添加单个依赖来获取所需的功能及其自动配置。

## 优点：

1. 简化依赖管理：起步依赖减少了开发者手动添加和管理多个相关依赖的工作量，提高了开发效率。
2. 保证版本兼容性：起步依赖确保了其包含的依赖之间的版本兼容性，避免了版本冲突带来的问题。
3. 易于使用：对于初学者或不熟悉某个技术的开发者，起步依赖提供了一种简单易用的方法来引入和使用相关技术。
4. 提供自动配置：起步依赖通常包含 Spring Boot 的自动配置功能，使得开发者无需进行繁琐的手动配置就可以使用相应的技术。
5. 提高可维护性：通过使用起步依赖，项目的依赖管理变得更加统一和规范，有利于提高项目的可维护性。

## 缺点：

1. 隐藏了底层实现：起步依赖隐藏了许多底层实现的细节，可能导致开发者在遇到问题时难以诊断。为了解决这个问题，开发者需要对 Spring Boot 的自动配置机制有一定的了解，以便在需要时进行调整和排查。
2. 过度依赖自动配置：虽然自动配置提供了很多便利，但过度依赖自动配置可能导致开发者对底层技术的了解不足。这可能在需要自定义配置或解决问题时带来困扰。
3. 可能引入不必要的依赖：起步依赖可能会引入一些项目实际不需要的依赖，这会增加项目的大小和复杂性。然而，这个问题通常可以通过自定义依赖或排除不必要的依赖来解决。

**总之，起步依赖为 Spring Boot 项目带来了很多便利，简化了依赖管理和配置过程。然而，开发者需要了解起步依赖的原理及其局限性，以便在需要时进行调整和故障排查。**

# 六、将一个Spring Boot项目制作成一个Starter

要将一个 Spring Boot 项目制作成一个 Starter，你需要遵循以下步骤：

## 1. 创建Maven项目

创建一个 Maven 项目，并设置 groupId、artifactId 和 version。这里我们将创建一个名为 `my-starter` 的项目。

## 2. 添加pox依赖

1. 在项目的 `pom.xml` 文件中添加 Spring Boot 的父 POM：

```xml
<parent>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-parent</artifactId>
  <version>2.7.4</version>
</parent>
```

2. 添加项目所需的依赖，例如 `spring-boot-starter-web`。你还可以根据需要添加其他依赖。

```xml
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
</dependencies>
```

## 3. 创建Web服务（controller）

创建一个简单的 Web 服务。在 `src/main/java` 目录下创建一个名为 `MyController` 的类，并使用 `@RestController` 注解标记它：

```java
package com.example.mystarter;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class MyController {

    @GetMapping("/hello")
    public String hello() {
        return "Hello from My Starter!";
    }
}
```

## 4.创建自动配置类--对web服务进行配置

创建一个自动配置类，用于启用 `MyController`。在 `src/main/java` 目录下创建一个名为 `MyStarterAutoConfiguration` 的类，并使用 `@Configuration` 和 `@ConditionalOnWebApplication` 注解标记它：

```java
package com.example.mystarter;

import org.springframework.boot.autoconfigure.condition.ConditionalOnWebApplication;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.Import;

@Configuration  //表示是一个配置类
@ConditionalOnWebApplication  //只有在 Web 应用程序中才会启用此自动配置。
@Import(MyController.class)  //导入MyController到配置类中
public class MyStarterAutoConfiguration {
  
}
```

这里，`@Import` 注解将 `MyController` 导入到自动配置类中，而 `@ConditionalOnWebApplication` 注解确保只有在 Web 应用程序中才会启用此自动配置。

## 5. 创建META-INF/spring.factories文件

在 `src/main/resources` 目录下创建一个名为 `META-INF/spring.factories` 的文件，用于注册自动配置类。在该文件中添加以下内容：

```yaml
org.springframework.boot.autoconfigure.EnableAutoConfiguration=com.example.mystarter.MyStarterAutoConfiguration
```

至此，你已经创建了一个简单的 Spring Boot Starter，它包含了一个简单的 Web 服务。要使用这个 Starter，你可以将其打包成 JAR 文件，并将其添加到其他 Spring Boot 项目的依赖中。在这些项目中，**只要添加了 `my-starter` 依赖，`MyController` 就会自动启用，提供一个名为 `/hello` 的 Web 服务。**

# 七、Spring Boot集成了许多优秀的组件，请举例几个较多使用的组件

Spring Boot 集成了许多优秀的组件，以下是其中几个比较常用的组件：

1. **Spring Data**：Spring Data 是 Spring 生态系统中用于简化数据访问的框架。它支持多种数据存储技术，如关系型数据库、NoSQL 数据库和搜索引擎。Spring Boot 集成了 Spring Data JPA、Spring Data MongoDB 等模块，使得开发者可以轻松地使用这些技术。
2. **Spring Security**：Spring Security 是 Spring 生态系统中的安全框架，用于处理认证和授权等安全问题。**它提供了诸如登录**、注销、权限控制等功能。Spring Boot 集成了 Spring Security，使得开发者可以轻松地保护 Web 应用程序。
3. **Spring Cloud**：Spring Cloud 是 Spring 生态系统中用于构建分布式系统的框架。它提供了一组工具和组件，如服务注册与发现、服务调用、配置管理、断路器等。Spring Boot 集成了 Spring Cloud Netflix、Spring Cloud Alibaba 等模块，使得开发者可以更加轻松地构建分布式系统。
4. **Spring WebFlux**：Spring WebFlux 是 Spring 5 引入的一种新的 Web 框架，用于处理非阻塞式的 Web 请求。它支持基于 Reactor 的编程模型，可以在高并发的场景下提高系统的吞吐量。Spring Boot 集成了 Spring WebFlux，使得开发者可以使用这种新的编程模型构建响应式的 Web 应用程序。
5. **Spring Batch**：Spring Batch 是 Spring 生态系统中的批处理框架，用于处理大量数据。它提供了诸如读取、处理和写入数据等功能。Spring Boot 集成了 Spring Batch，使得开发者可以更加轻松地编写和运行批处理作业。

总之，Spring Boot 集成了许多优秀的组件，使得开发者可以更加轻松地构建各种类型的应用程序。开发者可以根据实际需求选择适当的组件，从而快速构建出高质量的应用程序。

# 八、SpringBoot 集成Spring Data Jpa的java代码示例？使用H2内存数据库

好的，以下是一个简单的 Spring Boot 集成 Spring Data JPA 的 Java 代码示例，使用 H2 内存数据库：

## 添加依赖

在项目的 `pom.xml` 文件中添加 Spring Data JPA 和 H2 数据库的依赖：

```xml
 <parent>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-parent</artifactId>
      <version>2.7.4</version>
  </parent>

 <properties>
        <maven.compiler.source>8</maven.compiler.source>
        <maven.compiler.target>8</maven.compiler.target>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
        </dependency>
        <!--Spring Data Jpa -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-jpa</artifactId>
        </dependency>
     	 <!--H2数据库，排除内部依赖的slf4j -->
        <dependency>
            <groupId>com.h2database</groupId>
            <artifactId>h2</artifactId>
            <scope>runtime</scope>
            <exclusions>
                <exclusion>
                    <groupId>org.slf4j</groupId>
                    <artifactId>slf4j-nop</artifactId>
                </exclusion>
            </exclusions>
        </dependency>
    </dependencies>
```

## 修改配置文件、配置数据源

在 `application.yml` 文件中配置 H2 数据库：

```yml
server:
  port: 8090

spring:
  datasource:
    url: jdbc:h2:mem:testdb;MODE=MySQL  # 支持mysql
    driver-class-name: org.h2.Driver
    username: sa
    password:

  # 配置方言
  jpa:
    database-platform: org.hibernate.dialect.H2Dialect
```

## 编写代码

1. 创建实体类

在 `src/main/java` 目录下创建一个名为 `MyUser` 的实体类，用于映射数据库表：

```java
package com.example.demo;

import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;

// user是H2中的关键字，不能用
@Entity(name = "my_user")
public class MyUser {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;

    private Integer age;

    // getters and setters

    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Integer getAge() {
        return age;
    }

    public void setAge(Integer age) {
        this.age = age;
    }
}
```

2. 创建 Repository

在 `src/main/java` 目录下创建一个名为 `UserRepository` 的接口，继承 `JpaRepository` 接口，并指定实体类和主键类型：

```java
package com.example.demo;

import org.springframework.data.jpa.repository.JpaRepository;

public interface UserRepository extends JpaRepository<MyUser, Long> {
}
```

3. 创建控制器

在 `src/main/java` 目录下创建一个名为 `UserController` 的控制器类，用于处理 HTTP 请求：

```java
package com.example.demo;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RestController;

import java.util.List;

@RestController
public class UserController {

    @Autowired
    private UserRepository userRepository;

    @GetMapping("/users")
    public List<MyUser> getUsers() {
        return userRepository.findAll();
    }

    @PostMapping("/users")
    public MyUser createUser(@RequestBody MyUser user) {
        return userRepository.save(user);
    }
}
```

4. 运行应用程序

在启动类中添加 `@SpringBootApplication` 注解，表示这是一个 Spring Boot 应用程序：

```java
package com.example.demo;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class DemoApplication {

    public static void main(String[] args) {
        SpringApplication.run(DemoApplication.class, args);
    }
}
```

至此，我们已经完成了一个简单的 Spring Boot 集成 Spring Data JPA 的 Java 代码示例，使用 H2 内存数据库。你可以启动应用 。



