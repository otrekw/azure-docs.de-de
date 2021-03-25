---
title: Binden von Azure Cache for Redis an Ihre Azure Spring Cloud-Anwendung
description: Hier erfahren Sie, wie Sie Azure Cache for Redis an Ihre Azure Spring Cloud-Anwendung binden.
author: bmitchell287
ms.service: spring-cloud
ms.topic: how-to
ms.date: 10/31/2019
ms.author: brendm
ms.custom: devx-track-java
ms.openlocfilehash: 414f7dc64027b286d8a963452d2b86126b3c5818
ms.sourcegitcommit: 42e4f986ccd4090581a059969b74c461b70bcac0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/23/2021
ms.locfileid: "104877612"
---
# <a name="bind-azure-cache-for-redis-to-your-azure-spring-cloud-application"></a>Binden von Azure Cache for Redis an Ihre Azure Spring Cloud-Anwendung 

**Dieser Artikel gilt für:** ✔️ Java

Mit Azure Spring Cloud können Sie ausgewählte Azure-Dienste automatisch an Ihre Anwendungen binden, statt Ihre Spring Boot-Anwendungen manuell zu konfigurieren. In diesem Artikel wird gezeigt, wie Sie Ihre Anwendung an Azure Cache for Redis binden.

## <a name="prerequisites"></a>Voraussetzungen

* Eine bereitgestellte Azure Spring Cloud-Instanz
* Eine Azure Cache for Redis-Dienstinstanz
* Die Azure Spring Cloud-Erweiterung für die Azure-Befehlszeilenschnittstelle

Wenn Sie nicht über eine bereitgestellte Azure Spring Cloud-Instanz verfügen, folgen Sie den Schritten in diesem [Schnellstart zum Bereitstellen einer Azure Spring Cloud-App](spring-cloud-quickstart.md).

## <a name="bind-azure-cache-for-redis"></a>Binden von Azure Cache for Redis

1. Fügen Sie der Datei „pom.xml“ Ihres Projekts die folgende Abhängigkeit hinzu:

    ```xml
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-redis-reactive</artifactId>
    </dependency>
    ```
1. Entfernen Sie alle Eigenschaften vom Typ `spring.redis.*` aus der Datei `application.properties`.

1. Aktualisieren Sie die aktuelle Bereitstellung mit `az spring-cloud app update`, oder erstellen Sie eine neue Bereitstellung mithilfe von `az spring-cloud app deployment create`.

1. Navigieren Sie im Azure-Portal zur Seite Ihres Azure Spring Cloud-Diensts. Navigieren Sie zum **Anwendungsdashboard**, und wählen Sie die Anwendung aus, die an Azure Cache for Redis gebunden werden soll. Dabei handelt es sich um dieselbe Anwendung, die Sie im vorherigen Schritt aktualisiert oder bereitgestellt haben.

1. Wählen Sie **Dienstbindung** und dann **Dienstbindung erstellen** aus. Füllen Sie das Formular aus. Wählen Sie unbedingt als **Bindungstyp** den Wert **Azure Cache for Redis**, Ihren Azure Cache for Redis-Server und die Option **Primärschlüssel** aus.

1. Starten Sie die App neu. Die Bindung sollte nun funktionieren.

1. Um sicherzustellen, dass die Dienstbindung richtig ist, wählen Sie den Bindungsnamen aus, und überprüfen Sie dessen Details. Das Feld `property` sollte wie folgt aussehen:
    ```
    spring.redis.host=some-redis.redis.cache.windows.net
    spring.redis.port=6380
    spring.redis.password=abc******
    spring.redis.ssl=true
    ```

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie erfahren, wie Sie Ihre Azure Spring Cloud-Anwendung an Azure Cache for Redis binden. Weitere Informationen zum Binden von Diensten an Ihre Anwendung finden Sie unter [Binden einer Azure Database for MySQL-Instanz an Ihre Azure Spring Cloud-Anwendung](spring-cloud-howto-bind-mysql.md).
