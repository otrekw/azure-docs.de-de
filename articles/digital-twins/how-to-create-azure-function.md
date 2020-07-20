---
title: Einrichten einer Azure-Funktion zum Verarbeiten von Daten
titleSuffix: Azure Digital Twins
description: Erfahren Sie, wie Sie eine Azure-Funktion erstellen, die auf digitale Zwillinge zugreifen und von diesen ausgelöst werden kann.
author: cschormann
ms.author: cschorm
ms.date: 3/17/2020
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: 3b416e6ccb035ede06a360c2697a9b20ca417d98
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "84725901"
---
# <a name="connect-azure-functions-apps-for-processing-data"></a>Verbinden von Azure Functions-Apps für die Verarbeitung von Daten

Während der Vorschau wird die Aktualisierung von digitalen Zwillingen auf der Grundlage von Daten unter Verwendung von [**Ereignisrouten**](concepts-route-events.md) über Computeressourcen, z. B. [Azure Functions](../azure-functions/functions-overview.md), behandelt. Eine Azure-Funktion könnte verwendet werden, um als Reaktion darauf einen digitalen Zwilling zu aktualisieren:
* Von IoT Hub stammende Gerätetelemetriedaten
* Änderung von Eigenschaften oder anderen Daten, die von einem anderen digitalen Zwilling innerhalb des Zwillingsgraphen stammen

Dieser Artikel führt Sie durch die Erstellung einer Azure-Funktion zur Verwendung mit Azure Digital Twins. 

Hier finden Sie eine Übersicht über die darin enthaltenen Schritte:

1. Erstellen einer Azure Functions-App in Visual Studio
2. Schreiben einer Azure-Funktion mit einem [Event Grid](../event-grid/overview.md)-Trigger
3. Hinzufügen von Authentifizierungscode zur Funktion (für den Zugriff auf Azure Digital Twins)
4. Veröffentlichen der Funktions-App in Azure
5. Einrichten von [Sicherheitszugriff](concepts-security.md) Damit die Azure-Funktion zur Laufzeit ein Zugriffstoken für den Zugriff auf den Dienst erhält, müssen Sie die verwaltete Dienstidentität für die App der Funktion konfigurieren.

Im Rest dieses Artikels werden die Schritte zur Einrichtung der Azure-Funktion Schritt für Schritt durchlaufen.

## <a name="create-an-azure-functions-app-in-visual-studio"></a>Erstellen einer Azure Functions-App in Visual Studio

Wählen Sie in Visual Studio 2019 *Datei > Neues Projekt* aus. Suchen Sie die Vorlage *Azure Functions*, wählen Sie sie aus, und drücken Sie auf „Weiter“.

:::image type="content" source="media/how-to-create-azure-function/visual-studio-new-project.png" alt-text="Visual Studio: Dialogfeld „Neues Projekt“":::

Geben Sie einen Namen für die Funktions-App an, und drücken Sie auf „Erstellen“.

:::image type="content" source="media/how-to-create-azure-function/visual-studio-project-config.png" alt-text="Visual Studio: Dialogfeld „Projekt konfigurieren“":::

Wählen Sie den *Event Grid-Auslöser* aus, und drücken Sie auf „Erstellen“.

:::image type="content" source="media/how-to-create-azure-function/visual-studio-project-trigger.png" alt-text="Visual Studio: Dialogfeld für den Projekttrigger der Azure-Funktion":::

## <a name="write-an-azure-function-with-an-event-grid-trigger"></a>Schreiben einer Azure-Funktion mit einem Event Grid-Trigger

Mit dem folgenden Code wird eine kurze Azure-Funktion erstellt, die Sie zum Protokollieren von Ereignissen verwenden können: 

