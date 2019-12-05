---
title: Binden einer Azure Database for MySQL-Instanz an Ihre Azure Spring Cloud-Anwendung | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie, wie Sie eine Azure Database for MySQL-Instanz an Ihre Azure Spring Cloud-Anwendung binden.
author: jpconnock
ms.service: spring-cloud
ms.topic: tutorial
ms.date: 11/04/2019
ms.author: jeconnoc
ms.openlocfilehash: 6c5cd4ac384affaedbd813f9395f997f92eb69c4
ms.sourcegitcommit: 5cfe977783f02cd045023a1645ac42b8d82223bd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/17/2019
ms.locfileid: "74151124"
---
# <a name="tutorial-bind-an-azure-database-for-mysql-instance-to-your-azure-spring-cloud-application"></a>Tutorial: Binden einer Azure Database for MySQL-Instanz an Ihre Azure Spring Cloud-Anwendung 

Mit Azure Spring Cloud können Sie ausgewählte Azure-Dienste automatisch an Ihre Anwendungen binden, statt Ihre Spring Boot-Anwendung manuell zu konfigurieren. In diesem Tutorial erfahren Sie, wie Sie Ihre Anwendung an Ihre Azure Database for MySQL-Instanz binden.

## <a name="prerequisites"></a>Voraussetzungen

* Eine bereitgestellte Azure Spring Cloud-Instanz
* Ein Azure Database for MySQL-Konto
* Azure-Befehlszeilenschnittstelle

Wenn Sie keine Azure Spring Cloud-Instanz bereitgestellt haben, befolgen Sie die Anweisungen unter [Schnellstart: Starten einer Azure Spring Cloud-Anwendung über das Azure-Portal](spring-cloud-quickstart-launch-app-portal.md), um Ihre erste Spring Cloud-App bereitzustellen.

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

In diesem Tutorial haben Sie erfahren, wie Sie Ihre Azure Spring Cloud-Anwendung an eine Azure Database for MySQL-Instanz binden können.  Weitere Informationen zum Verwalten des Azure Spring Cloud-Diensts finden Sie im Artikel zu Dienstermittlung und -registrierung.

> [!div class="nextstepaction"]
> [Aktivieren von Dienstermittlung und -registrierung über die Spring Cloud-Registrierung](spring-cloud-service-registration.md)

