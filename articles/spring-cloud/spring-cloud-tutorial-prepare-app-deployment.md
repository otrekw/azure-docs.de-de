---
title: 'Anleitung: Vorbereiten einer Java Spring-Anwendung für die Bereitstellung in Azure Spring Cloud'
description: Erfahren Sie, wie Sie eine Java Spring-Anwendung für die Bereitstellung in Azure Spring Cloud vorbereiten.
author: bmitchell287
ms.service: spring-cloud
ms.topic: how-to
ms.date: 02/03/2020
ms.author: brendm
ms.custom: devx-track-java
ms.openlocfilehash: b2ae94da3d9b2dee62bc031c4a32d17b43be00a6
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2020
ms.locfileid: "87021272"
---
# <a name="prepare-a-java-spring-application-for-deployment-in-azure-spring-cloud"></a>Vorbereiten einer Java Spring-Anwendung für die Bereitstellung in Azure Spring Cloud

In diesem Thema wird gezeigt, wie Sie eine vorhandene Java Spring-Anwendung für die Bereitstellung in Azure Spring Cloud vorbereiten. Bei ordnungsgemäßer Konfiguration bietet Azure Spring Cloud stabile Dienste zur Überwachung, Skalierung und Aktualisierung Ihrer Java Spring Cloud-Anwendung.

In anderen Beispielen wird erläutert, wie eine Anwendung in Azure Spring Cloud bereitgestellt wird, wenn die POM-Datei konfiguriert ist. 
* [Starten der App über das Azure-Portal](spring-cloud-quickstart-launch-app-portal.md)
* [Starten der App über die Azure-Befehlszeilenschnittstelle](spring-cloud-quickstart-launch-app-cli.md)

In diesem Artikel werden die erforderlichen Abhängigkeiten erläutert und wie diese zur POM-Datei hinzugefügt werden.

## <a name="java-runtime-version"></a>Java Runtime-Version

In Azure Spring Cloud können nur Spring-/Java-Anwendungen ausgeführt werden.

