---
title: 'Tutorial: Vorbereiten einer Spring-Anwendung für die Bereitstellung in Azure Spring Cloud'
description: In diesem Tutorial bereiten Sie eine Java Spring-Anwendung für die Bereitstellung vor.
author: bmitchell287
ms.service: spring-cloud
ms.topic: tutorial
ms.date: 10/06/2019
ms.author: brendm
ms.openlocfilehash: 9918c7866b21cd2a9e021a355fb43977c91a89cf
ms.sourcegitcommit: 5397b08426da7f05d8aa2e5f465b71b97a75550b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/19/2020
ms.locfileid: "76277446"
---
# <a name="prepare-a-java-spring-application-for-deployment-in-azure-spring-cloud"></a>Vorbereiten einer Java Spring-Anwendung für die Bereitstellung in Azure Spring Cloud

In dieser Schnellstartanleitung wird gezeigt, wie Sie eine vorhandene Java Spring Cloud-Anwendung für die Bereitstellung in Azure Spring Cloud vorbereiten. Bei ordnungsgemäßer Konfiguration bietet Azure Spring Cloud stabile Dienste zur Überwachung, Skalierung und Aktualisierung Ihrer Java Spring Cloud-Anwendung.

## <a name="java-runtime-version"></a>Java Runtime-Version

In Azure Spring Cloud können nur Spring-/Java-Anwendungen ausgeführt werden.