```csharp
// Default URL for triggering Event Grid function in the local environment
// http://localhost:7071/runtime/webhooks/EventGrid?functionName={functionname}
using System;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Host;
using Microsoft.Azure.EventGrid.Models;
using Microsoft.Azure.WebJobs.Extensions.EventGrid;
using Microsoft.Extensions.Logging;

namespace FunctionSample
{
    public static class FooFunction    {
        [FunctionName("Foo")]
        public static void Run([EventGridTrigger]EventGridEvent eventGridEvent, ILogger log)
        {
            log.LogInformation(eventGridEvent.Data.ToString());
        }
    }
}
```

Dies ist Ihre grundlegende Azure-Funktion.

### <a name="run-and-debug-the-azure-function-app"></a>Ausführen und Debuggen der Azure-Funktions-App

Sie können die Funktion jetzt kompilieren und ausführen. Obwohl Azure-Funktionen letztendlich in der Cloud ausgeführt werden sollen, können Sie Azure-Funktionen auch lokal ausführen und debuggen.

Weitere Informationen hierzu finden Sie unter [Lokales Debuggen des Event Grid-Triggers](../azure-functions/functions-debug-event-grid-trigger-local.md).

### <a name="add-the-azure-digital-twins-sdk-to-your-azure-function-app"></a>Hinzufügen des Azure Digital Twins-SDKs zu Ihrer Azure-Funktions-App

