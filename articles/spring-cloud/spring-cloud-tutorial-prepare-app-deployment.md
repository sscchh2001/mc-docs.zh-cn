---
title: 如何准备要部署到 Azure Spring Cloud 中的应用程序
description: 了解如何准备要部署到 Azure Spring Cloud 中的应用程序。
author: bmitchell287
ms.service: spring-cloud
ms.topic: how-to
ms.date: 01/26/2021
ms.author: v-junlch
ms.custom: devx-track-java
ms.openlocfilehash: 9e5b0b6be9d30d45d01f4170322af1bb2cf6e9b8
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99058909"
---
# <a name="prepare-an-application-for-deployment-in-azure-spring-cloud"></a>准备要部署到 Azure Spring Cloud 中的应用程序

本主题介绍如何准备现有的需要部署到 Azure Spring Cloud 的 Java Spring 应用程序。 在配置正确的情况下，Azure Spring Cloud 可以提供强大的服务来监视、缩放和更新 Java Spring Cloud 应用程序。

在运行此示例之前，可以尝试[基础知识快速入门](spring-cloud-quickstart.md)。

其他示例说明了在配置 POM 文件时，如何将应用程序部署到 Azure Spring Cloud。 
* [启动第一个应用](spring-cloud-quickstart.md)
* [生成并运行微服务](spring-cloud-quickstart-sample-app-introduction.md)

本文介绍所需的依赖项，以及如何将它们添加到 POM 文件。

## <a name="java-runtime-version"></a>Java 运行时版本

只有 Spring/Java 应用程序能够在 Azure Spring Cloud 中运行。

