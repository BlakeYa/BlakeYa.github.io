# 数据存储：

## 常见的数据库

以下是Java世界中常用数据库的表格概述：

| 数据库               | 类型                          | 是否关系型数据库 | 特点                                                   | 使用场景                                           |
| -------------------- | ----------------------------- | ---------------- | ------------------------------------------------------ | -------------------------------------------------- |
| MySQL                | 关系型数据库（RDBMS）         | 是               | 开源、易于使用、高性能、成熟的社区支持                 | 网站、应用程序、CMS系统、数据仓库等                |
| PostgreSQL           | 关系型数据库（RDBMS）         | 是               | 开源、可扩展、强大的查询能力、支持多种数据类型         | 网站、企业级应用程序、GIS数据存储、数据仓库等      |
| Oracle               | 关系型数据库（RDBMS）         | 是               | 企业级、高性能、安全、可扩展                           | 企业级应用程序、数据仓库、高可用性和事务性应用等   |
| Microsoft SQL Server | 关系型数据库（RDBMS）         | 是               | 高性能、易于管理、多种版本、企业级特性                 | 企业级应用程序、数据仓库、BI解决方案等             |
| Redis                | 键值存储（Key-Value）         | 否               | 高性能、内存存储、支持多种数据结构、可持久化           | 缓存、会话管理、消息队列、实时分析等               |
| Apache Cassandra     | 列式存储（Columnar）          | 否               | 分布式、高可用性、高性能、可扩展                       | 大规模数据存储、实时分析、IoT数据、时序数据等      |
| MongoDB              | 文档型数据库（Document）      | 否               | 面向文档、高性能、可扩展、灵活的数据模型               | Web应用程序、大数据处理、实时分析、CMS系统等       |
| Neo4j                | 图形数据库（Graph）           | 否               | 高性能、易于查询、适合图形数据处理、可视化查询语言支持 | 社交网络分析、推荐系统、路径规划、知识图谱等       |
| InfluxDB             | 时间序列数据库（Time Series） | 否               | 高性能、易于查询、适合时间序列数据处理、数据压缩       | 物联网数据存储、监控系统、实时分析、金融数据分析等 |

这个表格总结了Java世界中常用数据库的类型、是否为关系型数据库、特点和使用场景。这些数据库根据应用需求和特性可以被选用来解决不同的业务场景和问题。

## MongoDB

### 简单使用MongoDB

以下是 MongoDB 的简单使用方式：

1. 安装 MongoDB

​	首先，您需要安装 MongoDB 数据库。MongoDB 可以在多个操作系统上运行，您可以在官方网站上找到相应的安装包和说明。

2. 启动 MongoDB

安装完 MongoDB 后，您需要启动 MongoDB 服务器。在大多数情况下，您只需在命令行中输入 "mongod" 命令即可启动 MongoDB 服务器。如果您需要自定义配置，请参阅官方文档以获取更多信息。

3. 连接到 MongoDB

启动 MongoDB 服务器后，您可以使用 MongoDB 客户端（例如 mongo shell、Compass 等）或 MongoDB 驱动程序在您的应用程序中连接到数据库。

**以下是使用 Java 驱动程序连接到 MongoDB 的简单示例：**

```java
import com.mongodb.client.MongoClients;
import com.mongodb.client.MongoClient;
import com.mongodb.client.MongoDatabase;
//可以直接使用此代码
public class MongoDBExample {
    public static void main(String[] args) {
        // 连接到 MongoDB 服务器
        MongoClient mongoClient = MongoClients.create("mongodb://localhost:27017");

        // 选择数据库和集合
        MongoDatabase database = mongoClient.getDatabase("test");
        MongoCollection<Document> collection = database.getCollection("myCollection");

        // 插入一条文档
        Document document = new Document("name", "John Doe")
                .append("age", 30)
                .append("address", new Document("street", "123 Main St")
                        .append("city", "Anytown")
                        .append("state", "CA")
                        .append("zip", "12345"));
        collection.insertOne(document);

        // 查询文档
        Document query = new Document("name", "John Doe");
        FindIterable<Document> results = collection.find(query);
        for (Document doc : results) {
            System.out.println(doc.toJson());
        }

        // 关闭连接
        mongoClient.close();
    }
}

```

从数据库中的数据存储为： 

```json
{
    "_id" : ObjectId("643a50da849bc574dbb49221"),
    "name" : "John Doe",
    "age" : NumberInt(30),
    "address" : {
        "street" : "123 Main St",
        "city" : "Anytown",
        "state" : "CA",
        "zip" : "12345"
    }
}
```

此示例将连接到本地 MongoDB 服务器，选择名为 "test" 的数据库（如果不存在会自动创建）和 "myCollection" 集合（如果不存在会自动创建）。然后，它插入一条文档并查询所有 "name" 字段为 "John Doe" 的文档。最后，它关闭了与 MongoDB 的连接。

**上述代码如果数据库不存在，会自动创建吗？**

是的，如果数据库不存在，MongoDB 会自动创建它。在上述示例中，如果 "test" 数据库不存在，MongoDB 将自动创建它，并在其中创建名为 "myCollection" 的新集合。在插入第一条文档时，MongoDB 还将自动为 "myCollection" 创建索引。

这是因为 MongoDB 是一种**无模式（Schemaless）**的数据库，这意味着它允许您在没有显式定义架构的情况下存储数据。当您插入第一条文档时，MongoDB 将根据文档中包含的字段动态地创建一个架构，并将其保存到数据库中。

### Spring Boot 集成MongoDB 操作

1. 添加依赖

在 `pom.xml` 文件中添加以下依赖：

```xml

<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-mongodb</artifactId>
</dependency>
```

