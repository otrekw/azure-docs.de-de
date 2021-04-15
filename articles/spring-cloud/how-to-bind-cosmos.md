---
title: Binden von Azure Cosmos DB an Ihre Azure Spring Cloud-Anwendung
description: Erfahren Sie, wie Sie Azure Cosmos DB an Ihre Azure Spring Cloud-Anwendung binden.
author: bmitchell287
ms.service: spring-cloud
ms.topic: how-to
ms.date: 10/06/2019
ms.author: brendm
ms.custom: devx-track-java
ms.openlocfilehash: bf761bded38ffa975610c7a7f217e911704ae3bf
ms.sourcegitcommit: b28e9f4d34abcb6f5ccbf112206926d5434bd0da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/09/2021
ms.locfileid: "107227941"
---
# <a name="bind-an-azure-cosmos-db-database-to-your-azure-spring-cloud-application"></a>Binden einer Azure Cosmos DB-Datenbank an Ihre Azure Spring Cloud-Anwendung

**Dieser Artikel gilt für:** ✔️ Java

Mit Azure Spring Cloud können Sie ausgewählte Azure-Dienste automatisch an Ihre Anwendungen binden, statt Ihre Spring Boot-Anwendungen manuell zu konfigurieren. In diesem Artikel wird gezeigt, wie Sie Ihre Anwendung an eine Azure Cosmos DB-Datenbank binden.

Voraussetzungen:

* Eine bereitgestellte Azure Spring Cloud-Instanz. Informationen zu den ersten Schritten finden Sie unter [Schnellstart: Starten einer Java Spring-Anwendung mit der Azure CLI](spring-cloud-quickstart.md).
* Ein Azure Cosmos DB-Konto mit mindestens der Berechtigungsebene „Mitwirkender“

## <a name="bind-azure-cosmos-db"></a>Binden von Azure Cosmos DB

Azure Cosmos DB verfügt über fünf verschiedene API-Typen, die Bindung unterstützen. Im Folgenden sehen Sie, wie diese verwendet werden:

1. Erstellen Sie eine Azure Cosmos DB-Datenbank. Weitere Informationen finden Sie in der Schnellstartanleitung zum [Erstellen einer Datenbank](../cosmos-db/create-cosmosdb-resources-portal.md). 

1. Notieren Sie den Namen der Datenbank. In diesem Verfahren lautet der Datenbankname **testdb**.

1. Fügen Sie eine der folgenden Abhängigkeiten in der Datei „pom.xml“ der Azure Spring Cloud-Anwendung hinzu. Wählen Sie die für Ihren API-Typ passende Abhängigkeit aus.

    * API-Typ: Core (SQL)

      ```xml
      <dependency>
          <groupId>com.microsoft.azure</groupId>
          <artifactId>azure-cosmosdb-spring-boot-starter</artifactId>
          <version>2.1.6</version>
      </dependency>
      ```

    * API-Typ: MongoDB

      ```xml
      <dependency>
          <groupId>org.springframework.boot</groupId>
          <artifactId>spring-boot-starter-data-mongodb</artifactId>
      </dependency>
      ```

    * API-Typ: Cassandra

      ```xml
      <dependency>
          <groupId>org.springframework.boot</groupId>
          <artifactId>spring-boot-starter-data-cassandra</artifactId>
      </dependency>
      ```

    * API-Typ: Gremlin (Graph)

      ```xml
      <dependency>
          <groupId>com.microsoft.spring.data.gremlin</groupId>
          <artifactId>spring-data-gremlin</artifactId>
          <version>2.1.7</version>
      </dependency>
      ```

    * API-Typ: Azure Table

      ```xml
      <dependency>
          <groupId>com.microsoft.azure</groupId>
          <artifactId>azure-storage-spring-boot-starter</artifactId>
          <version>2.0.5</version>
      </dependency>
      ```

1. Aktualisieren Sie die aktuelle Bereitstellung mit `az spring-cloud app update`, oder erstellen Sie eine neue Bereitstellung mithilfe von `az spring-cloud app deployment create`. Mit diesen Befehlen wird die Anwendung entweder aktualisiert oder mit der neuen Abhängigkeit erstellt.

1. Navigieren Sie im Azure-Portal zur Seite Ihres Azure Spring Cloud-Diensts. Navigieren Sie zum **Anwendungsdashboard**, und wählen Sie die Anwendung aus, die an Azure Cosmos DB gebunden werden soll. Dabei handelt es sich um dieselbe Anwendung, die Sie im vorherigen Schritt aktualisiert oder bereitgestellt haben.

1. Wählen Sie **Dienstbindung** und dann **Dienstbindung erstellen** aus. Wählen Sie zum Ausfüllen des Formulars die folgenden Werte aus:
   * **Azure Cosmos DB** für **Bindungstyp**
   * Den API-Typ
   * Den Datenbanknamen
   * Das Azure Cosmos DB-Konto

    > [!NOTE]
    > Verwenden Sie einen Schlüsselraum für den Datenbanknamen, wenn Sie mit Cassandra arbeiten.

1. Starten Sie die Anwendung neu, indem Sie auf der Anwendungsseite **Neu starten** auswählen.

1. Um sicherzustellen, dass der Dienst ordnungsgemäß gebunden ist, wählen Sie den Bindungsnamen aus, und überprüfen Sie dessen Details. Das Feld `property` sollte in etwa wie das folgende Beispiel aussehen:

    ```
    azure.cosmosdb.uri=https://<some account>.documents.azure.com:443
    azure.cosmosdb.key=abc******
    azure.cosmosdb.database=testdb
    ```

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie erfahren, wie Sie Ihre Azure Spring Cloud-Anwendung an eine Azure Cosmos DB-Datenbank binden. Weitere Informationen zum Binden von Diensten an Ihre Anwendung finden Sie unter [Binden von Azure-Diensten an Ihre Azure Spring Cloud-Anwendung: Azure Cache for Redis](spring-cloud-howto-bind-redis.md).