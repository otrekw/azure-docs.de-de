---
title: Verwenden des Java-In-Process-Agents für Application Insights in Azure Spring Cloud
description: Hier erfahren Sie, wie Sie Apps und Microservices mit dem Java-In-Process-Agent für Application Insights in Azure Spring Cloud überwachen.
author: MikeDodaro
ms.author: brendm
ms.service: spring-cloud
ms.topic: how-to
ms.date: 12/04/2020
ms.custom: devx-track-java
ms.openlocfilehash: c4871c3de8028eec1b6184c1d03ac2180b50f57d
ms.sourcegitcommit: aaa65bd769eb2e234e42cfb07d7d459a2cc273ab
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/27/2021
ms.locfileid: "98881349"
---
# <a name="application-insights-java-in-process-agent-in-azure-spring-cloud-preview"></a>Java-In-Process-Agent für Application Insights in Azure Spring Cloud (Vorschau)

In diesem Dokument wird erläutert, wie Sie Apps und Microservices mit dem Java-Agent für Application Insights in Azure Spring Cloud überwachen. 

Mit diesem Feature können Sie die folgenden Aktionen ausführen:

* Suchen nach Ablaufverfolgungsdaten mit unterschiedlichen Filtern.
* Anzeigen eines Abhängigkeitsdiagramms für Microservices
* Überprüfen der Anforderungsleistung
* Überwachen von Livemetriken in Echtzeit
* Überprüfen auf Anforderungsfehler
* Überprüfen von Anwendungsmetriken

Application Insights bietet viele überwachbare Perspektiven, z. B.:

* Anwendungszuordnung
* Leistung
* Fehler
* Metriken
* Livemetriken
* Verfügbarkeit

## <a name="enable-java-in-process-agent-for-application-insights"></a>Aktivieren des Java-In-Process-Agents für Application Insights

Führen Sie die folgenden Schritte aus, um die Previewfunktion für den Java-In-Process-Agent zu aktivieren.

1. Navigieren Sie zur Dienstübersichtsseite Ihrer Dienstinstanz.
2. Klicken Sie auf dem Blatt „Überwachung“ auf den Eintrag **Application Insights**.
3. Klicken Sie auf die Schaltfläche **Application Insights aktivieren**, um die **Application Insights**-Integration zu aktivieren.
4. Wählen Sie eine vorhandene Application Insights-Instanz aus, oder erstellen Sie eine neue.
5. Klicken Sie auf **Enable Java in-process agent** (Java-In-Process-Agent aktivieren), um die Vorschau des Java-In-Process-Agent-Features zu aktivieren. Hier können Sie auch eine Stichprobenhäufigkeit zwischen 0 und 100 festlegen.
6.  Klicken Sie auf **Speichern**, um die Änderung zu speichern.

## <a name="portal"></a>Portal

1. Navigieren Sie zur Seite **Dienst | Übersicht**, und klicken Sie im Abschnitt **Überwachung** auf **Application Insights**. 
2. Klicken Sie auf **Application Insights aktivieren**, um Application Insights in Azure Spring Cloud zu aktivieren.
3. Klicken Sie auf **Enable Java in-process agent** (Java-In-Process-Agent aktivieren), um die Java-IPA-Previewfunktion zu aktivieren. Wenn eine IPA-Previewfunktion aktiviert ist, können Sie eine optionale Stichprobenhäufigkeit konfigurieren (standardmäßig 10,0 %).

  [ ![IPA 0](media/spring-cloud-application-insights/insights-process-agent-0.png)](media/spring-cloud-application-insights/insights-process-agent-0.png)

## <a name="using-the-application-insights-feature"></a>Verwenden des Application Insights-Features

Wenn das **Application Insights**-Feature aktiviert ist, können Sie die folgenden Aktionen ausführen:

Klicken Sie im linken Navigationsbereich auf **Application Insights**, um zur Seite **Übersicht** in Application Insights zu springen. 

* Klicken Sie auf **Anwendungsübersicht**, um den Status von Aufrufen zwischen Anwendungen anzuzeigen.

  [ ![IPA 2](media/spring-cloud-application-insights/insights-process-agent-2-map.png)](media/spring-cloud-application-insights/insights-process-agent-2-map.png)