Azure Spring Cloud unterstützt sowohl Java 8 als auch Java 11. Die Hostingumgebung enthält die aktuelle Version von Azul Zulu OpenJDK für Azure. Weitere Informationen zu Azul Zulu OpenJDK für Azure finden Sie unter [Installieren des JDK für Azure und Azure Stack](https://docs.microsoft.com/azure/developer/java/fundamentals/java-jdk-install).

## <a name="spring-boot-and-spring-cloud-versions"></a>Spring Boot- and Spring Cloud-Versionen

Wenn Sie eine vorhandene Spring Boot-Anwendung für die Bereitstellung in Azure Spring Cloud vorbereiten möchten, nehmen Sie die Abhängigkeiten von Spring Boot und Spring Cloud in die POM-Datei der Anwendung auf, wie in den folgenden Abschnitten gezeigt wird.

Azure Spring Cloud unterstützt ausschließlich Spring Boot-Anwendungen der Version 2.1 oder Version 2.2. Die folgende Tabelle enthält die unterstützten Spring Boot- und Spring Cloud-Kombinationen:

Spring Boot-Version | Spring Cloud-Version
---|---
2.1 | Greenwich.RELEASE
2.2 | Hoxton.RELEASE
2.3 | Hoxton.SR5

### <a name="dependencies-for-spring-boot-version-21"></a>Abhängigkeiten für die Spring Boot-Version 2.1

Für Spring Boot-Version 2.1 fügen Sie die folgenden Abhängigkeiten zur POM-Datei der Anwendung hinzu.

```xml
    <!-- Spring Boot dependencies -->
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.1.12.RELEASE</version>
    </parent>

    <!-- Spring Cloud dependencies -->
    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>org.springframework.cloud</groupId>
                <artifactId>spring-cloud-dependencies</artifactId>
                <version>Greenwich.SR4</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
        </dependencies>
    </dependencyManagement>
```

### <a name="dependencies-for-spring-boot-version-22"></a>Abhängigkeiten für die Spring Boot-Version 2.2

Für Spring Boot-Version 2.2 fügen Sie die folgenden Abhängigkeiten zur POM-Datei der Anwendung hinzu.

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
                <version>Hoxton.SR1</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
        </dependencies>
    </dependencyManagement>
```
### <a name="dependencies-for-spring-boot-version-23"></a>Abhängigkeiten für die Spring Boot-Version 2.3

Für Spring Boot-Version 2.3 fügen Sie der POM-Datei der Anwendung die folgenden Abhängigkeiten hinzu.

```xml
    <!-- Spring Boot dependencies -->
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.3.0.RELEASE</version>
    </parent>

    <!-- Spring Cloud dependencies -->
    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>org.springframework.cloud</groupId>
                <artifactId>spring-cloud-dependencies</artifactId>
                <version>Hoxton.SR5</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
        </dependencies>
    </dependencyManagement>
```
## <a name="azure-spring-cloud-client-dependency"></a>Azure Spring Cloud-Clientabhängigkeit

Azure Spring Cloud hostet und verwaltet Spring Cloud-Komponenten. Die Komponenten beinhalten Spring Cloud-Dienstregistrierung und Spring Cloud-Konfigurationsserver. Schließen Sie die Azure Spring Cloud-Clientbibliothek in Ihre Abhängigkeiten ein, um die Kommunikation mit der Azure Spring Cloud-Dienstinstanz zu ermöglichen.

Die folgende Tabelle enthält die korrekten Azure Spring Cloud-Versionen für Ihre App mit Spring Boot und Spring Cloud:

Spring Boot-Version | Spring Cloud-Version | Azure Spring Cloud-Version
---|---|---
2.1 | Greenwich.RELEASE | 2.1
2.2 | Hoxton.RELEASE | 2.2
2.3 | Hoxton.SR5 | 2.3

Schließen Sie eine der folgenden Abhängigkeiten in die Datei „pom.xml“ ein. Wählen Sie die Abhängigkeit aus, deren Azure Spring Cloud-Version Ihrer eigenen Version entspricht.

### <a name="dependency-for-azure-spring-cloud-version-21"></a>Abhängigkeit für die Azure Spring Cloud-Version 2.1

Für Spring Boot-Version 2.1 fügen Sie die folgende Abhängigkeit zur POM-Datei der Anwendung hinzu.

```xml
<dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>spring-cloud-starter-azure-spring-cloud-client</artifactId>
        <version>2.1.2</version>
</dependency>
```

### <a name="dependency-for-azure-spring-cloud-version-22"></a>Abhängigkeit für die Azure Spring Cloud-Version 2.2

Für Spring Boot-Version 2.2 fügen Sie die folgende Abhängigkeit zur POM-Datei der Anwendung hinzu.

```xml
<dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>spring-cloud-starter-azure-spring-cloud-client</artifactId>
        <version>2.2.1</version>
</dependency>
```

Für Spring Boot-Version 2.3 fügen Sie der POM-Datei der Anwendung die folgende Abhängigkeit hinzu.

```xml
<dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>spring-cloud-starter-azure-spring-cloud-client</artifactId>
        <version>2.3.0</version>
</dependency>
```

## <a name="other-recommended-dependencies-to-enable-azure-spring-cloud-features"></a>Weitere empfohlene Abhängigkeiten zum Aktivieren von Azure Spring Cloud-Funktionen

Wenn Sie die integrierten Funktionen von Azure Spring Cloud (von Dienstregistrierung bis hin zu verteilter Ablaufverfolgung) aktivieren möchten, müssen Sie außerdem die folgenden Abhängigkeiten in Ihre Anwendung aufnehmen. Sie können einige dieser Abhängigkeiten löschen, wenn Sie keine entsprechenden Funktionen für die spezifischen Apps benötigen.

### <a name="service-registry"></a>Dienstregistrierung

Schließen Sie die Abhängigkeit `spring-cloud-starter-netflix-eureka-client` in die Datei „pom.xml“ ein, um den verwalteten Dienst für die Azure-Dienstregistrierung zu verwenden:

```xml
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
    </dependency>
```

Der Endpunkt des Dienstregistrierungsservers wird automatisch in Form von Umgebungsvariablen mit Ihrer App eingefügt. Anwendungen können sich selbst beim Dienstregistrierungsserver registrieren und andere abhängige Microservices ermitteln.

#### <a name="enablediscoveryclient-annotation"></a>EnableDiscoveryClient-Anmerkung

Fügen Sie dem Quellcode der Anwendung die folgende Anmerkung hinzu.
```java
@EnableDiscoveryClient
```
Informationen hierzu finden Sie beispielsweise in der piggymetrics-Anwendung aus früheren Beispielen:
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

### <a name="distributed-configuration"></a>Verteilte Konfiguration

Schließen Sie zum Aktivieren der verteilten Konfiguration die folgende Abhängigkeit vom Typ `spring-cloud-config-client` in den Abhängigkeitenabschnitt der Datei „pom.xml“ ein:

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-config-client</artifactId>
</dependency>
```

> [!WARNING]
> Geben Sie nicht `spring.cloud.config.enabled=false` in Ihrer Bootstrapkonfiguration an. Andernfalls kann die Anwendung nicht mehr mit dem Konfigurationsserver verwendet werden.

### <a name="metrics"></a>Metriken

Schließen Sie die Abhängigkeit `spring-boot-starter-actuator` in den Abhängigkeitenabschnitt der Datei „pom.xml“ ein:

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```

 Metriken werden in regelmäßigen Abständen von den JMX-Endpunkten gepullt. Die Metriken können über das Azure-Portal visualisiert werden.

 > [!WARNING]
 > Geben Sie `spring.jmx.enabled=true` in der Konfigurationseigenschaft an. Andernfalls können Metriken nicht im Azure-Portal visualisiert werden.

### <a name="distributed-tracing"></a>Verteilte Ablaufverfolgung

Schließen Sie die folgenden Abhängigkeiten vom Typ `spring-cloud-starter-sleuth` und `spring-cloud-starter-zipkin` in den Abhängigkeitenabschnitt der Datei „pom.xml“ ein:

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

 Darüber hinaus müssen Sie eine Azure Application Insights-Instanz für die Verwendung mit der Azure Spring Cloud-Dienstinstanz aktivieren. Informationen zur Verwendung von Application Insights mit Azure Spring Cloud finden Sie in der [Dokumentation zur verteilten Ablaufverfolgung](spring-cloud-tutorial-distributed-tracing.md).

## <a name="see-also"></a>Weitere Informationen
* [Analysieren von Anwendungsprotokollen und -metriken](https://docs.microsoft.com/azure/spring-cloud/diagnostic-services)
* [Einrichten Ihres Konfigurationsservers](https://docs.microsoft.com/azure/spring-cloud/spring-cloud-tutorial-config-server)
* [Verwenden der verteilten Ablaufverfolgung mit Azure Spring Cloud](https://docs.microsoft.com/azure/spring-cloud/spring-cloud-tutorial-distributed-tracing)
* [Schnellstartanleitung zu Spring](https://spring.io/quickstart)
* [Spring Boot-Dokumentation](https://spring.io/projects/spring-boot)

## <a name="next-steps"></a>Nächste Schritte

In diesem Thema haben Sie erfahren, wie Sie eine Java Spring-Anwendung für die Bereitstellung in Azure Spring Cloud konfigurieren. Informationen zum Einrichten einer Konfigurationsserverinstanz finden Sie unter [Einrichten einer Spring Cloud-Konfigurationsserverinstanz für Ihren Dienst](spring-cloud-tutorial-config-server.md).

Weitere Beispiele finden Sie auf GitHub: [Azure Spring Cloud-Beispiele](https://github.com/Azure-Samples/Azure-Spring-Cloud-Samples).
