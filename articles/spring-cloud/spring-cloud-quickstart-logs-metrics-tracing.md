---
title: 'Schnellstart: Überwachen von Azure Spring Cloud-Apps mit Protokollen, Metriken und Ablaufverfolgung'
description: Verwenden Sie Protokollstreaming, Protokollanalyse, Metriken und Ablaufverfolgung, um Piggy Metrics-Beispiel-Apps in Azure Spring Cloud zu überwachen.
author: MikeDodaro
ms.author: brendm
ms.service: spring-cloud
ms.topic: quickstart
ms.date: 08/04/2020
ms.custom: devx-track-java
ms.openlocfilehash: f9f03c355e1e619d004c8ec8c1cc2f91932db744
ms.sourcegitcommit: 8a7b82de18d8cba5c2cec078bc921da783a4710e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2020
ms.locfileid: "89046832"
---
# <a name="quickstart-monitoring-azure-spring-cloud-apps-with-logs-metrics-and-tracing"></a>Schnellstart: Überwachen von Azure Spring Cloud-Apps mit Protokollen, Metriken und Ablaufverfolgung

Mit der integrierten Überwachungsfunktion in Azure Spring Cloud können Sie komplexe Probleme debuggen und überwachen. Azure Spring Cloud integriert [Spring Cloud Sleuth](https://spring.io/projects/spring-cloud-sleuth) in [Azure Application Insights](https://docs.microsoft.com/azure/azure-monitor/app/app-insights-overview). Diese Integration bietet leistungsstarke Protokolle, Metriken und Funktionen für die Ablaufverfolgung aus dem Azure-Portal. In den folgenden Verfahren wird erläutert, wie Sie Protokollstreaming, die Protokollanalyse, Metriken und die verteilte Ablaufverfolgung mit bereitgestellten Piggy Metrics-Apps verwenden.

## <a name="prerequisites"></a>Voraussetzungen

Schließen Sie die vorherigen Schritte ab: 

* [Bereitstellen einer Instanz von Azure Spring Cloud](spring-cloud-quickstart-provision-service-instance.md)
* [Einrichten des Konfigurationsservers](spring-cloud-quickstart-setup-config-server.md)
* [Erstellen und Bereitstellen von Apps](spring-cloud-quickstart-deploy-apps.md)

## <a name="logs"></a>Protokolle

Es gibt zwei Möglichkeiten, um Protokolle in Azure Spring Cloud anzuzeigen: **Protokollstreaming** von Echtzeitprotokollen pro App-Instanz oder **Protokollanalyse** für aggregierte Protokolle mit erweiterter Abfragefunktion.

### <a name="log-streaming"></a>Protokollstreaming

#### <a name="cli"></a>[BEFEHLSZEILENSCHNITTSTELLE (CLI)](#tab/Azure-CLI)

Sie können mit dem folgenden Befehl Protokollstreaming in der Azure CLI verwenden:

```azurecli
az spring-cloud app logs -s <service instance name> -g <resource group name> -n gateway -f
```

Protokolle wie das folgende werden angezeigt:

[ ![Protokollstreaming über die Azure CLI](media/spring-cloud-quickstart-logs-metrics-tracing/logs-streaming-cli.png) ](media/spring-cloud-quickstart-logs-metrics-tracing/logs-streaming-cli.png#lightbox)

> [!TIP]
> Verwenden Sie `az spring-cloud app logs -h`, um weitere Parameter und Funktionen für Protokolldatenströme zu untersuchen.

#### <a name="intellij"></a>[IntelliJ](#tab/IntelliJ)

So rufen Sie die Protokolle mit dem Azure-Toolkit für IntelliJ ab:

1. Wählen Sie **Azure-Explorer** und dann **Spring Cloud** aus.

1. Klicken Sie mit der rechten Maustaste auf die ausgeführte App.

1. Wählen Sie in der Dropdownliste **Streamingprotokolle** aus.

   ![Streamingprotokolle auswählen](media/spring-cloud-intellij-howto/streaming-logs.png)
    
1. Wählen Sie **Instanz** aus.

   ![Instanz auswählen](media/spring-cloud-intellij-howto/select-instance.png)
    
1. Das Streamingprotokoll wird im Ausgabefenster angezeigt.

   ![Ausgabe des Streamingprotokolls](media/spring-cloud-intellij-howto/streaming-log-output.png)

---
### <a name="log-analytics"></a>Log Analytics

1. Navigieren Sie zur Seite **Dienst | Übersicht**, und wählen Sie im Abschnitt **Überwachung** die Option **Protokolle** aus. Klicken Sie in einer der Beispielabfragen für Azure Spring Cloud auf **Ausführen**. 

   [ ![Protokollanalyseeintrag](media/spring-cloud-quickstart-logs-metrics-tracing/logs-entry.png) ](media/spring-cloud-quickstart-logs-metrics-tracing/logs-entry.png#lightbox)
    
1. Anschließend werden gefilterte Protokolle angezeigt. Weitere Informationen zum Schreiben von Abfragen finden Sie unter [Erste Schritte mit Protokollabfragen in Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/log-query/get-started-queries).

   [ ![Protokollanalyseabfrage](media/spring-cloud-quickstart-logs-metrics-tracing/logs-query.png) ](media/spring-cloud-quickstart-logs-metrics-tracing/logs-query.png#lightbox)

## <a name="metrics"></a>Metriken

1. Navigieren Sie zur Seite **Dienst | Übersicht**, und wählen Sie im Abschnitt **Überwachung** die Option **Metriken** aus. Fügen Sie Ihre erste Metrik hinzu, indem Sie `system.cpu.usage` für **Metrik** und `Avg` für **Aggregation** auswählen, um die Zeitachse für die gesamte CPU-Auslastung anzuzeigen.

   [ ![Metrikeintrag](media/spring-cloud-quickstart-logs-metrics-tracing/metrics-basic-cpu.png) ](media/spring-cloud-quickstart-logs-metrics-tracing/metrics-basic-cpu.png#lightbox)
    
1. Klicken Sie auf der Symbolleiste oben auf **Filter hinzufügen**, und wählen Sie `App=Gateway` aus, um die CPU-Auslastung nur für die App **gateway** anzuzeigen.

   [ ![Verwenden eines Filters in Metriken](media/spring-cloud-quickstart-logs-metrics-tracing/metrics-filter.png) ](media/spring-cloud-quickstart-logs-metrics-tracing/metrics-filter.png#lightbox)

1. Schließen Sie den oben erstellten Filter, klicken Sie auf **Apply Splitting** (Aufteilung anwenden), und wählen Sie `App` für **Werte** aus, um die CPU-Auslastung auf der Grundlage verschiedener Apps anzuzeigen.

   [ ![Anwenden der Aufteilung in Metriken](media/spring-cloud-quickstart-logs-metrics-tracing/metrics-split.png) ](media/spring-cloud-quickstart-logs-metrics-tracing/metrics-split.png#lightbox)

## <a name="distributed-tracing"></a>Verteilte Ablaufverfolgung

1. Navigieren Sie zur Seite **Dienst | Übersicht**, und wählen Sie im Abschnitt **Überwachung** die Option **Verteilte Ablaufverfolgung** aus. Klicken Sie dann auf der rechten Seite auf die Registerkarte **Anwendungsübersicht anzeigen**.

   [ ![Eintrag der verteilten Ablaufverfolgung](media/spring-cloud-quickstart-logs-metrics-tracing/tracing-entry.png) ](media/spring-cloud-quickstart-logs-metrics-tracing/tracing-entry.png#lightbox)

1. Nun können Sie den Status von Aufrufen zwischen Piggy Metrics-Apps anzeigen. 

   [ ![Übersicht über verteilte Ablaufverfolgung](media/spring-cloud-quickstart-logs-metrics-tracing/tracing-overview.png) ](media/spring-cloud-quickstart-logs-metrics-tracing/tracing-overview.png#lightbox)
    
1. Klicken Sie auf den Link zwischen **gateway** und **account-service**, um weitere Details (etwa langsamste Aufrufe nach HTTP-Methoden) anzuzeigen.

   [ ![Verteilte Ablaufverfolgung](media/spring-cloud-quickstart-logs-metrics-tracing/tracing-call.png) ](media/spring-cloud-quickstart-logs-metrics-tracing/tracing-call.png#lightbox)
    
1. Klicken Sie abschließend auf **Leistung untersuchen**, um leistungsfähigere integrierte Leistungsanalysen zu untersuchen.

   [ ![Leistung der verteilten Ablaufverfolgung](media/spring-cloud-quickstart-logs-metrics-tracing/tracing-performance.png) ](media/spring-cloud-quickstart-logs-metrics-tracing/tracing-performance.png#lightbox)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

In den vorherigen Schritten haben Sie Azure-Ressourcen in einer Ressourcengruppe erstellt. Wenn Sie diese Ressourcen in Zukunft nicht mehr benötigen, löschen Sie die Ressourcengruppe im Portal, indem Sie den folgenden Befehl in Cloud Shell ausführen:

```azurecli
az group delete --name <your resource group name; for example: hellospring-1558400876966-rg> --yes
```

In den vorherigen Schritten haben Sie auch den Namen der Standardressourcengruppe festgelegt. Führen Sie zum Löschen dieser Standardeinstellung den folgenden Befehl in Cloud Shell aus:

```azurecli
az configure --defaults group=
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu integrierten Überwachungsfunktionen für Azure Spring Cloud finden Sie in den folgenden Artikeln:

> [!div class="nextstepaction"]
> [Analysieren von Protokollen und Metriken mit Diagnoseeinstellungen](diagnostic-services.md)
> [Verwenden der verteilten Ablaufverfolgung mit Azure Spring Cloud](spring-cloud-tutorial-distributed-tracing.md)
> [Streamen von Azure Spring Cloud-App-Protokollen in Echtzeit](spring-cloud-howto-log-streaming.md)
