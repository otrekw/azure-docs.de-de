---
title: Binden einer Azure Database for MySQL-Instanz an Ihre Azure Spring Cloud-Anwendung
description: Binden einer Azure Database for MySQL-Instanz an Ihre Azure Spring Cloud-Anwendung
author: bmitchell287
ms.service: spring-cloud
ms.topic: how-to
ms.date: 11/04/2019
ms.author: brendm
ms.custom: devx-track-java
ms.openlocfilehash: cc1c186f9acb6d49314f5d581a4b51ffe49d6627
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "104877337"
---
# <a name="bind-an-azure-database-for-mysql-instance-to-your-azure-spring-cloud-application"></a>Binden einer Azure Database for MySQL-Instanz an Ihre Azure Spring Cloud-Anwendung 

**Dieser Artikel gilt für:** ✔️ Java

Mit Azure Spring Cloud können Sie ausgewählte Azure-Dienste automatisch an Ihre Anwendungen binden, statt Ihre Spring Boot-Anwendung manuell zu konfigurieren. In diesem Artikel erfahren Sie, wie Sie Ihre Anwendung an Ihre Azure Database for MySQL-Instanz binden.

## <a name="prerequisites"></a>Voraussetzungen

* Eine bereitgestellte Azure Spring Cloud-Instanz
* Ein Azure Database for MySQL-Konto
* Azure CLI

Wenn Sie keine Azure Spring Cloud-Instanz bereitgestellt haben, befolgen Sie die Anweisungen unter [Schnellstart: Starten einer Azure Spring Cloud-Anwendung über das Azure-Portal](spring-cloud-quickstart.md), um Ihre erste Spring Cloud-App bereitzustellen.

## <a name="bind-your-app-to-your-azure-database-for-mysql-instance"></a>Binden der App an Ihre Azure Database for MySQL-Instanz

1. Notieren Sie sich den Benutzernamen und das Kennwort des Administrators für Ihr Azure Database for MySQL-Konto. 

1. Stellen Sie eine Verbindung mit dem Server her, erstellen Sie eine Datenbank mit dem Namen **testdb** von einem MySQL-Client, und erstellen Sie dann ein neues Konto ohne Administratorrechte.

1. Fügen Sie der Datei *pom.xml* Ihres Projekts die folgende Abhängigkeit hinzu:

    ```xml
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-jpa</artifactId>
    </dependency>
    ```
1. Entfernen Sie in der Datei *application.properties* alle `spring.datasource.*`-Eigenschaften.

1. Aktualisieren Sie die aktuelle Bereitstellung durch Ausführen von `az spring-cloud app update`, oder erstellen Sie eine neue Bereitstellung für diese Änderung, indem Sie `az spring-cloud app deployment create` ausführen.  Mit diesen Befehlen wird die Anwendung mit der neuen Abhängigkeit entweder aktualisiert oder erstellt.

1. Suchen Sie im Azure-Portal auf der Seite Ihres **Azure Spring Cloud**-Diensts nach dem **Anwendungs-Dashboard**, und wählen Sie dann die Anwendung aus, die Sie an Ihre Azure Database for MySQL-Instanz binden möchten.  Dabei handelt es sich um dieselbe Anwendung, die Sie im vorherigen Schritt aktualisiert oder bereitgestellt haben. 

1. Wählen Sie **Dienstbindung** und dann die Schaltfläche **Dienstbindung erstellen** aus. 

1. Füllen Sie das Formular aus, und wählen Sie als **Bindungstyp** die Option **Azure MySQL** aus. Verwenden Sie den gleichen Datenbanknamen wie zuvor sowie den Benutzernamen und das Kennwort, die Sie im ersten Schritt notiert haben.

1. Starten Sie die App neu. Die Bindung sollte jetzt funktionieren.

1. Um sicherzustellen, dass die Dienstbindung richtig ist, wählen Sie den Bindungsnamen aus, und überprüfen Sie die Details. Das Feld `property` sollte wie folgt aussehen:
    ```
    spring.datasource.url=jdbc:mysql://some-server.mysql.database.azure.com:3306/testdb?useSSL=true&requireSSL=false&useLegacyDatetimeCode=false&serverTimezone=UTC
    spring.datasource.username=admin@some-server
    spring.datasource.password=abc******
    spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MySQL5InnoDBDialect
    ```

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie erfahren, wie Sie eine Azure Spring Cloud-Anwendung an eine Azure Database for MySQL-Instanz binden können. Weitere Informationen zum Binden von Diensten an eine Anwendung finden Sie unter [Binden einer Azure Cosmos DB-Datenbank an Ihre Azure Spring Cloud-Anwendung](spring-cloud-howto-bind-cosmos.md).