Azure Spring Cloud 支持 Java 8 和 Java 11。 托管环境包含用于 Azure 的最新版 Azul Zulu OpenJDK。 若要详细了解用于 Azure 的 Azul Zulu OpenJDK，请参阅[安装 JDK](https://docs.microsoft.com/azure/developer/java/fundamentals/java-jdk-install)。

## <a name="spring-boot-and-spring-cloud-versions"></a>Spring Boot 和 Spring Cloud 版本

若要准备要部署到 Azure Spring Cloud 的现有 Spring Boot 应用程序，请按以下部分中所述，在应用程序 POM 文件中包含 Spring Boot 和 Spring Cloud 依赖项。

Azure Spring Cloud 仅支持使用 Spring Boot 版本2.1 或 2.2 的 Spring Boot 应用。 下表列出了支持的 Spring Boot 和 Spring Cloud 组合：

Spring Boot 版本 | Spring Cloud 版本
---|---
2.2 | Hoxton.SR8
2.3 | Hoxton.SR8
2.4.1+ | 2020.0.0

> [!NOTE]
> 我们发现 Spring Boot 2.4.0 在应用和 Eureka 之间的 TLS 身份验证存在问题，请使用 2.4.1 版或更高版本。 如果坚持使用 2.4.0，请参阅我们的[常见问题解答](/spring-cloud/spring-cloud-faq?pivots=programming-language-java#development)以获取解决方法。

### <a name="dependencies-for-spring-boot-version-2223"></a>Spring Boot 版本 2.2/2.3 的依赖项

对于 Spring Boot 版本 2.2，请将以下依赖项添加到应用程序 POM 文件中。

```xml
    <!-- Spring Boot dependencies -->
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.2.4.RELEASE</version>
    </parent>

    <!-- Spring Cloud dependencies -->
    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>org.springframework.cloud</groupId>
                <artifactId>spring-cloud-dependencies</artifactId>
                <version>Hoxton.SR8</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
        </dependencies>
    </dependencyManagement>
```

### <a name="dependencies-for-spring-boot-version-24"></a>Spring Boot 版本 2.4 的依赖项

对于 Spring Boot 版本 2.2，请将以下依赖项添加到应用程序 POM 文件中。

```xml
    <!-- Spring Boot dependencies -->
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.4.1.RELEASE</version>
    </parent>

    <!-- Spring Cloud dependencies -->
    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>org.springframework.cloud</groupId>
                <artifactId>spring-cloud-dependencies</artifactId>
                <version>2020.0.0</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
        </dependencies>
    </dependencyManagement>
```

> [!WARNING]
> 请勿在配置中指定 `server.port`。 Azure Spring Cloud 会将此设置重写为固定端口号。 也请遵从此设置，不要在代码中指定服务器端口。

## <a name="other-recommended-dependencies-to-enable-azure-spring-cloud-features"></a>启用 Azure Spring Cloud 功能的其他推荐依赖项

若要启用服务注册表到分布式跟踪的 Azure Spring Cloud 内置功能，你还需要在应用程序中包含以下依赖项。 如果不需要特定应用的相应功能，你可以删除这些依赖项。

### <a name="service-registry"></a>服务注册表

若要使用托管的 Azure 服务注册表服务，请在 pom.xml 文件中包括 `spring-cloud-starter-netflix-eureka-client` 依赖项，如下所示：

```xml
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
    </dependency>
```

服务注册表服务器的终结点自动作为应用的环境变量注入。 然后，应用程序可自行注册到服务注册表服务器，并发现其他依赖性微服务。


#### <a name="enablediscoveryclient-annotation"></a>EnableDiscoveryClient 注释

将以下注释添加到应用程序源代码中。
```java
@EnableDiscoveryClient
```
有关示例，请参阅前面示例中的 piggymetrics 应用程序：
```java
package com.piggymetrics.gateway;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.client.discovery.EnableDiscoveryClient;
import org.springframework.cloud.netflix.zuul.EnableZuulProxy;

@SpringBootApplication
@EnableDiscoveryClient
@EnableZuulProxy

public class GatewayApplication {
    public static void main(String[] args) {
        SpringApplication.run(GatewayApplication.class, args);
    }
}
```

### <a name="distributed-configuration"></a>分布式配置

若要启用分布式配置，请在 pom.xml 文件的 dependencies 节中包括以下 `spring-cloud-config-client` 依赖项：

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-config-client</artifactId>
</dependency>
```

> [!WARNING]
> 请勿在启动配置中指定 `spring.cloud.config.enabled=false`。 否则，应用程序将再也不能与配置服务器配合使用。

### <a name="metrics"></a>指标

在 pom.xml 文件的 dependencies 节中包括 `spring-boot-starter-actuator` 依赖项，如下所示：

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```

 指标会定期从 JMX 终结点拉取。 可以通过 Azure 门户将指标可视化。

 > [!WARNING]
 > 请在配置属性中指定 `spring.jmx.enabled=true`。 否则，无法在 Azure 门户中直观显示指标。

### <a name="distributed-tracing"></a>分布式跟踪

还需让 Azure Application Insights 实例能够兼容 Azure Spring Cloud 服务实例。 若要了解如何将 Application Insights 与 Azure Spring Cloud 配合使用，请参阅[有关分布式跟踪的文档](spring-cloud-tutorial-distributed-tracing.md)。

#### <a name="spring-boot-2223"></a>Spring Boot 2.2/2.3
在 pom.xml 文件的 dependencies 节中包括下面的 `spring-cloud-starter-sleuth` 和 `spring-cloud-starter-zipkin` 依赖项：

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-sleuth</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-zipkin</artifactId>
</dependency>
```

#### <a name="spring-boot-24"></a>Spring Boot 2.4
在 pom.xml 文件的 dependencies 节中包括下面的 `spring-cloud-sleuth-zipkin` 依赖项：

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-sleuth-zipkin</artifactId>
</dependency>
```

## <a name="see-also"></a>另请参阅
* [分析应用程序日志和指标](./diagnostic-services.md)
* [设置配置服务器](./spring-cloud-tutorial-config-server.md)
* [将分布式跟踪与 Azure Spring Cloud 配合使用](./spring-cloud-tutorial-distributed-tracing.md)
* [Spring 快速入门指南](https://spring.io/quickstart)
* [Spring Boot 文档](https://spring.io/projects/spring-boot)

## <a name="next-steps"></a>后续步骤

本主题介绍了如何配置 Java Spring 应用程序，以便将其部署到 Azure Spring Cloud。 若要了解如何设置配置服务器实例，请参阅[设置配置服务器实例](spring-cloud-tutorial-config-server.md)。

GitHub 中提供了更多示例：[Azure Spring Cloud 示例](https://github.com/Azure-Samples/Azure-Spring-Cloud-Samples)。


