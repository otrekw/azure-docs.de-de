---
title: 'Tutorial: Verwenden der verteilten Ablaufverfolgung mit Azure Spring Cloud'
description: In diesem Tutorial erfahren Sie, wie Sie die verteilte Ablaufverfolgung von Spring Cloud über Azure Application Insights verwenden.
author: bmitchell287
ms.service: spring-cloud
ms.topic: tutorial
ms.date: 10/06/2019
ms.author: brendm
ms.openlocfilehash: 0815aa084462d1b829d64cd7c5d6fa7cebf534fc
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/24/2020
ms.locfileid: "78273212"
---
# <a name="use-distributed-tracing-with-azure-spring-cloud"></a>Verwenden der verteilten Ablaufverfolgung mit Azure Spring Cloud

Mit den Tools zur verteilten Ablaufverfolgung in Azure Spring Cloud können Sie komplexe Probleme problemlos debuggen und überwachen. Azure Spring Cloud integriert [Spring Cloud Sleuth](https://spring.io/projects/spring-cloud-sleuth) in [Azure Application Insights](https://docs.microsoft.com/azure/azure-monitor/app/app-insights-overview). Diese Integration bietet leistungsstarke Funktionen für die verteilte Ablaufverfolgung aus dem Azure-Portal.

In diesem Artikel wird Folgendes behandelt:

> [!div class="checklist"]
> * Aktivieren der verteilten Ablaufverfolgung im Azure-Portal.
> * Hinzufügen von Spring Cloud Sleuth zu Ihrer Anwendung.
> * Anzeigen von Abhängigkeitszuordnungen für Ihre Microserviceanwendungen.
> * Suchen nach Ablaufverfolgungsdaten mit unterschiedlichen Filtern.

## <a name="prerequisites"></a>Voraussetzungen

Um dieses Tutorial abzuschließen, benötigen Sie einen Azure Spring Cloud-Dienst, der bereits bereitgestellt ist und ausgeführt wird. Führen Sie den [Schnellstart zum Bereitstellen einer App über die Azure CLI](spring-cloud-quickstart-launch-app-cli.md) aus, um einen Azure Spring Cloud-Dienst bereitzustellen und auszuführen.
    
## <a name="add-dependencies"></a>Hinzufügen von Abhängigkeiten

1. Fügen Sie die folgende Zeile der Datei „application.properties“ hinzu:

   ```xml
   spring.zipkin.sender.type = web
   ```

   Nach dieser Änderung kann der Zipkin-Sender an das Internet senden.

1. Überspringen Sie diesen Schritt, wenn Sie die [Anleitung zum Vorbereiten einer Azure Spring Cloud-Anwendung](spring-cloud-tutorial-prepare-app-deployment.md) befolgt haben. Wechseln Sie andernfalls zu Ihrer lokalen Entwicklungsumgebung, und bearbeiten Sie die Datei „pom.xml“, um die folgende Spring Cloud Sleuth-Abhängigkeit einzufügen:

    ```xml
    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>org.springframework.cloud</groupId>
                <artifactId>spring-cloud-sleuth</artifactId>
                <version>${spring-cloud-sleuth.version}</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
        </dependencies>
    </dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-sleuth</artifactId>
        </dependency>
    </dependencies>
    ```

1. Erstellen Sie den Azure Spring Cloud-Dienst, und stellen Sie ihn erneut bereit, damit diese Änderungen übernommen werden.

## <a name="modify-the-sample-rate"></a>Ändern der Abtastrate

Durch Anpassen der Abtastrate können Sie die Rate für die Erfassung von Telemetriedaten ändern. Wenn Sie Daten beispielsweise nur halb so häufig abrufen möchten, öffnen Sie die Datei „application.properties“, und ändern Sie die folgende Zeile:

```xml
spring.sleuth.sampler.probability=0.5
```

Wenn Sie bereits eine Anwendung erstellt und bereitgestellt haben, können Sie die Samplingrate ändern. Fügen Sie hierzu in der Azure CLI oder im Azure-Portal die vorherige Zeile als Umgebungsvariable hinzu.

## <a name="enable-application-insights"></a>Aktivieren von Application Insights

1. Navigieren Sie im Azure-Portal zur Seite Ihres Azure Spring Cloud-Diensts.
1. Wählen Sie auf der Seite **Überwachung** die Option **Verteilte Ablaufverfolgung** aus.
1. Wählen Sie **Einstellung bearbeiten** aus, um eine neue Einstellung zu bearbeiten oder hinzuzufügen.
1. Erstellen Sie eine neue Application Insights-Abfrage, oder wählen Sie eine vorhandene aus.
1. Wählen Sie die zu überwachende Protokollkategorie aus, und geben Sie die Aufbewahrungsdauer in Tagen an.
1. Wählen Sie zum Anwenden der neuen Ablaufverfolgung die Option **Übernehmen** aus.

## <a name="view-the-application-map"></a>Anzeigen der Anwendungsübersicht

Kehren Sie zur Seite **Verteilte Ablaufverfolgung** zurück, und wählen Sie **Anwendungsübersicht anzeigen** aus. Überprüfen Sie die visuelle Darstellung Ihrer Anwendungs- und Überwachungseinstellungen. Informationen zur Verwendung der Anwendungsübersicht finden Sie unter [Anwendungsübersicht: Selektieren verteilter Anwendungen](https://docs.microsoft.com/azure/azure-monitor/app/app-map).

## <a name="use-search"></a>Verwenden der Suche

Verwenden Sie die Suchfunktion, um andere spezifische Telemetrieelemente abzufragen. Wählen Sie auf der Seite **Distributed Tracing** (Verteilte Ablaufverfolgung) die Option **Suchen** aus. Weitere Informationen zur Verwendung der Suchfunktionen finden Sie unter [Verwenden von Search in Application Insights](https://docs.microsoft.com/azure/azure-monitor/app/diagnostic-search).

## <a name="use-application-insights"></a>Verwenden von Application Insights

Application Insights bietet neben Anwendungsübersicht und Suchfunktion auch Überwachungsfunktionen. Suchen Sie im Azure-Portal nach dem Namen Ihrer Anwendung, und öffnen Sie dann eine Application Insights-Seite, um Überwachungsinformationen zu erhalten. Weitere Informationen zur Verwendung dieser Tools finden Sie unter [Protokollabfragen in Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/log-query/query-language).

## <a name="disable-application-insights"></a>Deaktivieren von Application Insights

1. Navigieren Sie im Azure-Portal zur Seite Ihres Azure Spring Cloud-Diensts.
1. Wählen Sie im Abschnitt **Überwachung** die Option **Verteilte Ablaufverfolgung** aus.
1. Wählen Sie **Deaktivieren** aus, um Application Insights zu deaktivieren

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie weitere Informationen zur verteilten Ablaufverfolgung erhalten und erfahren, wie Sie die Funktion in Azure Spring Cloud aktivieren. Fahren Sie mit dem nächsten Tutorial fort, um zu erfahren, wie Sie Ihre Anwendung an eine Azure Cosmos DB-Datenbank binden.

> [!div class="nextstepaction"]
> [Erfahren Sie, wie Sie eine Azure Cosmos DB-Datenbank an Ihre Azure Spring Cloud-Anwendung binden](spring-cloud-tutorial-bind-cosmos.md)
