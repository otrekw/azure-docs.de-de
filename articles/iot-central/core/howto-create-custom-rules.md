---
title: Erweitern von Azure IoT Central mit benutzerdefinierten Regeln und Benachrichtigungen | Microsoft-Dokumentation
description: Als Lösungsentwickler möchten Sie eine IoT Central-Anwendung konfigurieren, die E-Mail-Benachrichtigungen sendet, wenn ein Gerät keine Telemetriedaten mehr übermittelt. Bei dieser Lösung werden Azure Stream Analytics, Azure Functions und SendGrid verwendet.
author: TheJasonAndrew
ms.author: v-anjaso
ms.date: 02/09/2021
ms.topic: how-to
ms.service: iot-central
services: iot-central
ms.custom: mvc, devx-track-csharp
manager: philmea
ms.openlocfilehash: 824308b66803d2dfa05383ff06ce97c48626619d
ms.sourcegitcommit: de98cb7b98eaab1b92aa6a378436d9d513494404
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/17/2021
ms.locfileid: "100557560"
---
# <a name="extend-azure-iot-central-with-custom-rules-using-stream-analytics-azure-functions-and-sendgrid"></a>Erweitern von Azure IoT Central mit benutzerdefinierten Regeln mithilfe von Stream Analytics, Azure Functions und SendGrid

Diese Schrittanleitung veranschaulicht, wie Sie als Lösungsentwickler Ihre IoT Central-Anwendung um benutzerdefinierte Regeln und Benachrichtigungen erweitern können. Das Beispiel zeigt, wie eine Benachrichtigung an einen Bediener gesendet wird, wenn ein Gerät keine Telemetriedaten mehr sendet. Die Lösung verwendet eine [Azure Stream Analytics](../../stream-analytics/index.yml)-Abfrage, um zu erkennen, wenn ein Gerät das Senden von Telemetriedaten einstellt. Der Stream Analytics-Auftrag verwendet [Azure Functions](../../azure-functions/index.yml), um mithilfe von [SendGrid](https://sendgrid.com/docs/for-developers/partners/microsoft-azure/) Benachrichtigungs-E-Mails zu senden.

Diese Schrittanleitung zeigt, wie Sie IoT Central mithilfe von integrierten Regeln und Aktionen über die bisherige Funktionalität hinaus erweitern.

In dieser Schrittanleitung erfahren Sie Folgendes:

* Streamen von Telemetriedaten aus einer IoT Central-Anwendung mithilfe des *kontinuierlichen Datenexports*
* Erstellen einer Stream Analytics-Abfrage, die erkennt, wenn ein Gerät keine Daten mehr sendet
* Senden einer E-Mail-Benachrichtigung mithilfe der Dienste Azure Functions und SendGrid

## <a name="prerequisites"></a>Voraussetzungen

Um die in dieser Anleitung aufgeführten Schritte ausführen zu können, benötigen Sie ein aktives Azure-Abonnement.

Wenn Sie kein Azure-Abonnement besitzen, erstellen Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F), bevor Sie beginnen.

### <a name="iot-central-application"></a>IoT Central-Anwendung

