---
title: 'Tutorial: Binden von Azure Cache for Redis an Ihre Azure Spring Cloud-Anwendung'
description: In diesem Tutorial erfahren Sie, wie Sie Azure Cache for Redis an Ihre Azure Spring Cloud-Anwendung binden.
author: jpconnock
ms.service: spring-cloud
ms.topic: tutorial
ms.date: 10/31/2019
ms.author: jeconnoc
ms.openlocfilehash: 662d36f8a25f2f0a21d800b7b1a25e94b13908a7
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2019
ms.locfileid: "75461492"
---
# <a name="bind-azure-cache-for-redis-to-your-azure-spring-cloud-application"></a>Binden von Azure Cache for Redis an Ihre Azure Spring Cloud-Anwendung 

Mit Azure Spring Cloud können Sie ausgewählte Azure-Dienste automatisch an Ihre Anwendungen binden, statt Ihre Spring Boot-Anwendungen manuell zu konfigurieren. In diesem Artikel wird gezeigt, wie Sie Ihre Anwendung an Azure Cache for Redis binden.

## <a name="prerequisites"></a>Voraussetzungen

* Eine bereitgestellte Azure Spring Cloud-Instanz
* Eine Azure Cache for Redis-Dienstinstanz
* Die Azure Spring Cloud-Erweiterung für die Azure-Befehlszeilenschnittstelle

Wenn Sie nicht über eine bereitgestellte Azure Spring Cloud-Instanz verfügen, folgen Sie den Schritten in diesem [Schnellstart zum Bereitstellen einer Azure Spring Cloud-App](spring-cloud-quickstart-launch-app-portal.md).

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

In diesem Tutorial haben Sie erfahren, wie Sie Ihre Azure Spring Cloud-Anwendung an Azure Cache for Redis binden. Weitere Informationen zum Binden von Diensten an Ihre Anwendung finden Sie im Tutorial zum Binden einer Anwendung an eine Azure Database for MySQL-Instanz.

> [!div class="nextstepaction"]
> [Tutorial: Binden einer Azure Database for MySQL-Instanz an Ihre Azure Spring Cloud-Anwendung](spring-cloud-tutorial-bind-mysql.md)