这将为您的应用程序添加 Spring Boot Data MongoDB Starter 依赖项。
2. 配置数据源

在 `application.properties` 文件中添加 MongoDB 数据源的连接信息，例如：

```bash
spring.data.mongodb.uri=mongodb://localhost:27017/test
```

这将配置 Spring Data MongoDB 连接到本地 MongoDB 实例上的 "test" 数据库。
3. 创建实体类

创建一个普通的 Java 类，将其注释为实体类，并在类中定义要映射到 MongoDB 集合的属性。例如，以下是一个简单的 "Person" 实体类：

```java

import org.springframework.data.annotation.Id;
import org.springframework.data.mongodb.core.mapping.Document;

@Document(collection = "people")
public class Person {

    @Id
    private String id;
    private String name;
    private int age;

    public Person() {}

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    // getters and setters
}
```

在此示例中，我们使用 `@Document` 注释指定了要映射到 "people" 集合的实体类。还定义了 "id"、"name" 和 "age" 属性，并添加了一个带参数的构造函数。
4. 创建仓库接口

创建一个接口，并继承 `MongoRepository` 接口，以使用 Spring Boot Data JPA 自动创建 MongoDB 仓库。例如，以下是一个简单的 "PersonRepository" 接口：

```java

import org.springframework.data.mongodb.repository.MongoRepository;

public interface PersonRepository extends MongoRepository<Person, String> {

}
```

在此示例中，我们使用 `MongoRepository` 接口自动创建了一个 "PersonRepository" 仓库，用于管理 "Person" 实体类与 MongoDB 中的 "people" 集合之间的映射关系。该接口继承 `MongoRepository` 并指定了实体类和 ID 的类型。
5. 使用仓库操作数据

现在，您可以在您的代码中注入 `PersonRepository`，并使用其内置的方法来保存、查询和删除数据。例如，以下是一个简单的 Spring Boot 控制器类，用于保存和查询 "Person" 实体：

```java
package org.example;

import org.example.springBootMonogo.Person;
import org.example.springBootMonogo.PersonRepository;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

import javax.annotation.PostConstruct;

@SpringBootApplication
public class MyApplication {

    public static void main(String[] args) {
        SpringApplication.run(MyApplication.class, args);
    }

    @Autowired
    PersonRepository personRepository;

    @PostConstruct
    public void setDataToMongoDB(){
        personRepository.save(new Person("blake", 22));
    }

    @PostConstruct
    public void getDataFromMongoDB(){
        System.out.println(personRepository.findAll().toString());
    }

}

```

在此示例中，我们注入了 `PersonRepository`，并在 `savePerson` 方法中使用 `personRepository.save()` 方法保存 "Person" 实体。在 `getPerson` 方法中，我们使用 `personRepository.findById()` 方法根据。 

### MongoDB的特性

MongoDB的主要特点包括：

1. 面向文档的数据存储：MongoDB使用BSON格式存储数据，它是一种二进制的JSON表示形式，可以轻松表示复杂的数据结构。
2. 灵活的数据模型：不需要预先定义表结构，可以随着应用程序需求的变化而轻松地更改数据结构。
3. 高性能：MongoDB为读写操作提供了高性能，具有内置的缓存和索引支持。
4. 水平可扩展性：MongoDB可以通过分片（sharding）实现水平扩展，以满足大规模数据集和高吞吐量的需求。
5. 高可用性：MongoDB支持复制集（Replica Sets），以确保数据的高可用性和故障转移能力。

MongoDB广泛应用于各种场景，包括Web应用程序、大数据处理、实时分析、内容管理系统等。它是许多公司和开发者的首选NoSQL数据库之一。

### MongoDB对事物的支持（支持）

MongoDB 从版本 **4.0** 开始支持多文档事务（Multi-Document Transactions）功能。在 MongoDB 中，事务是基于文档级别的，可以跨越多个集合和文档进行操作，可以保证 ACID 特性。具体地，事务允许多个操作作为一个单元原子地执行，要么全部提交成功，要么全部回滚。

**在使用单机 MongoDB 的事务时，您需要将数据库配置为副本集，启用副本集的写关注选项（write concern）**，并使用 MongoClient 或 MongoDB 驱动程序的事务 API 进行操作。例如，以下代码演示了如何在 Java 中使用 MongoDB 的事务 API：

在使用单机的 MongoDB 的事务时，需要将数据库配置为副本集（Replica Set），启用副本集的写关注选项（Write Concern），并使用 MongoClient 或 MongoDB 驱动程序的事务 API 进行操作。

### MongoDB是否支持分布式事务（不支持）

MongoDB 在 4.2 版本中引入了分布式事务的概念，可以支持跨多个数据库的 ACID 事务。但是，**MongoDB 目前还不支持分步事务，也就是不支持在同一个事务中执行部分写操作并提交，然后再执行其他写操作并提交**。这是因为 MongoDB 事务的粒度是整个事务，而不是单个操作。

在 MongoDB 中，事务是以数据库为粒度进行管理的。每个数据库都有一个事务状态，可以使用 startTransaction() 方法开始一个事务，使用 commitTransaction() 方法提交事务，使用 abortTransaction() 方法回滚事务。在同一个事务中，可以对多个集合进行读写操作，而这些操作要么全部提交，要么全部回滚。因此，MongoDB 的事务模型比较简单和直观，但是对于某些复杂的场景可能不太灵活。

总之，MongoDB 目前还不支持分步事务，但是可以支持跨多个数据库的 ACID 事务。如果需要在同一个事务中执行部分写操作并提交，可以考虑使用其他数据库或技术，例如 MySQL、Oracle、PostgreSQL 等。