Erstellen Sie über die Website des [Azure IoT Central-Anwendungs-Managers](https://aka.ms/iotcentral) eine IoT Central-Anwendung mit folgenden Einstellungen:

| Einstellung | value |
| ------- | ----- |
| Tarif | Standard |
| Anwendungsvorlage | In-Store-Analyse – Bedingungsüberwachung |
| Anwendungsname | Standardwert übernehmen oder eigenen Namen angeben |
| URL | Standardwert übernehmen oder eigenes eindeutiges URL-Präfix angeben |
| Verzeichnis | Ihr Azure Active Directory-Mandant |
| Azure-Abonnement | Ihr Azure-Abonnement |
| Region | Ihre nächstgelegene Region |

Bei den Beispielen und Screenshots in diesem Artikel wird die Region **USA** verwendet. Wählen Sie einen Standort in Ihrer Nähe, und stellen Sie sicher, dass Sie alle Ressourcen in derselben Region erstellen.

Diese Anwendungsvorlage enthält zwei simulierte Thermostatgeräte, die Telemetriedaten senden.

### <a name="resource-group"></a>Resource group

Verwenden Sie das [Azure-Portal zum Erstellen einer Ressourcengruppe](https://portal.azure.com/#create/Microsoft.ResourceGroup) namens **DetectStoppedDevices**. Diese soll die weiteren Ressourcen enthalten, die Sie erstellen. Erstellen Sie Ihre Azure-Ressourcen am gleichen Standort wie Ihre IoT Central-Anwendung.

### <a name="event-hubs-namespace"></a>Event Hubs-Namespace

Verwenden Sie das [Azure-Portal zum Erstellen eines Event Hubs-Namespace](https://portal.azure.com/#create/Microsoft.EventHub) mit den folgenden Einstellungen:

| Einstellung | value |
| ------- | ----- |
| Name    | Namen Ihres Namespace auswählen |
| Tarif | Basic |
| Subscription | Ihr Abonnement |
| Resource group | DetectStoppedDevices |
| Location | East US |
| Durchsatzeinheiten | 1 |

### <a name="stream-analytics-job"></a>Stream Analytics-Auftrag

Verwenden Sie das [Azure-Portal zum Erstellen eines Stream Analytics-Namespace](https://portal.azure.com/#create/Microsoft.StreamAnalyticsJob) mit den folgenden Einstellungen:

| Einstellung | value |
| ------- | ----- |
| Name    | Namen Ihres Auftrags auswählen |
| Subscription | Ihr Abonnement |
| Resource group | DetectStoppedDevices |
| Location | East US |
| Hosting-Umgebung | Cloud |
| Streamingeinheiten | 3 |

### <a name="function-app"></a>Funktionen-App

Verwenden Sie das [Azure-Portal zum Erstellen einer Funktions-App](https://portal.azure.com/#create/Microsoft.FunctionApp) mit den folgenden Einstellungen:

| Einstellung | value |
| ------- | ----- |
| App-Name    | Namen Ihrer Funktions-App auswählen |
| Subscription | Ihr Abonnement |
| Resource group | DetectStoppedDevices |
| OS | Windows |
| Hostingplan | Verbrauchstarif |
| Location | East US |
| Laufzeitstapel | .NET |
| Storage | Neu erstellen |

### <a name="sendgrid-account-and-api-keys"></a>SendGrid-Konto und API-Schlüssel

Sollten Sie nicht über ein SendGrid-Konto verfügen, erstellen Sie zunächst ein [kostenloses Konto](https://app.sendgrid.com/).

1. Wählen Sie in den Dashboard-Einstellungen von SendGrid im linken Menü **API-Schlüssel** aus.
1. Klicken Sie auf **API-Schlüssel erstellen**.
1. Benennen Sie den neuen API-Schlüssel mit **AzureFunctionAccess**.
1. Klicken Sie auf **Erstellen und anzeigen**.

    :::image type="content" source="media/howto-create-custom-rules/sendgrid-api-keys.png" alt-text="Screenshot der Erstellung eines SendGrid-API-Schlüssels":::

Anschließend erhalten Sie einen API-Schlüssel. Speichern Sie diese Zeichenfolge für die spätere Verwendung.

## <a name="create-an-event-hub"></a>Erstellen eines Ereignis-Hubs

Sie können eine IoT Central-Anwendung konfigurieren, um Telemetriedaten kontinuierlich an einen Event Hub zu exportieren. In diesem Abschnitt erstellen Sie einen Event Hub, um Telemetriedaten aus Ihrer IoT Central-Anwendung zu empfangen. Der Event Hub übermittelt die Telemetriedaten zur Verarbeitung an Ihren Stream Analytics-Auftrag.

1. Navigieren Sie im Azure-Portal zu Ihrem Event Hubs-Namespace, und klicken Sie auf **+ Event Hub**.
1. Nennen Sie den Event Hub **centralexport**, und klicken Sie auf **Erstellen**.

Ihr Event Hubs-Namespace sieht wie im folgenden Screenshot aus: 

```:::image type="content" source="media/howto-create-custom-rules/event-hubs-namespace.png" alt-text="Screenshot of Event Hubs namespace." border="false":::

## Define the function

This solution uses an Azure Functions app to send an email notification when the Stream Analytics job detects a stopped device. To create your function app:

1. In the Azure portal, navigate to the **App Service** instance in the **DetectStoppedDevices** resource group.
1. Select **+** to create a new function.
1. Select **HTTP Trigger**.
1. Select **Add**.

    :::image type="content" source="media/howto-create-custom-rules/add-function.png" alt-text="Image of the Default HTTP trigger function"::: 

## Edit code for HTTP Trigger

The portal creates a default function called **HttpTrigger1**:

```:::image type="content" source="media/howto-create-custom-rules/default-function.png" alt-text="Screenshot of Edit HTTP trigger function.":::

1. Replace the C# code with the following code:

    ```csharp
    #r "Newtonsoft.Json"
    #r "SendGrid"
    using System;
    using SendGrid.Helpers.Mail;
    using Microsoft.Azure.WebJobs.Host;
    using Microsoft.AspNetCore.Mvc;
    using Microsoft.Extensions.Primitives;
    using Newtonsoft.Json;

    public static SendGridMessage Run(HttpRequest req, ILogger log)
    {
        string requestBody = new StreamReader(req.Body).ReadToEnd();
        log.LogInformation(requestBody);
        var notifications = JsonConvert.DeserializeObject<IList<Notification>>(requestBody);

        SendGridMessage message = new SendGridMessage();
        message.Subject = "Contoso device notification";

        var content = "The following device(s) have stopped sending telemetry:<br/><br/><table><tr><th>Device ID</th><th>Time</th></tr>";
        foreach(var notification in notifications) {
            log.LogInformation($"No message received - Device: {notification.deviceid}, Time: {notification.time}");
            content += $"<tr><td>{notification.deviceid}</td><td>{notification.time}</td></tr>";
        }
        content += "</table>";
        message.AddContent("text/html", content);  

        return message;
    }

    public class Notification
    {
        public string deviceid { get; set; }
        public string time { get; set; }
    }
    ```

    You may see an error message until you save the new code.
1. Select **Save** to save the function.

## Add SendGrid Key

To add your SendGrid API Key, you need to add it to your **Function Keys** as follows:

1. Select **Function Keys**.
1. Choose **+ New Function Key**.
1. Enter the *Name* and *Value* of the API Key you created before.
1. Click **OK.**

    :::image type="content" source="media/howto-create-custom-rules/add-key.png" alt-text="Screenshot of Add Sangrid Key.":::


## Configure HttpTrigger function to use SendGrid

To send emails with SendGrid, you need to configure the bindings for your function as follows:

1. Select **Integrate**.
1. Choose **Add Output** under **HTTP ($return)**.
1. Select **Delete.**
1. Choose **+ New Output**.
1. For Binding Type, then choose **SendGrid**.
1. For SendGrid API Key Setting Type, click New.
1. Enter the *Name* and *Value* of your SendGrid API key.
1. Add the following information:

| Setting | Value |
| ------- | ----- |
| Message parameter name | Choose your name |
| To address | Choose the name of your To Address |
| From address | Choose the name of your From Address |
| Message subject | Enter your subject header |
| Message text | Enter the message from your integration |

1. Select **OK**.

    :::image type="content" source="media/howto-create-custom-rules/add-output.png" alt-text="Screenshot of Add SandGrid Output.":::


### Test the function works

To test the function in the portal, first choose **Logs** at the bottom of the code editor. Then choose **Test** to the right of the code editor. Use the following JSON as the **Request body**:

```json
[{"deviceid":"test-device-1","time":"2019-05-02T14:23:39.527Z"},{"deviceid":"test-device-2","time":"2019-05-02T14:23:50.717Z"},{"deviceid":"test-device-3","time":"2019-05-02T14:24:28.919Z"}]
```

Die Meldungen des Funktionsprotokolls werden im Bereich **Protokolle** angezeigt:

```:::image type="content" source="media/howto-create-custom-rules/function-app-logs.png" alt-text="Function log output":::

After a few minutes, the **To** email address receives an email with the following content:

```txt
The following device(s) have stopped sending telemetry:

Device ID    Time
test-device-1    2019-05-02T14:23:39.527Z
test-device-2    2019-05-02T14:23:50.717Z
test-device-3    2019-05-02T14:24:28.919Z
```

## <a name="add-stream-analytics-query"></a>Hinzufügen einer Stream Analytics-Abfrage

Die Lösung verwendet eine Stream Analytics-Abfrage, um zu erkennen, wenn ein Gerät mehr als 120 Sekunden lang keine Telemetriedaten sendet. Die Abfrage verwendet die Telemetriedaten aus dem Event Hub als Eingabe. Der Auftrag sendet die Abfrageergebnisse an die Funktions-App. In diesem Abschnitt konfigurieren Sie den Stream Analytics-Auftrag:

1. Navigieren Sie im Azure-Portal zu Ihrem Stream Analytics-Auftrag. Wählen Sie unter **Auftragstopologie** den Eintrag **Eingaben**, anschließend **+ Datenstromeingabe hinzufügen** und dann **Event Hub** aus.
1. Verwenden Sie die Informationen in der folgenden Tabelle, um die Eingabe mithilfe des zuvor erstellten Event Hubs zu konfigurieren, und klicken Sie dann auf **Speichern**:

    | Einstellung | Wert |
    | ------- | ----- |
    | Eingabealias | centraltelemetry |
    | Subscription | Ihr Abonnement |
    | Event Hub-Namespace | Ihr Event Hub-Namespace |
    | Event Hub-Name | Vorhandenen Wert verwenden: **centralexport** |

1. Wählen Sie unter **Auftragstopologie** den Eintrag **Ausgaben** anschließend **+ Hinzufügen** und dann **Azure-Funktion** aus.
1. Verwenden Sie die Informationen in der folgenden Tabelle, um die Ausgabe zu konfigurieren, und klicken Sie dann auf **Speichern**:

    | Einstellung | value |
    | ------- | ----- |
    | Ausgabealias | emailnotification |
    | Subscription | Ihr Abonnement |
    | Funktionen-App | Ihre Funktions-App |
    | Funktion  | HttpTrigger1 |

1. Wählen Sie unter **Auftragstopologie** die Option **Abfrage** aus, und ersetzen Sie die vorhandene Abfrage durch folgende SQL-Abfrage:

    ```sql
    with
    LeftSide as
    (
        SELECT
        -- Get the device ID from the message metadata and create a column
        GetMetadataPropertyValue([centraltelemetry], '[EventHub].[IoTConnectionDeviceId]') as deviceid1, 
        EventEnqueuedUtcTime AS time1
        FROM
        -- Use the event enqueued time for time-based operations
        [centraltelemetry] TIMESTAMP BY EventEnqueuedUtcTime
    ),
    RightSide as
    (
        SELECT
        -- Get the device ID from the message metadata and create a column
        GetMetadataPropertyValue([centraltelemetry], '[EventHub].[IoTConnectionDeviceId]') as deviceid2, 
        EventEnqueuedUtcTime AS time2
        FROM
        -- Use the event enqueued time for time-based operations
        [centraltelemetry] TIMESTAMP BY EventEnqueuedUtcTime
    )

    SELECT
        LeftSide.deviceid1 as deviceid,
        LeftSide.time1 as time
    INTO
        [emailnotification]
    FROM
        LeftSide
        LEFT OUTER JOIN
        RightSide 
        ON
        LeftSide.deviceid1=RightSide.deviceid2 AND DATEDIFF(second,LeftSide,RightSide) BETWEEN 1 AND 120
        where
        -- Find records where a device didn't send a message 120 seconds
        RightSide.deviceid2 is NULL
    ```

1. Wählen Sie **Speichern** aus.
1. Um den Stream Analytics-Auftrag zu starten, klicken Sie nacheinander auf **, Übersicht**, **Start**, **Jetzt** und **Start**:

    :::image type="content" source="media/howto-create-custom-rules/stream-analytics.png" alt-text="Screenshot von Stream Analytics":::

## <a name="configure-export-in-iot-central"></a>Konfigurieren des Exports nach IoT Central 

Navigieren Sie auf der Website des [Azure IoT Central-Anwendungs-Managers](https://aka.ms/iotcentral) zu der erstellten IoT Central-Anwendung.

In diesem Abschnitt konfigurieren Sie die Anwendung so, dass die Telemetriedaten aus den simulierten Geräten an Ihren Event Hub gestreamt werden. So konfigurieren Sie den Export:

1. Navigieren Sie zur Seite **Datenexport**, wählen Sie **+ Neu** und dann **Azure Event Hubs** aus.
1. Verwenden Sie die folgenden Einstellungen, um den Export zu konfigurieren, und klicken Sie dann auf **Speichern**: 

    | Einstellung | Wert |
    | ------- | ----- |
    | Anzeigename | Exportieren nach Event Hubs |
    | Enabled | Andererseits |
    | Typ der zu exportierenden Daten | Telemetrie |
    | Anreicherungen | Geben Sie das gewünschte Schlüssel-Wert-Paar ein, nach dem die exportierten Daten organisiert werden sollen. | 
    | Destination | Erstellen Sie ein neues Ziel, und geben Sie Informationen zum Speicherort der Daten ein. |

    :::image type="content" source="media/howto-create-custom-rules/cde-configuration.png" alt-text="Screenshot des Datenexports":::

Warten Sie, bis der Exportstatus **Wird ausgeführt** lautet, bevor Sie fortfahren.

## <a name="test"></a>Test

Um die Lösung zu testen, können Sie den kontinuierlichen Datenexport aus IoT Central deaktivieren, um angehaltene Geräte zu simulieren:

1. Navigieren Sie in Ihrer IoT Central-Anwendung zur Seite **Datenexport**, und wählen Sie die Exportkonfiguration **In Event Hubs exportieren** aus.
1. Legen Sie **Aktiviert** auf **Aus** fest, und klicken Sie auf **Speichern**.
1. Nach Ablauf von mindestens zwei Minuten erhält die als **Empfänger** angegebene E-Mail-Adresse mindestens eine E-Mail, deren Inhalt dem folgenden Beispiel ähnelt:

    ```txt
    The following device(s) have stopped sending telemetry:

    Device ID         Time
    Thermostat-Zone1  2019-11-01T12:45:14.686Z
    ```

## <a name="tidy-up"></a>Aufräumen

Um nach dieser Vorgehensweise aufzuräumen und unnötige Kosten zu vermeiden, löschen Sie die Ressourcengruppe **DetectStoppedDevices** im Azure-Portal.

Sie können die IoT Central-Anwendung in der Anwendung selbst auf der Seite **Verwaltung** löschen.

## <a name="next-steps"></a>Nächste Schritte

In dieser Schrittanleitung wurde Folgendes vermittelt:

* Streamen von Telemetriedaten aus einer IoT Central-Anwendung mithilfe des *kontinuierlichen Datenexports*
* Erstellen einer Stream Analytics-Abfrage, die erkennt, wenn ein Gerät keine Daten mehr sendet
* Senden einer E-Mail-Benachrichtigung mithilfe der Dienste Azure Functions und SendGrid

Sie wissen jetzt, wie Sie benutzerdefinierte Regeln und Benachrichtigungen erstellen können. Als Nächstes sollten Sie sich über das [Erweitern von Azure IoT Central mit benutzerdefinierten Analysen](howto-create-custom-analytics.md) informieren.
