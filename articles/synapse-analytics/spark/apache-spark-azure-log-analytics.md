---
title: Erfassen und Visualisieren von Metriken und Protokollen mithilfe von Azure Log Analytics (Vorschau)
description: Hier erfahren Sie, wie Sie den in Synapse integrierten Azure Log Analytics-Connector aktivieren, um die Apache Spark Anwendungsmetriken und -protokolle zu erfassen und an Ihren Azure Log Analytics-Arbeitsbereich zu senden.
services: synapse-analytics
author: jejiang
ms.author: jejiang
ms.reviewer: jrasnick
ms.service: synapse-analytics
ms.topic: tutorial
ms.subservice: spark
ms.date: 03/25/2021
ms.custom: references_regions
ms.openlocfilehash: 243618192593d93bba9d5229e7becfb2af62ce32
ms.sourcegitcommit: 5f482220a6d994c33c7920f4e4d67d2a450f7f08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/08/2021
ms.locfileid: "107107728"
---
# <a name="tutorial-use-azure-log-analytics-to-collect-and-visualize-metrics-and-logs-preview"></a>Tutorial: Erfassen und Visualisieren von Metriken und Protokollen mithilfe von Azure Log Analytics (Vorschau)

In diesem Tutorial erfahren Sie, wie Sie den in Synapse integrierten Azure Log Analytics-Connector aktivieren, um die Apache Spark Anwendungsmetriken und -protokolle zu erfassen und an Ihren [Azure Log Analytics-Arbeitsbereich](/azure/azure-monitor/logs/quick-create-workspace) zu senden. Anschließend können Sie eine Azure Monitor-Arbeitsmappe verwenden, um die Metriken und Protokolle zu visualisieren.

## <a name="configure-azure-log-analytics-workspace-information-in-synapse-studio"></a>Konfigurieren von Informationen für den Azure Log Analytics-Arbeitsbereich in Synapse Studio

### <a name="step-1-create-an-azure-log-analytics-workspace"></a>Schritt 1: Erstellen eines Azure Log Analytics-Arbeitsbereichs