Die Funktions-App interagiert mit Azure Digital Twins unter Verwendung der [Azure IoT Digital Twin-Clientbibliothek für .NET (C#)](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/digitaltwins/Azure.DigitalTwins.Core). Wenn Sie das SDK verwenden möchten, müssen Sie die folgenden Pakete in Ihr Projekt einbinden:
* `Azure.DigitalTwins.Core` (Version `1.0.0-preview.2`)
* `Azure.Identity`

Damit die Konfiguration der Azure SDK-Pipeline für Azure Functions ordnungsgemäß eingerichtet werden kann, benötigen Sie auch Folgendes:
* `Azure.Net.Http`
* `Azure.Core`

Je nachdem, welche Tools Sie verwenden, können Sie hierzu den Visual Studio-Paket-Manager oder das Befehlszeilentool `dotnet` verwenden. 

Fügen Sie Ihrer Azure-Funktion folgende using-Anweisungen hinzu:

```csharp
using Azure.Identity;
using Azure.DigitalTwins.Core;
using System.Net.Http;
using Azure.Core.Pipeline;
```

## <a name="add-authentication-code-to-the-azure-function"></a>Hinzufügen von Authentifizierungscode zur Azure-Funktion

Fügen Sie als nächstes einen Authentifizierungscode hinzu, der der Funktion den Zugriff auf Azure Digital Twins ermöglicht.

Fügen Sie für diese Werte Variablen zu Ihrer Funktionsklasse hinzu: 
* Die Azure Digital Twins-App-ID
* Die URL Ihrer Azure Digital Twins-Instanz Es ist eine bewährte Methode, die Dienst-URL aus einer Umgebungsvariablen zu lesen, anstatt für diese die Hartcodierung in der Funktion zu verwenden.
* Eine statische Variable zum Aufnehmen einer HttpClient-Instanz Die Erstellung von HttpClient ist relativ aufwendig, und wir möchten vermeiden, dies für jeden Funktionsaufruf durchführen zu müssen.

Fügen Sie auch eine lokale Variable innerhalb Ihrer Funktion hinzu, um Ihre Azure Digital Twins-Clientinstanz für das Funktionsprojekt aufzunehmen. Verwenden Sie hierfür *keine* statische Variable innerhalb Ihrer Klasse.

Ändern Sie schließlich die Funktionssignatur so, dass sie asynchron ist.

Nach diesen Änderungen sollte Ihr Funktionscode dem folgenden ähneln:

```csharp
namespace FunctionSample
{
    public static class FooFunction
    {
        const string adtAppId = "https://digitaltwins.azure.net";
        private static string adtInstanceUrl = Environment.GetEnvironmentVariable("ADT_SERVICE_URL");
        private static HttpClient httpClient = new HttpClient();

        [FunctionName("Foo")]
        public static async Task Run([EventGridTrigger]EventGridEvent eventGridEvent, ILogger log)
        {
            DigitalTwinsClient client = null;
            try
            {
                ManagedIdentityCredential cred = new ManagedIdentityCredential(adtAppId);
                DigitalTwinsClientOptions opts = 
                    new DigitalTwinsClientOptions { Transport = new HttpClientTransport(httpClient) });
                client = new DigitalTwinsClient(new Uri(adtInstanceUrl), cred, opts);
                                                
                log.LogInformation($"ADT service client connection created.");
            }
            catch (Exception e)
            {
                log.LogError($"ADT service client connection failed. " + e.ToString());
                return;
            }
            log.LogInformation(eventGridEvent.Data.ToString());
        }
    }
}
```

Damit Ihre Funktions-App auf Azure Digital Twins zugreifen kann, muss sie über eine vom System verwaltete Identität und Zugriffsrechte auf Ihre Azure Digital Twins-Instanz verfügen.

Verwenden Sie den folgenden Befehl, um die vom System verwaltete Identität zu erstellen. Beachten Sie in der Ausgabe das Feld *principalId*.

```azurecli
az functionapp identity assign -g <your-resource-group> -n <your-App-Service-(function-app)-name>
```

Verwenden Sie den Wert *principalId* im folgenden Befehl, um die Identität der Funktions-App der Rolle *Besitzer* für Ihre Azure Digital Twins-Instanz zuzuweisen:

```azurecli
az dt role-assignment create --dt-name <your-Azure-Digital-Twins-instance> --assignee "<principal-ID>" --role "Azure Digital Twins Owner (Preview)"
```

Weitere Informationen zur verwalteten Identität finden Sie unter [Verwenden verwalteter Identitäten für App Service und Azure Functions](../app-service/overview-managed-identity.md).

Zuletzt können Sie die URL Ihrer Azure Digital Twins-Instanz für Ihre Funktion zugänglich gestalten, indem Sie eine Umgebungsvariable festlegen. Weitere Informationen hierzu finden Sie unter [Umgebungsvariablen](https://docs.microsoft.com/sandbox/functions-recipes/environment-variables).

> [!TIP]
> Die URL der Azure Digital Twins-Instanz wird durch Hinzufügen von *https://* an den Anfang des *Hostnamens* der Azure Digital Twins-Instanz erstellt. Um den Hostnamen mit allen Eigenschaften der Instanz anzuzeigen, können Sie `az dt show --dt-name <your-Azure-Digital-Twins-instance>` ausführen.

```azurecli
az functionapp config appsettings set -g <your-resource-group> -n <your-App-Service-(function-app)-name> --settings "ADT_SERVICE_URL=https://<your-Azure-Digital-Twins-instance-URL>"
```

## <a name="publish-the-azure-function-app"></a>Veröffentlichen der Azure-Funktions-App

Um die Funktions-App in Azure zu veröffentlichen, wählen Sie das Funktionsprojekt (nicht die Projektmappe) im Projektmappen-Explorer mit der rechten Maustaste aus, und wählen Sie dann *Publish()* aus.

Die folgende Registerkarte wird angezeigt:

:::image type="content" source="media/how-to-create-azure-function/visual-studio-publish-1.png" alt-text="Visual Studio: Dialogfeld für die Funktion „Publish“, Seite 1":::

Wählen oder erstellen Sie einen App Service-Plan zur Verwendung mit Azure Functions. Wenn Sie sich nicht sicher sind, verwenden Sie zunächst den Standardverbrauchsplan.

> [!IMPORTANT] 
> Für die Veröffentlichung einer Azure-Funktion fallen zusätzliche Gebühren für Ihr Abonnement an, unabhängig von Azure Digital Twins.

:::image type="content" source="media/how-to-create-azure-function/visual-studio-publish-2.png" alt-text="Visual Studio: Dialogfeld für die Funktion „Publish“, Seite 2":::

Geben Sie auf der folgenden Seite den gewünschten Namen für die neue App der Funktion, eine Ressourcengruppe und andere Details ein.

## <a name="set-up-security-access-for-the-azure-function-app"></a>Einrichten des Sicherheitszugriffs für die Azure-Funktions-App

Das Azure-Funktionsgerüst aus früheren Beispielen erfordert, dass ihm ein Bearertoken übergeben wird, um sich bei Azure Digital Twins authentifizieren zu können. Um sicherzustellen, dass dieses Bearertoken übergeben wird, müssen Sie die [verwaltete Dienstidentität (MSI)](../active-directory/managed-identities-azure-resources/overview.md) für die Funktions-App einrichten. Dies muss nur einmal für jede Funktions-App durchgeführt werden.

Wechseln Sie zum [Azure-Portal](https://portal.azure.com/), und navigieren Sie zu Ihrer Funktions-App, um dies einzurichten.

Wählen Sie auf der Registerkarte *Plattformfeatures* die Option *Identität* aus:

:::image type="content" source="media/how-to-create-azure-function/visual-studio-msi-1.png" alt-text="Azure-Portal: Auswählen der Identität für eine Azure-Funktion":::

Legen Sie auf der Seite „Identität“ den Umschalter für *Status* auf *Ein* fest. 

:::image type="content" source="media/how-to-create-azure-function/visual-studio-msi-2.png" alt-text="Azure-Portal: Aktivieren des Identitätsstatus":::

Beachten Sie auch die auf dieser Seite gezeigte **Objekt-ID**, da sie im nächsten Abschnitt verwendet wird.

### <a name="assign-access-roles"></a>Zuweisen von Zugriffsrollen

Da in Azure Digital Twins die rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC) verwendet wird (weitere Informationen hierzu finden Sie unter [Konzepte: Sicherheit für Azure Digital Twins-Lösungen](concepts-security.md)), müssen Sie auch eine Rolle für jede Funktions-App hinzufügen, der Sie den Zugriff auf Azure Digital Twins gestatten möchten.

Sie benötigen für die Zuweisung einer Rolle die **Ressourcen-ID** der von Ihnen erstellten Azure Digital Twins-Instanz. Wenn Sie sie bei der Erstellung Ihrer Instanz nicht bereits früher aufgezeichnet haben, können Sie sie mit diesem Befehl abrufen:

```bash
az dt show --name <your-instance-name> -g <your-resource-group-name>
```
Die Ressourcen-ID wird Teil der Ausgabe sein, als lange Zeichenfolge namens „id“, die mit den Buchstaben „/subscriptions/...“ beginnt.

Verwenden Sie die Ressource-ID zusammen mit der Objekt-ID der vorherigen Azure-Funktion im nachfolgenden Befehl:

```azurecli
az role assignment create --role "Azure Digital Twins Owner (Preview)" --assignee <object-ID> --scope <resource-ID>
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie die Schritte zum Einrichten einer Azure-Funktion für die Verwendung mit Azure Digital Twins befolgt. Als nächstes können Sie Ihre Azure-Funktion bei Event Grid abonnieren, um an einem Endpunkt zu lauschen. Für diesen Endpunkt könnte Folgendes gelten:
* Ein Event Grid-Endpunkt, der an Azure Digital Twins angefügt ist, um Nachrichten zu verarbeiten, die von Azure Digital Twins selbst stammen (z. B. Nachrichten zur Änderung von Eigenschaften, Telemetrienachrichten, die von [digitalen Zwillingen](concepts-twins-graph.md) im Zwillingsgraphen erzeugt werden, oder Lebenszyklusnachrichten).
* Die von IoT Hub zum Senden von Telemetrie- und anderen Geräteereignissen verwendeten IoT-Systemthemen.
* Ein Event Grid-Endpunkt, der Nachrichten von anderen Diensten empfängt.

Als nächstes erfahren Sie, wie Sie auf Ihrer grundlegenden Azure-Funktion aufbauen können, um IoT Hub-Daten in Azure Digital Twins zu erfassen:
* [Gewusst wie: Erfassen von Telemetriedaten aus IoT Hub](how-to-ingest-iot-hub-data.md)