Azure Spring Cloud unterstützt sowohl Java 8 als auch Java 11. Die Hostingumgebung enthält die aktuelle Version von Azul Zulu OpenJDK für Azure. Weitere Informationen zu Azul Zulu OpenJDK für Azure finden Sie unter [Installieren des JDK für Azure und Azure Stack](https://docs.microsoft.com/azure/java/jdk/java-jdk-install).

## <a name="spring-boot-and-spring-cloud-versions"></a>Spring Boot- and Spring Cloud-Versionen

Von Azure Spring Cloud werden nur Spring Boot-Apps unterstützt. Azure Spring Cloud unterstützt sowohl die Spring Boot-Version 2.0 als auch die Version 2.1. Die folgende Tabelle enthält die unterstützten Spring Boot- und Spring Cloud-Kombinationen:

Spring Boot-Version | Spring Cloud-Version
---|---
2.0 | Finchley.RELEASE
2.1 | Greenwich.RELEASE

Vergewissern Sie sich, dass die Datei „pom.xml“ die korrekten Spring Boot- und Spring Cloud-Abhängigkeiten enthält (basierend auf Ihrer Spring Boot-Version).

### <a name="dependencies-for-spring-boot-version-20"></a>Abhängigkeiten für die Spring Boot-Version 2.0

```xml
    <!-- Spring Boot dependencies -->
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.0.9.RELEASE</version>
    </parent>

    <!-- Spring Cloud dependencies -->
    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>org.springframework.cloud</groupId>
                <artifactId>spring-cloud-dependencies</artifactId>
                <version>Finchley.SR4</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
        </dependencies>
    </dependencyManagement>
```

### <a name="dependencies-for-spring-boot-version-21"></a>Abhängigkeiten für die Spring Boot-Version 2.1

```xml
    <!-- Spring Boot dependencies -->
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.1.8.RELEASE</version>
    </parent>

    <!-- Spring Cloud dependencies -->
    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>org.springframework.cloud</groupId>
                <artifactId>spring-cloud-dependencies</artifactId>
                <version>Greenwich.SR3</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
        </dependencies>
    </dependencyManagement>
```

## <a name="azure-spring-cloud-client-dependency"></a>Azure Spring Cloud-Clientabhängigkeit

Azure Spring Cloud hostet und verwaltet Spring Cloud-Komponenten für Sie. Diese Komponenten beinhalten Spring Cloud-Dienstregistrierung und Spring Cloud-Konfigurationsserver. Schließen Sie die Azure Spring Cloud-Clientbibliothek in Ihre Abhängigkeiten ein, um die Kommunikation mit der Azure Spring Cloud-Dienstinstanz zu ermöglichen.

Die folgende Tabelle enthält die korrekten Azure Spring Cloud-Versionen für Ihre App mit Spring Boot und Spring Cloud:

Spring Boot-Version | Spring Cloud-Version | Azure Spring Cloud-Version
---|---|---
2.0 | Finchley.RELEASE | 2.0
2.1 | Greenwich.RELEASE | 2.1

Schließen Sie eine der folgenden Abhängigkeiten in die Datei „pom.xml“ ein. Wählen Sie die Abhängigkeit aus, deren Azure Spring Cloud-Version Ihrer eigenen Version entspricht.

### <a name="dependency-for-azure-spring-cloud-version-20"></a>Abhängigkeit für die Azure Spring Cloud-Version 2.0

```xml
<dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>spring-cloud-starter-azure-spring-cloud-client</artifactId>
        <version>2.0.0</version>
</dependency>
```

### <a name="dependency-for-azure-spring-cloud-version-21"></a>Abhängigkeit für die Azure Spring Cloud-Version 2.1

```xml
<dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>spring-cloud-starter-azure-spring-cloud-client</artifactId>
        <version>2.1.0</version>
</dependency>
```

## <a name="other-required-dependencies"></a>Andere erforderliche Abhängigkeiten

Zur Aktivierung der integrierten Features von Azure Spring Cloud muss Ihre Anwendung die folgenden Abhängigkeiten enthalten. Dadurch wird sichergestellt, dass Ihre Anwendung sich selbst korrekt mit den einzelnen Komponenten konfiguriert.  

### <a name="service-registry-dependency"></a>Abhängigkeit für die Dienstregistrierung

Schließen Sie die Abhängigkeit `spring-cloud-starter-netflix-eureka-client` in die Datei „pom.xml“ ein, um den verwalteten Dienst für die Azure-Dienstregistrierung zu verwenden:

```xml
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
    </dependency>
```

Der Endpunkt des Dienstregistrierungsservers wird automatisch in Form von Umgebungsvariablen mit Ihrer App eingefügt. Anwendungen können sich selbst beim Dienstregistrierungsserver registrieren und andere abhängige Microservices ermitteln.

### <a name="distributed-configuration-dependency"></a>Abhängigkeit für die verteilte Konfiguration

Schließen Sie zum Aktivieren der verteilten Konfiguration die folgende Abhängigkeit vom Typ `spring-cloud-config-client` in den Abhängigkeitenabschnitt der Datei „pom.xml“ ein:

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-config-client</artifactId>
</dependency>
```

> [!WARNING]
> Geben Sie nicht `spring.cloud.config.enabled=false` in Ihrer Bootstrapkonfiguration an. Andernfalls kann die Anwendung nicht mehr mit dem Konfigurationsserver verwendet werden.

### <a name="metrics-dependency"></a>Metrikabhängigkeit

Schließen Sie die Abhängigkeit `spring-boot-starter-actuator` in den Abhängigkeitenabschnitt der Datei „pom.xml“ ein:

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```

 Metriken werden in regelmäßigen Abständen von den JMX-Endpunkten gepullt. Die Metriken können über das Azure-Portal visualisiert werden.

### <a name="distributed-tracing-dependency"></a>Abhängigkeit für die verteilte Ablaufverfolgung

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

 Darüber hinaus müssen Sie eine Azure Application Insights-Instanz für die Verwendung mit der Azure Spring Cloud-Dienstinstanz aktivieren. Eine Anleitung für die Verwendung von Application Insights mit Azure Spring Cloud finden Sie im [Tutorial zur verteilten Ablaufverfolgung](spring-cloud-tutorial-distributed-tracing.md).

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie erfahren, wie Sie eine Java Spring-Anwendung für die Bereitstellung in Azure Spring Cloud konfigurieren. Informationen zum Einrichten einer Konfigurationsserverinstanz finden Sie im nächsten Tutorial.

> [!div class="nextstepaction"]
> [Tutorial: Einrichten einer Spring Cloud-Konfigurationsserverinstanz für Ihren Dienst](spring-cloud-tutorial-config-server.md)

Weitere Beispiele finden Sie auf GitHub: [Azure Spring Cloud-Beispiele](https://github.com/Azure-Samples/Azure-Spring-Cloud-Samples/tree/master/service-binding-cosmosdb-sql).