Eine Anleitung zum Erstellen eines Log Analytics-Arbeitsbereichs finden Sie in den folgenden Dokumenten:
- [Erstellen eines Log Analytics-Arbeitsbereichs im Azure-Portal](https://docs.microsoft.com/azure/azure-monitor/logs/quick-create-workspace)
- [Erstellen eines Log Analytics-Arbeitsbereichs mit Azure CLI 2.0](https://docs.microsoft.com/azure/azure-monitor/logs/quick-create-workspace-cli)
- [Erstellen und Konfigurieren eines Log Analytics-Arbeitsbereichs in Azure Monitor mithilfe von PowerShell](https://docs.microsoft.com/azure/azure-monitor/logs/powershell-workspace-configuration)

### <a name="step-2-prepare-a-spark-configuration-file"></a>Schritt 2: Vorbereiten einer Spark-Konfigurationsdatei

#### <a name="option-1-configure-with-azure-log-analytics-workspace-id-and-key"></a>Option 1. Konfigurieren mit der ID und dem Schlüssel des Azure Log Analytics-Arbeitsbereichs 

Kopieren Sie die folgende Spark-Konfiguration, speichern Sie sie als **spark_loganalytics_conf.txt**, und füllen Sie die Parameter aus:

   - `<LOG_ANALYTICS_WORKSPACE_ID>`: Die ID des Azure Log Analytics-Arbeitsbereichs.
   - `<LOG_ANALYTICS_WORKSPACE_KEY>`: Azure Log Analytics-Schlüssel: **Azure-Portal > Azure Log Analytics-Arbeitsbereich > Agent-Verwaltung > Primärschlüssel**

```properties
spark.synapse.logAnalytics.enabled true
spark.synapse.logAnalytics.workspaceId <LOG_ANALYTICS_WORKSPACE_ID>
spark.synapse.logAnalytics.secret <LOG_ANALYTICS_WORKSPACE_KEY>
```

#### <a name="option-2-configure-with-an-azure-key-vault"></a>Option 2. Konfigurieren mit einer Azure Key Vault-Instanz

> [!NOTE]
>
> Benutzern, die Spark-Anwendungen übermitteln, muss die Berechtigung zum Lesen von Geheimnissen erteilt werden. Weitere Informationen finden Sie unter [Gewähren des Zugriffs auf Key Vault-Schlüssel, -Zertifikate und -Geheimnisse mit der rollenbasierten Zugriffssteuerung in Azure](https://docs.microsoft.com/azure/key-vault/general/rbac-guide).

Führen Sie die folgenden Schritte aus, um eine Azure Key Vault-Instanz zum Speichern des Arbeitsbereichsschlüssels zu konfigurieren:

1. Erstellen Sie im Azure-Portal Ihren Schlüsseltresor, und navigieren Sie dorthin.
2. Wählen Sie auf den Seiten mit den Key Vault-Einstellungen **Geheimnisse** aus.
3. Klicken Sie auf **Generieren/Importieren**.
4. Wählen Sie auf dem Bildschirm **Geheimnis erstellen** folgende Werte aus:
   - **Name**: Geben Sie einen Namen für das Geheimnis ein. (Verwenden Sie `"SparkLogAnalyticsSecret"` als Standardwert.)
   - **Wert**: Geben Sie den Schlüssel des Log Analytics-Arbeitsbereichs ( **<LOG_ANALYTICS_WORKSPACE_KEY>** ) für das Geheimnis ein.
   - Behalten Sie bei den anderen Optionen die Standardwerte bei. Klicken Sie auf **Erstellen**.
5. Kopieren Sie die folgende Spark-Konfiguration, speichern Sie sie als **spark_loganalytics_conf.txt**, und füllen Sie die Parameter aus:

   - `<LOG_ANALYTICS_WORKSPACE_ID>`: Die ID des Azure Log Analytics-Arbeitsbereichs.
   - `<AZURE_KEY_VAULT_NAME>`: Der von Ihnen konfigurierte Azure Key Vault-Name.
   - `<AZURE_KEY_VAULT_SECRET_KEY_NAME>` (Optional): Der Geheimnisname in der Azure Key Vault-Instanz für den Arbeitsbereichsschlüssel (Standardwert: SparkLogAnalyticsSecret).

```properties
spark.synapse.logAnalytics.enabled true
spark.synapse.logAnalytics.workspaceId <LOG_ANALYTICS_WORKSPACE_ID>
spark.synapse.logAnalytics.keyVault.name <AZURE_KEY_VAULT_NAME>
spark.synapse.logAnalytics.keyVault.key.secret <AZURE_KEY_VAULT_SECRET_KEY_NAME>
```

> [!NOTE]
>
> Sie können auch die ID des Log Analytics-Arbeitsbereichs in Azure Key Vault speichern. Gehen Sie wie oben beschrieben vor, und speichern Sie die Arbeitsbereichs-ID mit dem Geheimnisnamen `"SparkLogAnalyticsWorkspaceId"`. Alternativ können Sie auch die Konfiguration `spark.synapse.logAnalytics.keyVault.key.workspaceId` verwenden, um den Geheimnisnamen der Arbeitsbereichs-ID in Azure Key Vault anzugeben.

#### <a name="option-3-configure-with-an-azure-key-vault-linked-service"></a>Option 3. Konfigurieren mit einem mit Azure Key Vault verknüpften Dienst

> [!NOTE]
>
> Dem Synapse-Arbeitsbereich muss die Berechtigung zum Lesen von Geheimnissen erteilt werden. Weitere Informationen finden Sie unter [Gewähren des Zugriffs auf Key Vault-Schlüssel, -Zertifikate und -Geheimnisse mit der rollenbasierten Zugriffssteuerung in Azure](https://docs.microsoft.com/azure/key-vault/general/rbac-guide).

Führen Sie die folgenden Schritte aus, um in Synapse Studio einen mit Azure Key Vault verknüpften Dienst zum Speichern des Arbeitsbereichsschlüssels zu konfigurieren:

1. Führen Sie alle Schritte im Abschnitt `Option 2. Configure with an Azure Key Vault` aus.
2. Erstellen Sie in Synapse Studio einen mit Azure Key Vault verknüpften Dienst:

    a. Klicken Sie unter **Synapse Studio > Verwalten > Verknüpfte Dienste** auf die Schaltfläche **Neu**.

    b. Suchen Sie mithilfe des Suchfelds nach **Azure Key Vault**.

    c. Geben Sie einen Namen für den verknüpften Dienst ein.

    d. Wählen Sie Ihren Azure-Schlüsseltresor aus. Klicken Sie auf **Erstellen**.

3. Fügen Sie der Spark-Konfiguration ein Element vom Typ `spark.synapse.logAnalytics.keyVault.linkedServiceName` hinzu.

```properties
spark.synapse.logAnalytics.enabled true
spark.synapse.logAnalytics.workspaceId <LOG_ANALYTICS_WORKSPACE_ID>
spark.synapse.logAnalytics.keyVault.name <AZURE_KEY_VAULT_NAME>
spark.synapse.logAnalytics.keyVault.key.secret <AZURE_KEY_VAULT_SECRET_KEY_NAME>
spark.synapse.logAnalytics.keyVault.linkedServiceName <LINKED_SERVICE_NAME>
```

#### <a name="available-spark-configuration"></a>Verfügbare Spark-Konfiguration

| Konfigurationsname                                  | Standardwert                | Beschreibung                                                                                                                                                                                                |
| --------------------------------------------------- | ---------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| spark.synapse.logAnalytics.enabled                  | false                        | „true“, um die Azure Log Analytics-Senke für die Spark-Anwendungen zu aktivieren. Andernfalls „false“.                                                                                                                  |
| spark.synapse.logAnalytics.workspaceId              | -                            | Die ID des Azure Log Analytics-Zielarbeitsbereichs.                                                                                                                                                          |
| spark.synapse.logAnalytics.secret                   | -                            | Das Geheimnis des Azure Log Analytics-Zielarbeitsbereichs.                                                                                                                                                      |
| spark.synapse.logAnalytics.keyVault.linkedServiceName   | -                            | Der Name des mit Azure Key Vault verknüpften Diensts für die ID und den Schlüssel des Azure Log Analytics-Arbeitsbereichs.                                                                                                                       |
| spark.synapse.logAnalytics.keyVault.name            | -                            | Der Name der Azure Key Vault-Instanz für die ID und den Schlüssel von Azure Log Analytics.                                                                                                                                                |
| spark.synapse.logAnalytics.keyVault.key.workspaceId | SparkLogAnalyticsWorkspaceId | Der Name des Azure Key Vault-Geheimnisses für die ID des Azure Log Analytics-Arbeitsbereichs.                                                                                                                                       |
| spark.synapse.logAnalytics.keyVault.key.secret      | SparkLogAnalyticsSecret      | Der Name des Azure Key Vault-Geheimnisses für den Schlüssel des Azure Log Analytics-Arbeitsbereichs.                                                                                                                                      |
| spark.synapse.logAnalytics.keyVault.uriSuffix       | ods.opinsights.azure.com     | Das [URI-Suffix][uri_suffix] des Azure Log Analytics-Zielarbeitsbereichs. Befindet sich Ihr Azure Log Analytics-Arbeitsbereich nicht in der globalen Azure-Region, muss das URI-Suffix an die entsprechende Cloud angepasst werden. |

> [!NOTE]  
> - Für Azure China-Clouds muss der Parameter „spark.synapse.logAnalytics.keyVault.uriSuffix“ auf „ods.opinsights.azure.cn“ festgelegt werden. 
> - Für Azure Government-Clouds muss der Parameter „spark.synapse.logAnalytics.keyVault.uriSuffix“ auf „ods.opinsights.azure.us“ festgelegt werden. 

[uri_suffix]: https://docs.microsoft.com/azure/azure-monitor/logs/data-collector-api#request-uri


### <a name="step-3-upload-your-spark-configuration-to-a-spark-pool"></a>Schritt 3: Hochladen Ihrer Spark-Konfiguration in einen Spark-Pool
Sie können die Konfigurationsdatei in Ihren Synapse Spark-Pool in Synapse Studio hochladen.

   1. Navigieren Sie in Azure Synapse Studio zu Ihrem Apache Spark-Pool („Verwalten“ > „Apache Spark-Pools“).
   2. Klicken Sie rechts neben Ihrem Apache Spark-Pool auf die Schaltfläche **...** .
   3. Wählen Sie „Apache Spark-Konfiguration“ aus. 
   4. Klicken Sie auf **Hochladen**, und wählen Sie die erstellte Konfiguration **spark_loganalytics_conf.txt** aus.
   5. Klicken Sie auf **Hochladen** und anschließend auf **Anwenden**.

      > [!div class="mx-imgBorder"]
      > ![Spark-Poolkonfiguration](./media/apache-spark-azure-log-analytics/spark-pool-configuration.png)

> [!NOTE] 
>
> Von allen an den obigen Spark-Pool übermittelten Spark-Anwendungen wird die Konfigurationseinstellung verwendet, durch die Metriken und Protokolle der Spark-Anwendung an den angegebenen Azure Log Analytics-Arbeitsbereich gepusht werden.

## <a name="submit-a-spark-application-and-view-the-logs-and-metrics-in-azure-log-analytics"></a>Übermitteln einer Spark-Anwendung und Anzeigen der Protokolle und Metriken in Azure Log Analytics

 1. Verwenden Sie eine der folgenden Methoden, um eine Spark-Anwendung an den im vorherigen Schritt konfigurierten Spark-Pool zu übermitteln:
    - Ausführen eines Synapse Studio-Notebooks 
    - Übermitteln eines Synapse Apache Spark-Batchauftrags per Spark-Auftragsdefinition
    - Ausführen einer Pipeline mit einer Spark-Aktivität

 2. Navigieren Sie zum angegebenen Azure Log Analytics-Arbeitsbereich, und sehen Sie sich die Anwendungsmetriken und -protokolle an, wenn die Spark-Anwendung gestartet wird.

## <a name="use-the-sample-azure-log-analytics-workbook-to-visualize-the-metrics-and-logs"></a>Visualisieren der Metriken und Protokolle unter Verwendung der Azure Log Analytics-Beispielarbeitsmappe

1. [Laden Sie die Arbeitsmappe herunter.](https://aka.ms/SynapseSparkLogAnalyticsWorkbook)
2. Öffnen Sie die Arbeitsmappendatei, und **kopieren** Sie den Inhalt.
3. Navigieren Sie zur Azure Log Analytics-Arbeitsmappe ([Azure-Portal](https://portal.azure.com/) > „Log Analytics-Arbeitsbereich“ > „Arbeitsmappen“).
4. Öffnen Sie die Azure Log Analytics-Arbeitsmappe **Empty** (Leer) im Modus **Erweiterter Editor** (Symbol „</>“).
5. **Fügen** Sie alle vorhandenen JSON-Daten ein.
6. Wählen Sie anschließend **Anwenden** > **Bearbeitung abgeschlossen** aus.

    > [!div class="mx-imgBorder"]
    > ![Neue Arbeitsmappe](./media/apache-spark-azure-log-analytics/new-workbook.png)

    > [!div class="mx-imgBorder"]
    > ![Importieren einer Arbeitsmappe](./media/apache-spark-azure-log-analytics/import-workbook.png)

Übermitteln Sie als Nächstes Ihre Apache Spark-Anwendung an den konfigurierten Spark-Pool. Wenn sich die Anwendung im Ausführungszustand befindet, können Sie die ausgeführte Anwendung in der Dropdownliste der Arbeitsmappe auswählen.

> [!div class="mx-imgBorder"]
> ![Abbildung: Arbeitsmappe](./media/apache-spark-azure-log-analytics/workbook.png)

Außerdem können Sie die Arbeitsmappe per Kusto-Abfrage anpassen und Warnungen konfigurieren.

> [!div class="mx-imgBorder"]
> ![Kusto-Abfrage und Warnungen](./media/apache-spark-azure-log-analytics/kusto-query-and-alerts.png)

## <a name="sample-kusto-queries"></a>Kusto-Beispielabfragen

1. Beispiel für das Abfragen von Spark-Ereignissen:

   ```kusto
   SparkListenerEvent_CL
   | where workspaceName_s == "{SynapseWorkspace}" and clusterName_s == "{SparkPool}" and livyId_s == "{LivyId}"
   | order by TimeGenerated desc
   | limit 100 
   ```

2. Beispiel für das Abfragen von Treiber- und Executor-Protokollen der Spark-Anwendung:

   ```kusto
   SparkLoggingEvent_CL
   | where workspaceName_s == "{SynapseWorkspace}" and clusterName_s == "{SparkPool}" and livyId_s == "{LivyId}"
   | order by TimeGenerated desc
   | limit 100
   ```

3. Beispiel für das Abfragen von Spark-Metriken:

   ```kusto
   SparkMetrics_CL
   | where workspaceName_s == "{SynapseWorkspace}" and clusterName_s == "{SparkPool}" and livyId_s == "{LivyId}"
   | where name_s endswith "jvm.total.used"
   | summarize max(value_d) by bin(TimeGenerated, 30s), executorId_s
   | order by TimeGenerated asc
   ```

## <a name="create-and-manage-alerts-using-azure-log-analytics"></a>Erstellen und Verwalten von Warnungen mithilfe von Azure Log Analytics

Azure Monitor-Warnungen ermöglichen es Benutzern, eine Log Analytics-Abfrage zu verwenden, um Metriken und Protokolle mit einer bestimmten Häufigkeit auszuwerten und basierend auf den Ergebnissen eine Warnung auszulösen.

Weitere Informationen finden Sie unter [Erstellen, Anzeigen und Verwalten von Protokollwarnungen mithilfe von Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/alerts/alerts-log).

## <a name="limitation"></a>Einschränkung

 - Azure Synapse Analytics-Arbeitsbereiche mit aktiviertem [verwaltetem virtuellem Netzwerk](https://docs.microsoft.com/azure/synapse-analytics/security/synapse-workspace-managed-vnet) werden nicht unterstützt.
 - Folgende Regionen werden aktuell nicht unterstützt:
   - USA (Ost) 2
   - Norwegen, Osten
   - Vereinigte Arabische Emirate, Norden

## <a name="next-steps"></a>Nächste Schritte

 - [Schnellstart: Erstellen eines serverlosen Apache Spark-Pools mithilfe von Synapse Studio](https://docs.microsoft.com/azure/synapse-analytics/quickstart-create-apache-spark-pool-studio)
 - [Erstellen, Entwickeln und Verwalten von Synapse Studio-Notebooks in Azure Synapse Analytics](https://docs.microsoft.com/azure/synapse-analytics/spark/apache-spark-development-using-notebooks)
 - [Tutorial: Erstellen einer Apache Spark-Auftragsdefinition in Synapse Studio](https://docs.microsoft.com/azure/synapse-analytics/spark/apache-spark-job-definitions)