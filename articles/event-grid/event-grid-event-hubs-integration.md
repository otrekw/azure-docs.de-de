---
title: 'Tutorial: Senden von Event Hubs-Daten an Data Warehouse – Event Grid'
description: 'Tutorial: Hier wird erläutert, wie Daten mithilfe von Azure Event Grid und Event Hubs zu Azure Synapse Analytics migriert werden. Zum Abrufen einer Capture-Datei wird eine Azure-Funktion verwendet.'
ms.topic: tutorial
ms.date: 07/07/2020
ms.custom: devx-track-csharp, devx-track-azurecli
ms.openlocfilehash: 42a2f7fd557970328f6d88b08e296317cecd8c66
ms.sourcegitcommit: 6a350f39e2f04500ecb7235f5d88682eb4910ae8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/01/2020
ms.locfileid: "96462146"
---
# <a name="tutorial-stream-big-data-into-a-data-warehouse"></a>Tutorial: Streamen von Big Data in ein Data Warehouse
Bei Azure [Event Grid](overview.md) handelt es sich um einen intelligenten Ereignisroutingdienst, der es Ihnen ermöglicht, auf Benachrichtigungen (Ereignisse) von Apps und Diensten zu reagieren. Er kann beispielsweise eine Azure-Funktion auslösen, um Event Hubs-Daten zu verarbeiten, die in Azure Blob Storage oder Azure Data Lake Storage erfasst wurden, und die Daten zu anderen Datenrepositorys migrieren. Das [Beispiel für die Integration von Event Hubs und Event Grid](https://github.com/Azure/azure-event-hubs/tree/master/samples/e2e/EventHubsCaptureEventGridDemo) veranschaulicht, wie Sie Event Hubs mit Event Grid für die nahtlose Migration von erfassten Event Hubs-Daten vom Blobspeicher zu einer Azure Synapse Analytics-Instanz verwenden.

![Anwendungsübersicht](media/event-grid-event-hubs-integration/overview.png)

Dieses Diagramm zeigt den Workflow der Lösung, die Sie in diesem Tutorial erstellen: 

1. Daten, die an einen Azure Event Hub gesendet werden, werden in einem Azure Blob Storage erfasst.
2. Wenn die Datenerfassung abgeschlossen ist, wird ein Ereignis erzeugt und an ein Azure Event Grid gesendet. 
3. Das Event Grid leitet diese Ereignisdaten an eine Azure-Funktions-App weiter.
4. Die Funktions-App verwendet die Blob-URL in den Event-Daten, um den Blob aus dem Speicher abzurufen. 
5. Die Funktions-App migriert die Blobdaten zu einer Azure Synapse Analytics-Instanz. 

In diesem Artikel führen Sie die folgenden Schritte aus:

> [!div class="checklist"]
> * Verwenden Sie eine Azure Resource Manager-Vorlage für die Bereitstellung der Infrastruktur: einen Event Hub, ein Speicherkonto, eine Funktions-App, einen dedizierten SQL-Pool.
> * Erstellen Sie eine Tabelle im dedizierten SQL-Pool.
> * Fügen Sie Code zur Funktions-App hinzu.
> * Abonnieren Sie das Ereignis. 
> * Führen Sie die App aus, die Daten an den Event Hub sendet
> * Zeigen Sie migrierte Daten im Data Warehouse an.

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Für dieses Tutorial benötigen Sie Folgendes:

* Ein Azure-Abonnement. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.
* [Visual Studio 2019](https://www.visualstudio.com/vs/) mit Workloads für .NET-Desktopentwicklung, Azure-Entwicklung, ASP.NET- und Webentwicklung, Node.js-Entwicklung und Python-Entwicklung.
* Laden Sie das Beispielprojekt [EventHubsCaptureEventGridDemo](https://github.com/Azure/azure-event-hubs/tree/master/samples/e2e/EventHubsCaptureEventGridDemo) auf Ihren Computer herunter.

## <a name="deploy-the-infrastructure"></a>Bereitstellen der Infrastruktur
In diesem Schritt stellen Sie die erforderliche Infrastruktur mit einer [Resource Manager-Vorlage](https://github.com/Azure/azure-docs-json-samples/blob/master/event-grid/EventHubsDataMigration.json) bereit. Wenn Sie die Vorlage bereitstellen, werden die folgenden Ressourcen erstellt:

* Event Hub mit aktiviertem Capture-Feature.
* Speicherkonto für die erfassten Dateien. 
* App Service-Plan zum Hosten der Funktions-App
* Funktions-App zum Verarbeiten des Ereignisses
* SQL Server zum Hosten des Data Warehouse
* Azure Synapse Analytics zum Speichern der migrierten Daten

### <a name="launch-azure-cloud-shell-in-azure-portal"></a>Starten von Azure Cloud Shell im Azure-Portal

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an. 
2. Wählen Sie oben die Schaltfläche **Cloud Shell** aus.

    ![Azure-Portal](media/event-grid-event-hubs-integration/azure-portal.png)
3. Die Cloud Shell wird unten im Browser geöffnet.

    ![Cloud Shell](media/event-grid-event-hubs-integration/launch-cloud-shell.png) 
4. Wenn Sie in der Cloud Shell eine Option zur Auswahl zwischen **Bash** und **PowerShell** sehen, wählen Sie **Bash** aus. 
5. Wenn Sie die Cloud Shell zum ersten Mal verwenden, erstellen Sie ein Speicherkonto, indem Sie **Speicher erstellen** auswählen. Zum Speichern von Daten benötigt Azure Cloud Shell ein Azure-Speicherkonto. 

    ![Screenshot: Dialogfeld „Für Sie wurde kein Speicher bereitgestellt.“ mit hervorgehobener Schaltfläche „Speicher erstellen“](media/event-grid-event-hubs-integration/create-storage-cloud-shell.png)
6. Warten Sie, bis die Cloud Shell initialisiert wird. 

    ![Erstellen von Speicher für Cloud Shell](media/event-grid-event-hubs-integration/cloud-shell-initialized.png)


### <a name="use-azure-cli"></a>Mithilfe der Azure-Befehlszeilenschnittstelle

1. Erstellen Sie eine Azure-Ressourcengruppe, indem Sie den folgenden CLI-Befehl ausführen: 
    1. Kopieren Sie den folgenden Befehl, und fügen Sie ihn in das Cloud Shell-Fenster ein. Ändern Sie ggf. den Namen der Ressourcengruppe und des Standorts.

        ```azurecli
        az group create -l eastus -n rgDataMigration
        ```
    2. Drücken Sie die **EINGABETASTE**. 

        Beispiel:
    
        ```azurecli
        user@Azure:~$ az group create -l eastus -n ehubegridgrp
        {
          "id": "/subscriptions/00000000-0000-0000-0000-0000000000000/resourceGroups/ehubegridgrp",
          "location": "eastus",
          "managedBy": null,
          "name": "ehubegridgrp",
          "properties": {
            "provisioningState": "Succeeded"
          },
          "tags": null
        }
        ```
2. Stellen Sie alle im vorherigen Abschnitt genannten Ressourcen (Event Hub, Speicherkonto, Funktions-App, Azure Synapse Analytics) bereit, indem Sie den folgenden CLI-Befehl ausführen: 
    1. Kopieren Sie den folgenden Befehl in das Cloud Shell-Fenster. Alternativ können Sie auch in einen Editor Ihrer Wahl kopieren/einfügen, Werte festlegen und dann den Befehl in die Cloud Shell kopieren. 

        ```azurecli
        az group deployment create \
            --resource-group rgDataMigration \
            --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/event-grid/EventHubsDataMigration.json \
            --parameters eventHubNamespaceName=<event-hub-namespace> eventHubName=hubdatamigration sqlServerName=<sql-server-name> sqlServerUserName=<user-name> sqlServerPassword=<password> sqlServerDatabaseName=<database-name> storageName=<unique-storage-name> functionAppName=<app-name>
        ```
    2. Geben Sie Werte für die folgenden Entitäten an:
        1. Name der Ressourcengruppe, die Sie zuvor erstellt haben.
        2. Name des Event Hub-Namespace. 
        3. Name für den Event Hub. Sie können den Wert unverändert lassen (hubdatamigration).
        4. Name für den SQL Server.
        5. Name und Kennwort des SQL-Benutzers 
        6. Name für Azure Synapse Analytics
        7. Name des Speicherkontos. 
        8. Name der Funktionen-App. 
    3.  Drücken Sie **EINGABETASTE** i, Cloud Shell-Fenster zum Ausführen des Befehls. Dieser Vorgang kann einige Zeit in Anspruch nehmen, da Sie zahlreiche Ressourcen erstellen. Stellen Sie im Ergebnis des Befehls sicher, dass es keine Fehler gegeben hat. 
    

### <a name="use-azure-powershell"></a>Mithilfe von Azure PowerShell

1. Wechseln Sie in Azure Cloud Shell zum PowerShell-Modus. Wählen Sie den Pfeil nach unten in der linken oberen Ecke der Azure Cloud Shell, und wählen Sie **PowerShell**.

    ![Wechsel zu PowerShell](media/event-grid-event-hubs-integration/select-powershell-cloud-shell.png)
2. Erstellen Sie eine Azure-Ressourcengruppe, indem Sie den folgenden Befehl ausführen: 
    1. Kopieren Sie den folgenden Befehl, und fügen Sie ihn in das Cloud Shell-Fenster ein.

        ```powershell
        New-AzResourceGroup -Name rgDataMigration -Location eastus
        ```
    2. Geben Sie einen Namen für die **Ressourcengruppe** an.
    3. Drücken Sie die EINGABETASTE. 
3. Stellen Sie alle im vorherigen Abschnitt genannten Ressourcen (Event Hub, Speicherkonto, Funktions-App, Azure Synapse Analytics) bereit, indem Sie den folgenden Befehl ausführen:
    1. Kopieren Sie den folgenden Befehl in das Cloud Shell-Fenster. Alternativ können Sie auch in einen Editor Ihrer Wahl kopieren/einfügen, Werte festlegen und dann den Befehl in die Cloud Shell kopieren. 

        ```powershell
        New-AzResourceGroupDeployment -ResourceGroupName rgDataMigration -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/event-grid/EventHubsDataMigration.json -eventHubNamespaceName <event-hub-namespace> -eventHubName hubdatamigration -sqlServerName <sql-server-name> -sqlServerUserName <user-name> -sqlServerDatabaseName <database-name> -storageName <unique-storage-name> -functionAppName <app-name>
        ```
    2. Geben Sie Werte für die folgenden Entitäten an:
        1. Name der Ressourcengruppe, die Sie zuvor erstellt haben.
        2. Name des Event Hub-Namespace. 
        3. Name für den Event Hub. Sie können den Wert unverändert lassen (hubdatamigration).
        4. Name für den SQL Server.
        5. Name und Kennwort des SQL-Benutzers 
        6. Name für Azure Synapse Analytics
        7. Name des Speicherkontos. 
        8. Name der Funktionen-App. 
    3.  Drücken Sie **EINGABETASTE** i, Cloud Shell-Fenster zum Ausführen des Befehls. Dieser Vorgang kann einige Zeit in Anspruch nehmen, da Sie zahlreiche Ressourcen erstellen. Stellen Sie im Ergebnis des Befehls sicher, dass es keine Fehler gegeben hat. 

### <a name="close-the-cloud-shell"></a>Schließen der Cloud Shell 
Schließen Sie die Cloud Shell, indem Sie die Schaltfläche **Cloud Shell** im Portal (oder) **X** in der rechten oberen Ecke des Cloud Shell-Fensters auswählen. 

### <a name="verify-that-the-resources-are-created"></a>Überprüfen, ob die Ressourcen erstellt wurden

1. Wählen Sie im Azure-Portals im linken Menü **Ressourcengruppe**. 
2. Filtern Sie die Liste der Ressourcengruppen, indem Sie den Namen Ihrer Ressourcengruppe in das Suchfeld eingeben. 
3. Wählen Sie Ihre Ressourcengruppe in der Liste aus.

    ![Ressourcengruppe auswählen](media/event-grid-event-hubs-integration/select-resource-group.png)
4. Überprüfen Sie, ob die folgenden Ressourcen in der Ressourcengruppe aufgeführt sind:

    ![Ressourcen in der Ressourcengruppe](media/event-grid-event-hubs-integration/resources-in-resource-group.png)

### <a name="create-a-table-in-azure-synapse-analytics"></a>Erstellen einer Tabelle in Azure Synapse Analytics
Erstellen Sie in Ihrem Data Warehouse durch Ausführen des Skripts [CreateDataWarehouseTable.sql](https://github.com/Azure/azure-event-hubs/blob/master/samples/e2e/EventHubsCaptureEventGridDemo/scripts/CreateDataWarehouseTable.sql) eine Tabelle. Verwenden Sie zum Ausführen des Skripts Visual Studio oder den Abfrage-Editor im Portal. Die folgenden Schritte veranschaulichen die Verwendung des Query-Editors: 

1. Wählen Sie in der Liste der Ressourcen in der Ressourcengruppe Ihren **dedizierten SQL-Pool** aus. 
2. Wählen Sie auf der Seite „Azure Synapse Analytics“ im linken Menü **Abfrage-Editor (Vorschauversion)** aus. 

    ![Seite „Azure Synapse Analytics“](media/event-grid-event-hubs-integration/sql-data-warehouse-page.png)
2. Geben Sie den Namen für **Benutzer** und **Kennwort** für die SQL Server ein, und wählen **OK**. Unter Umständen müssen Sie Ihre Client-IP-Adresse der Firewall hinzufügen, um sich bei SQL Server anzumelden. 

    ![SQL Server-Authentifizierung](media/event-grid-event-hubs-integration/sql-server-authentication.png)
4. Fügen Sie das folgende SQL-Skript ins Abfragefenster ein, und führen Sie es aus: 

    ```sql
    CREATE TABLE [dbo].[Fact_WindTurbineMetrics] (
        [DeviceId] nvarchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL, 
        [MeasureTime] datetime NULL, 
        [GeneratedPower] float NULL, 
        [WindSpeed] float NULL, 
        [TurbineSpeed] float NULL
    )
    WITH (CLUSTERED COLUMNSTORE INDEX, DISTRIBUTION = ROUND_ROBIN);
    ```

    ![SQL-Abfrage ausführen](media/event-grid-event-hubs-integration/run-sql-query.png)
5. Halten Sie diese Registerkarte oder dieses Fenster offen, damit Sie überprüfen können, ob die Daten am Ende des Tutorials erstellt wurden. 

### <a name="update-the-function-runtime-version"></a>Aktualisieren der Funktionsruntimeversion

1. Wählen Sie im Azure-Portals im linken Menü **Ressourcengruppe**.
2. Wählen Sie die Ressourcengruppe aus, in der die Funktions-App enthalten ist. 
3. Wählen Sie in der Liste der Ressourcen in der Ressourcengruppe die Funktions-App vom Typ **App Service** aus.
4. Wählen Sie im linken Menü unter **Einstellungen** die Option **Konfiguration** aus. 
5. Wechseln Sie im rechten Bereich zur Registerkarte **Runtimeeinstellungen der Funktion**. 
5. Aktualisieren Sie die **Runtimeversion** auf **~3**. 

    ![Aktualisieren der Funktionsruntimeversion](media/event-grid-event-hubs-integration/function-runtime-version.png)
    

## <a name="publish-the-azure-functions-app"></a>Veröffentlichen der Azure Functions-App

1. Starten Sie Visual Studio.
2. Öffnen Sie die Lösung **EventHubsCaptureEventGridDemo.sln**, die Sie aus [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/e2e/EventHubsCaptureEventGridDemo) als Teil der Voraussetzungen heruntergeladen haben.
3. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf **FunctionEGDWDumper**, und wählen Sie **Veröffentlichen** aus.

   ![Veröffentlichen einer Funktions-App](media/event-grid-event-hubs-integration/publish-function-app.png)
4. Wenn der folgende Bildschirm angezeigt wird, wählen Sie **Start**. 

   ![Screenshot: Visual Studio mit der Schaltfläche „Start“ im Abschnitt „Veröffentlichen“](media/event-grid-event-hubs-integration/start-publish-button.png) 
5. Wählen Sie im Dialogfeld **Veröffentlichen** unter **Ziel** die Option **Azure** und dann **Weiter** aus. 

   ![Schaltfläche „Veröffentlichen starten“](media/event-grid-event-hubs-integration/publish-select-azure.png)
6. Wählen Sie **Azure-Funktions-App (Windows)** und dann **Weiter** aus. 

   ![Auswählen von „Azure-Funktions-App (Windows)“](media/event-grid-event-hubs-integration/select-azure-function-windows.png)
7. Wählen Sie auf der Registerkarte **Functions-Instanz** Ihr Azure-Abonnement aus, erweitern Sie die Ressourcengruppe, und wählen Sie Ihre Funktions-App und anschließend **Fertig stellen** aus. Sie müssen sich bei Ihrem Azure-Konto anmelden, wenn Sie dies nicht bereits getan haben. 

   ![Auswählen Ihrer Funktionen-App](media/event-grid-event-hubs-integration/publish-select-function-app.png)
8. Wählen Sie im Abschnitt **Dienstabhängigkeiten** die Option **Konfigurieren** aus.
9. Wählen Sie auf der Seite **Abhängigkeit konfigurieren** das zuvor erstellte Speicherkonto und dann **Weiter** aus. 
10. Übernehmen Sie die Einstellungen für Name und Wert der Verbindungszeichenfolge, und wählen Sie **Weiter** aus.
11. Deaktivieren Sie die Option **Geheimnisspeicher**, und wählen Sie dann **Fertig stellen** aus.  
8. Wenn Visual Studio das Profil konfiguriert hat, wählen Sie **Veröffentlichen**.

   ![Wählen Sie "Veröffentlichen"](media/event-grid-event-hubs-integration/select-publish.png)

Nach dem Veröffentlichen der Funktion können Sie das Ereignis abonnieren.

## <a name="subscribe-to-the-event"></a>Abonnieren des Ereignisses

1. Navigieren Sie in einer neuen Registerkarte oder einem neuen Fenster eines Webbrowsers zum [Azur-Portal](https://portal.azure.com).
2. Wählen Sie im Azure-Portals im linken Menü **Ressourcengruppe**. 
3. Filtern Sie die Liste der Ressourcengruppen, indem Sie den Namen Ihrer Ressourcengruppe in das Suchfeld eingeben. 
4. Wählen Sie Ihre Ressourcengruppe in der Liste aus.

    ![Ressourcengruppe auswählen](media/event-grid-event-hubs-integration/select-resource-group.png)
4. Wählen Sie in der Liste der Ressourcen in der Ressourcengruppe den App Service-Plan (nicht App Service) aus. 
5. Wählen Sie auf der Seite „App Service Plan“ im linken Menü **Apps** und dann die Funktions-App aus. 

    ![Auswählen Ihrer Funktions-App](media/event-grid-event-hubs-integration/select-function-app-app-service-plan.png)
6. Erweitern Sie die Funktions-App und die Funktionen, und wählen Sie dann Ihre Funktion. 
7. Wählen Sie **Event Grid-Abonnement hinzufügen** in der Symbolleiste aus. 

    ![Auswählen Ihrer Azure-Funktion](media/event-grid-event-hubs-integration/select-function-add-button.png)
8. Führen Sie auf der Seite **Azure Event Grid-Abonnement** die folgenden Aktionen aus: 
    1. Geben Sie auf der Seite **EREIGNISABONNEMENTDETAILS** einen Namen für das Abonnement ein (z.B. captureEventSub) und wählen Sie **Erstellen** aus. 
    2. Führen Sie im Abschnitt **THEMENDETAILS** die folgenden Aktionen aus:
        1. Wählen Sie unter **Thementypen** die Option **Event Hubs-Namespaces** aus. 
        2. Wählen Sie Ihr Azure-Abonnement.
        2. Wählen Sie die Azure-Ressourcengruppe aus.
        3. Wählen Sie Ihren Event Hubs-Namespace aus.
    3. Vergewissern Sie sich im Abschnitt **EREIGNISTYPEN**, dass unter **Nach Ereignistypen filtern** die Option **Capture File Created** (Capture-Datei erstellt) ausgewählt ist. 
    4. Vergewissern Sie sich im Abschnitt **ENDPUNKTDETAILS**, dass **Endpunkttyp** auf **Azure-Funktion** und **Endpunkt** auf die Azure-Funktion festgelegt ist. 
    
        ![Event Grid-Abonnement erstellen](media/event-grid-event-hubs-integration/create-event-subscription.png)

## <a name="run-the-app-to-generate-data"></a>Ausführen der App zum Generieren von Daten
Sie haben nun Event Hub, Azure Synapse Analytics, die Azure-Funktions-App und das Ereignisabonnement fertig eingerichtet. Vor dem Ausführen einer Anwendung, die Daten für Event Hubs generiert, müssen Sie einige Werte konfigurieren.

1. Navigieren Sie im Azure-Portal zu der Ressourcengruppe, die Sie zuvor erstellt haben. 
2. Wähen Sie den Event Hubs-Namespace aus.
3. Wählen Sie auf der Seite **Event Hubs-Namespace** die Option **SAS-Richtlinien** im linken Menü aus.
4. Wählen Sie in der Richtlinienliste **RootManageSharedAccessKey** aus. 
5. Wählen Sie die Schaltfläche „Kopieren“ neben dem Textfeld **Verbindungszeichenfolge-primärer Schlüssel** aus. 

    ![Verbindungszeichenfolge für den Event Hub-Namespace](media/event-grid-event-hubs-integration/get-connection-string.png)
1. Kehren Sie zur Visual Studio-Lösung zurück. 
2. Öffnen Sie **program.cs** im Projekt „WindTurbineDataGenerator“.
5. Ersetzen Sie die zwei konstanten Werte. Verwenden Sie für **EventHubConnectionString** den kopierten Wert. Verwenden Sie **hubdatamigration** als Event Hub-Name. Wenn Sie einen anderen Namen für den Event Hub verwendet haben, geben Sie diesen Namen an. 

   ```cs
   private const string EventHubConnectionString = "Endpoint=sb://demomigrationnamespace.servicebus.windows.net/...";
   private const string EventHubName = "hubdatamigration";
   ```

6. Erstellen Sie die Projektmappe. Führen Sie die Anwendung **WindTurbineGenerator.exe** aus. 
7. Fragen Sie nach ein paar Minuten in der Tabelle im Data Warehouse die migrierten Daten ab.

    ![Abfrageergebnisse](media/event-grid-event-hubs-integration/query-results.png)

### <a name="event-data-generated-by-the-event-hub"></a>Vom Event Hub generierte Ereignisdaten
Event Grid verteilt Ereignisdaten an die Abonnenten. Das folgende Beispiel zeigt Ereignisdaten, die erzeugt werden, wenn Daten, die über einen Event Hub übertragen werden, in einem Blob erfasst werden. Beachten Sie ins Besondere, dass die `fileUrl`-Eigenschaft im `data`-Objekt auf den Blob im Speicher verweist. Die Funktions-App verwendet diese URL zum Abrufen der Blobdatei mit den erfassten Daten.

```json
[
    {
        "topic": "/subscriptions/<guid>/resourcegroups/rgDataMigrationSample/providers/Microsoft.EventHub/namespaces/tfdatamigratens",
        "subject": "eventhubs/hubdatamigration",
        "eventType": "Microsoft.EventHub.CaptureFileCreated",
        "eventTime": "2017-08-31T19:12:46.0498024Z",
        "id": "14e87d03-6fbf-4bb2-9a21-92bd1281f247",
        "data": {
            "fileUrl": "https://tf0831datamigrate.blob.core.windows.net/windturbinecapture/tfdatamigratens/hubdatamigration/1/2017/08/31/19/11/45.avro",
            "fileType": "AzureBlockBlob",
            "partitionId": "1",
            "sizeInBytes": 249168,
            "eventCount": 1500,
            "firstSequenceNumber": 2400,
            "lastSequenceNumber": 3899,
            "firstEnqueueTime": "2017-08-31T19:12:14.674Z",
            "lastEnqueueTime": "2017-08-31T19:12:44.309Z"
        }
    }
]
```


## <a name="next-steps"></a>Nächste Schritte

* Informationen zu den Unterschieden in der Azure-Messagingdiensten finden Sie unter [Wählen zwischen Azure-Diensten, die Nachrichten übermitteln](compare-messaging-services.md).
* Eine Einführung in Event Grid finden Sie unter [Informationen zu Event Grid](overview.md).
* Eine Einführung zu Event Hubs Capture finden Sie unter [Aktivieren von Event Hubs Capture über das Azure-Portal](../event-hubs/event-hubs-capture-enable-through-portal.md).
* Weitere Informationen zum Einrichten und Ausführen des Beispiels finden Sie unter [Event Hubs Capture and Event Grid sample](https://github.com/Azure/azure-event-hubs/tree/master/samples/e2e/EventHubsCaptureEventGridDemo) (Beispiel für Events Hubs Capture und Event Grid).