* Klicken Sie auf die Verbindung zwischen customers-service und `petclinic`, um weitere Details anzuzeigen, z. B. eine SQL-Abfrage.

* Klicken Sie im linken Navigationsbereich auf **Leistung**, um die Leistungsdaten für die Vorgänge aller Anwendungen sowie Abhängigkeiten und Rollen anzuzeigen.

  [ ![IPA 4](media/spring-cloud-application-insights/insights-process-agent-4-performance.png)](media/spring-cloud-application-insights/insights-process-agent-4-performance.png)

* Klicken Sie im linken Navigationsbereich auf **Fehler**, um zu sehen, ob etwas Unerwartetes für Ihre Anwendungen angezeigt wird.

  [ ![IPA 6](media/spring-cloud-application-insights/insights-process-agent-6-failures.png)](media/spring-cloud-application-insights/insights-process-agent-6-failures.png)

* Klicken Sie im linken Navigationsbereich auf **Metriken**, und wählen Sie den gewünschten Namespace aus. Es werden ggf. sowohl Spring Boot-Metriken als auch benutzerdefinierte Metriken angezeigt.

  [ ![IPA 7](media/spring-cloud-application-insights/insights-process-agent-5-metrics.png)](media/spring-cloud-application-insights/insights-process-agent-5-metrics.png)

* Klicken Sie im linken Navigationsbereich auf **Livemetriken**, um die Echtzeitmetriken für verschiedene Dimensionen anzuzeigen.

  [ ![IPA 8](media/spring-cloud-application-insights/petclinic-microservices-live-metrics.jpg)](media/spring-cloud-application-insights/petclinic-microservices-live-metrics.jpg)

* Klicken Sie im linken Navigationsbereich auf **Verfügbarkeit**, um die Verfügbarkeit und Reaktionsfähigkeit von Web-Apps zu überwachen, indem Sie [Verfügbarkeitstests in Application Insights erstellen](../azure-monitor/app/monitor-web-app-availability.md).

  [ ![IPA 9](media/spring-cloud-application-insights/petclinic-microservices-availability.jpg)](media/spring-cloud-application-insights/petclinic-microservices-availability.jpg)

## <a name="arm-template"></a>ARM-Vorlage
Kopieren Sie den folgenden Inhalt in `azuredeploy.json`, um die Azure Resource Manager-Vorlage zu verwenden.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
        {
            "type": "Microsoft.AppPlatform/Spring",
            "name": "customize this",
            "apiVersion": "2020-07-01",
            "location": "[resourceGroup().location]",
            "resources": [
                {
                    "type": "monitoringSettings",
                    "apiVersion": "2020-11-01-preview",
                    "name": "default",
                    "properties": {
                        "appInsightsInstrumentationKey": "00000000-0000-0000-0000-000000000000",
                        "appInsightsSamplingRate": 88.0
                    },
                    "dependsOn": [
                        "[resourceId('Microsoft.AppPlatform/Spring', 'customize this')]"
                    ]
                }
            ],
            "properties": {}
        }
    ]
}
```

## <a name="cli"></a>Befehlszeilenschnittstelle (CLI)
Wenden Sie die ARM-Vorlage mit einem der folgenden CLI-Befehle an:

* Für eine vorhandene Azure Spring Cloud-Instanz:

```azurecli
az spring-cloud app-insights update [--app-insights/--app-insights-key] "assignedName" [--sampling-rate] "samplingRate" –name "assignedName" –resource-group "resourceGroupName"
```
* Für eine neu erstellte Azure Spring Cloud-Instanz:

```azurecli
az spring-cloud create/update [--app-insights]/[--app-insights-key] "assignedName" --disable-app-insights false --enable-java-agent true --name "assignedName" –resource-group "resourceGroupName"
```
* So deaktivieren Sie AppInsights:

```azurecli
az spring-cloud app-insights update --disable –name "assignedName" –resource-group "resourceGroupName"

```

## <a name="see-also"></a>Weitere Informationen
* [Verwenden der verteilten Ablaufverfolgung mit Azure Spring Cloud](spring-cloud-tutorial-distributed-tracing.md)
* [Analysieren von Protokollen und Metriken](diagnostic-services.md)
* [Streamen von Protokollen in Echtzeit](spring-cloud-howto-log-streaming.md)