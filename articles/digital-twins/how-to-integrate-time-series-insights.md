---
title: Integration in Azure Time Series Insights
titleSuffix: Azure Digital Twins
description: Erfahren Sie, wie Sie Ereignisrouten von Azure Digital Twins in Azure Time Series Insights einrichten.
author: alexkarcher-msft
ms.author: alkarche
ms.date: 4/7/2021
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: 1ac9a0bc83c2a33becb5348862e5a572dddf3ac2
ms.sourcegitcommit: 4a54c268400b4158b78bb1d37235b79409cb5816
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2021
ms.locfileid: "108133923"
---
# <a name="integrate-azure-digital-twins-with-azure-time-series-insights"></a>Integrieren von Azure Digital Twins in Azure Time Series Insights

In diesem Artikel erfahren Sie, wie Sie Azure Digital Twins in [Azure Time Series Insights (TSI)](../time-series-insights/overview-what-is-tsi.md) integrieren.

Mithilfe der in diesem Artikel beschriebenen Lösung können Sie Verlaufsdaten zu ihrer IoT-Lösung erfassen und analysieren. Azure Digital Twins eignet sich hervorragend dazu, Daten in Time Series Insights einzuspeisen, da Sie mehrere Datenströme zu korrelieren und Ihre Informationen standardisieren können, bevor sie an Time Series Insights gesendet werden.

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie eine Beziehung mit Time Series Insights einrichten können, müssen Sie die folgenden Ressourcen einrichten:
* Einen **IoT-Hub**. Anweisungen hierzu finden Sie unter *Schnellstart: Senden von Telemetriedaten von einem Gerät an einen IoT-Hub und Durchführen der Überwachung per Azure CLI* im Abschnitt [*Erstellen eines IoT Hubs*](../iot-hub/quickstart-send-telemetry-cli.md#create-an-iot-hub).
* Eine **Azure Digital Twins-Instanz**.
Anweisungen dazu finden Sie unter [*Gewusst wie: Einrichten einer Azure Digital Twins-Instanz und der Authentifizierung*](./how-to-set-up-instance-portal.md).
* Ein **Modell und einen Zwilling in der Azure Digital Twins-Instanz**.
Sie müssen die Informationen des Zwillings einige Male aktualisieren, damit die Daten in Time Series Insights nachverfolgt werden können. Anweisungen finden Sie im Artikel *Erfassen von IoT Hub Telemetriedaten in Azure Digital Twins* im Abschnitt [*Hinzufügen eines Modells und eines Zwillings*](how-to-ingest-iot-hub-data.md#add-a-model-and-twin).

> [!TIP]
> In diesem Artikel werden die geänderten Werte für digitale Zwillinge, die in Time Series Insights angezeigt werden, der Einfachheit halber manuell aktualisiert. Wenn Sie diesen Artikel jedoch mit simulierten Livedaten durcharbeiten möchten, können Sie eine Azure-Funktion einrichten, die digitale Zwillinge basierend auf IoT-Telemetrieereignissen von einem simulierten Gerät aktualisiert. Anweisungen hierzu finden Sie unter [*Erfassen von IoT Hub-Telemetriedaten in Azure Digital Twins*](how-to-ingest-iot-hub-data.md). In diesem Artikel werden u. a. die letzten Schritte zum Ausführen des Gerätesimulators und zum Überprüfen der ordnungsgemäßen Funktion des Datenflusses erläutert.
>
> Suchen Sie später nach einem weiteren TIPP, um zu sehen, wo Sie mit der Ausführung des Gerätesimulators beginnen, und lassen Sie ihre Azure-Funktionen die Zwillinge automatisch aktualisieren, anstatt manuelle Befehle zum Aktualisieren digitaler Zwillinge zu senden.


## <a name="solution-architecture"></a>Lösungsarchitektur

Sie fügen Time Series Insights über den folgenden Pfad an Azure Digital Twins an.

:::row:::
    :::column:::
        :::image type="content" source="media/how-to-integrate-time-series-insights/diagram-simple.png" alt-text="Diagramm: Azure-Dienste in einem End-to-End-Szenario, wobei Time Series Insights hervorgehoben ist" lightbox="media/how-to-integrate-time-series-insights/diagram-simple.png":::
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::

## <a name="create-event-hub-namespace"></a>Erstellen eines Event Hub-Namespace

Bevor Sie die Event Hubs erstellen, erstellen Sie zuerst einen Event Hub-Namespace, der Ereignisse von Ihrer Azure Digital Twins-Instanz empfängt. Sie können entweder die folgenden Azure CLI Anweisungen oder das Azure-Portal verwenden: [*Schnellstart: Erstellen eines Event Hubs mithilfe des Azure-Portals*](../event-hubs/event-hubs-create.md). Informationen zu Regionen, die Event Hubs unterstützen, finden Sie unter [*Verfügbare Produkte nach Region*](https://azure.microsoft.com/global-infrastructure/services/?products=event-hubs).

```azurecli-interactive
az eventhubs namespace create --name <name-for-your-event-hubs-namespace> --resource-group <your-resource-group> -l <region>
```

> [!TIP]
> Wenn Sie eine Fehlermeldung mit dem Hinweis `BadRequest: The specified service namespace is invalid.` erhalten, stellen Sie sicher, dass der für Ihren Namespace ausgewählte Name die in diesem Referenzdokument beschriebenen Benennungsanforderungen erfüllt: [Erstellen eines Namespace](/rest/api/servicebus/create-namespace).

Sie verwenden diesen Event Hubs-Namespace, um die beiden für diesen Artikel erforderlichen Event Hubs zu speichern:

  1. **Zwillingshub:** Event Hub zum Empfangen von Änderungsereignissen für Zwillinge
  2. **Zeitreihenhub:** Event Hub zum Streamen von Ereignissen an Time Series Insights

In den nächsten Abschnitten erfahren Sie Schritt für Schritt, wie Sie diese Hubs in Ihrem Event Hub-Namespace erstellen und konfigurieren.

## <a name="create-twins-hub"></a>Erstellen eines Zwillingshubs

Der erste Event Hub, den Sie in diesem Artikel erstellen, ist der **Zwillingshub**. Dieser Event Hub empfängt Zwillingsänderungsereignisse von Azure Digital Twins.
Führen Sie zum Einrichten des Zwillingshubs die folgenden Schritte in diesem Abschnitt aus:

1. Erstellen des Zwillingshubs
2. Erstellen einer Autorisierungsregel zum Steuern der Berechtigungen für den Hub
3. Erstellen eines Endpunkts in Azure Digital Twins, der die Autorisierungsregel für den Zugriff auf den Hub verwendet
4. Erstellen einer Route in Azure Digital Twins, die das Zwillingsaktualisierungsereignis an den Endpunkt und den Hub für verbundene Zwillinge sendet
5. Abrufen der Verbindungszeichenfolge für den Zwillingshub

Erstellen Sie den **Zwillingshub** mit dem folgenden CLI-Befehl. Geben Sie einen Namen für Ihren Zwillingshub an.

```azurecli-interactive
az eventhubs eventhub create --name <name-for-your-twins-hub> --resource-group <your-resource-group> --namespace-name <your-event-hubs-namespace-from-above>
```

### <a name="create-twins-hub-authorization-rule"></a>Erstellen einer Autorisierungsregel für den Zwillingshub

Erstellen Sie eine [Autorisierungsregel](/cli/azure/eventhubs/eventhub/authorization-rule#az_eventhubs_eventhub_authorization_rule_create) mit Sende- und Empfangsberechtigungen. Geben Sie einen Namen für die Regel an.

```azurecli-interactive
az eventhubs eventhub authorization-rule create --rights Listen Send --name <name-for-your-twins-hub-auth-rule> --resource-group <your-resource-group> --namespace-name <your-event-hubs-namespace-from-earlier> --eventhub-name <your-twins-hub-from-above>
```

### <a name="create-twins-hub-endpoint"></a>Erstellen eines Endpunkts für den Zwillingshub

Erstellen Sie einen Azure Digital Twins-[Endpunkt](concepts-route-events.md#create-an-endpoint), der Ihren Event Hub mit Ihrer Azure Digital Twins-Instanz verknüpft. Geben Sie einen Namen für den Endpunkt Ihres Zwillingshubs an.

```azurecli-interactive
az dt endpoint create eventhub -n <your-Azure-Digital-Twins-instance-name> --eventhub-resource-group <your-resource-group> --eventhub-namespace <your-event-hubs-namespace-from-earlier> --eventhub <your-twins-hub-name-from-above> --eventhub-policy <your-twins-hub-auth-rule-from-earlier> --endpoint-name <name-for-your-twins-hub-endpoint>
```

### <a name="create-twins-hub-event-route"></a>Erstellen einer Ereignisroute für den Zwillingshub

Azure Digital Twins-Instanzen können [Ereignisse zur Aktualisierung von Zwillingen](./concepts-event-notifications.md) ausgeben, wenn der Zustand eines Zwillings aktualisiert wird. In diesem Abschnitt erstellen Sie eine Azure Digital Twins-**Ereignisroute**, die diese Aktualisierungsereignisse zur weiteren Verarbeitung an den Zwillingshub weiterleitet.

Erstellen Sie eine [Route](concepts-route-events.md#create-an-event-route) in Azure Digital Twins, um Ereignisse zur Aktualisierung von Zwillingen an Ihren obigen Endpunkt zu senden. Der Filter in dieser Route erlaubt nur das Weitergeben von Nachrichten über Zwillingsaktualisierungen an Ihren Endpunkt. Geben Sie einen Namen für die Zwillingshub-Ereignisroute an.

```azurecli-interactive
az dt route create -n <your-Azure-Digital-Twins-instance-name> --endpoint-name <your-twins-hub-endpoint-from-above> --route-name <name-for-your-twins-hub-event-route> --filter "type = 'Microsoft.DigitalTwins.Twin.Update'"
```

### <a name="get-twins-hub-connection-string"></a>Abrufen der Verbindungszeichenfolge für den Zwillingshub

Rufen Sie die [Event Hubs-Verbindungszeichenfolge der Zwillinge](../event-hubs/event-hubs-get-connection-string.md) mithilfe der Autorisierungsregeln ab, die Sie zuvor für den Zwillingshub erstellt haben.

```azurecli-interactive
az eventhubs eventhub authorization-rule keys list --resource-group <your-resource-group> --namespace-name <your-event-hubs-namespace-from-earlier> --eventhub-name <your-twins-hub-from-above> --name <your-twins-hub-auth-rule-from-earlier>
```
Notieren Sie sich den Wert **primaryConnectionString** aus dem Ergebnis, um später in diesem Artikel die App-Einstellung für den Zwillingshub zu konfigurieren.

## <a name="create-time-series-hub"></a>Erstellen eines Zeitreihenhubs

Der zweite Event Hub, den Sie in diesem Artikel erstellen, ist der **Zeitreihenhub**. Dies ist ein Event Hub, der die Azure Digital Twins-Ereignisse an Time Series Insights streamt.
Führen Sie zum Einrichten des Zeitreihenhubs die folgenden Schritte aus:

1. Erstellen des Zeitreihenhubs
2. Erstellen einer Autorisierungsregel zum Steuern der Berechtigungen für den Hub
3. Abrufen der Verbindungszeichenfolge für den Zeitreihenhub

Wenn Sie später die Time Series Insights-Instanz erstellen, verbinden Sie diesen Zeitreihenhub als Ereignisquelle für die Time Series Insights-Instanz.

Erstellen Sie den **Zeitreihenhub** mit dem folgenden Befehl. Geben Sie einen Namen für den Zeitreihenhub an.

```azurecli-interactive
 az eventhubs eventhub create --name <name-for-your-time-series-hub> --resource-group <your-resource-group> --namespace-name <your-event-hub-namespace-from-earlier>
```

### <a name="create-time-series-hub-authorization-rule"></a>Erstellen einer Autorisierungsregel für den Zeitreihenhub

Erstellen Sie eine [Autorisierungsregel](/cli/azure/eventhubs/eventhub/authorization-rule#az_eventhubs_eventhub_authorization_rule_create) mit Sende- und Empfangsberechtigungen. Geben Sie einen Namen für die Autorisierungsregel für den Zeitreihenhub an.

```azurecli-interactive
az eventhubs eventhub authorization-rule create --rights Listen Send --name <name-for-your-time-series-hub-auth-rule> --resource-group <your-resource-group> --namespace-name <your-event-hub-namespace-from-earlier> --eventhub-name <your-time-series-hub-name-from-above>
```

### <a name="get-time-series-hub-connection-string"></a>Abrufen der Verbindungszeichenfolge für den Zeitreihenhub

Rufen Sie die [Verbindungszeichenfolge für den Zeitreihenhub](../event-hubs/event-hubs-get-connection-string.md) mithilfe der Autorisierungsregeln ab, die Sie zuvor für den Zeitreihenhub erstellt haben:

```azurecli-interactive
az eventhubs eventhub authorization-rule keys list --resource-group <your-resource-group> --namespace-name <your-event-hub-namespace-from-earlier> --eventhub-name <your-time-series-hub-name-from-earlier> --name <your-time-series-hub-auth-rule-from-earlier>
```
Notieren Sie sich den Wert **primaryConnectionString** aus dem Ergebnis, um später in diesem Artikel die App-Einstellung für den Zeitreihenhub zu konfigurieren.

Notieren Sie sich außerdem die folgenden Werte, um sie später zum Erstellen einer Time Series Insights-Instanz zu verwenden:
* Event Hub-Namespace
* Name des Zeitreihenhubs
* Zeitreihenhub-Authentifizierungsregel

## <a name="create-a-function"></a>Erstellen einer Funktion

In diesem Abschnitt erstellen Sie eine Azure-Funktion, die Ereignisse zur Aktualisierung von Zwillingen von ihrer ursprünglichen Form als JSON-Patchdokumente in JSON-Objekte konvertiert, die nur aktualisierte und hinzugefügte Werte von Ihren Zwillingen enthalten.

### <a name="step-1-create-function-app"></a>Schritt 1: Erstellen einer Funktions-App

Erstellen Sie zunächst in Visual Studio ein neues Funktions-App-Projekt. Anweisungen hierzu finden Sie im Abschnitt [**Erstellen einer Funktions-App in Visual Studio**](how-to-create-azure-function.md#create-a-function-app-in-visual-studio) im Artikel *Vorgehensweise: Einrichten einer Funktion zur Verarbeitung von Daten*.

### <a name="step-2-add-a-new-function"></a>Schritt 2: Hinzufügen einer neuen Funktion

Erstellen Sie eine neue Azure-Funktion namens *ProcessDTUpdatetoTSI.cs*, um Gerätetelemetrieereignisse auf die Time Series Insights-Instanz zu aktualisieren. Als Funktionstyp wird **Event Hub-Trigger** verwendet.

:::image type="content" source="media/how-to-integrate-time-series-insights/create-event-hub-trigger-function.png" alt-text="Screenshot: Visual Studio zum Erstellen einer neuen Azure-Funktion vom Typ „Event Hub-Trigger“":::

### <a name="step-3-fill-in-function-code"></a>Schritt 3: Angeben des Funktionscodes

Fügen Sie Ihrem Projekt dann die folgenden Pakete hinzu:
* [Microsoft.Azure.WebJobs](https://www.nuget.org/packages/Microsoft.Azure.WebJobs/)
* [Microsoft.Azure.WebJobs.Extensions.EventHubs](https://www.nuget.org/packages/Microsoft.Azure.WebJobs/)
* [Microsoft.NET.Sdk.Functions](https://www.nuget.org/packages/Microsoft.NET.Sdk.Functions/)

Ersetzen Sie den Code in der Datei *ProcessDTUpdatetoTSI.cs* durch den folgenden Code:

:::code language="csharp" source="~/digital-twins-docs-samples/sdks/csharp/updateTSI.cs":::

Speichern Sie Ihren Funktionscode.

### <a name="step-4-publish-the-function-app-to-azure"></a>Schritt 4: Veröffentlichen der Funktions-App in Azure

Veröffentlichen Sie das Projekt mit der Funktion *ProcessDTUpdatetoTSI.cs* in einer Funktions-App in Azure.

Anweisungen hierzu finden Sie im Abschnitt [**Veröffentlichen der Funktions-App in Azure**](how-to-create-azure-function.md#publish-the-function-app-to-azure) des Artikels *Vorgehensweise: Einrichten einer Funktion zur Verarbeitung von Daten*.

Speichern Sie den Namen der Funktions-App, um sie später zum Konfigurieren der App-Einstellungen für die beiden Event Hubs zu verwenden.

### <a name="step-5-security-access-for-the-function-app"></a>Schritt 5: Sicherheitszugriff für die Funktions-App

**Weisen Sie dann eine Zugriffsrolle** für die Funktion zu, und **Konfigurieren Sie die Anwendungseinstellungen**, damit sie auf Ihre Azure Digital Twins-Instanz zugreifen kann. Anweisungen hierzu finden Sie im Abschnitt [**Einrichten des Sicherheitszugriff für die Funktions-App**](how-to-create-azure-function.md#set-up-security-access-for-the-function-app) des Artikels *Vorgehensweise: Einrichten einer Funktion zur Verarbeitung von Daten*.

### <a name="step-6-configure-app-settings-for-the-two-event-hubs"></a>Schritt 6: Konfigurieren von App-Einstellungen für die beiden Event Hubs

Als Nächstes fügen Sie Umgebungsvariablen in den Einstellungen der Funktions-App hinzu, die ihr den Zugriff auf den Zwillingshub und den Zeitreihenhub ermöglichen.

Verwenden Sie den zuvor gespeicherten Zwillingshubwert für **primaryConnectionString**, um in Ihrer Funktions-App eine App-Einstellung zu erstellen, die die Zwillingshub-Verbindungszeichenfolge enthält:

```azurecli-interactive
az functionapp config appsettings set --settings "EventHubAppSetting-Twins=<your-twins-hub-primaryConnectionString>" -g <your-resource-group> -n <your-App-Service-(function-app)-name>
```

Verwenden Sie den zuvor gespeicherten Zeitreihenhubwert für **primaryConnectionString**, um in Ihrer Funktions-App eine App-Einstellung zu erstellen, die die Zeitreihenhub-Verbindungszeichenfolge enthält:

```azurecli-interactive
az functionapp config appsettings set --settings "EventHubAppSetting-TSI=<your-time-series-hub-primaryConnectionString>" -g <your-resource-group> -n <your-App-Service-(function-app)-name>
```

## <a name="create-and-connect-a-time-series-insights-instance"></a>Erstellen und Verbinden einer Time Series Insights-Instanz

In diesem Abschnitt richten Sie eine Time Series Insights-Instanz zum Empfangen von Daten von Ihrem Zeitreihenhub ein. Weitere Informationen zu diesem Prozess finden Sie unter [*Tutorial: Einrichten einer Azure Time Series Insights Gen2-Umgebung*](../time-series-insights/tutorial-set-up-environment.md). Führen Sie die folgenden Schritte aus, um eine Time Series Insights-Umgebung zu erstellen:

1. Suchen Sie im [Azure-Portal](https://portal.azure.com) nach *Time Series Insights-Umgebungen*, und wählen Sie die Schaltfläche **Hinzufügen** aus. Wählen Sie die folgenden Optionen aus, um die Zeitreihenumgebung zu erstellen:

    * **Abonnement**: Wählen Sie Ihr Abonnement aus.
        - **Ressourcengruppe**: Wählen Sie Ihre Ressourcengruppe aus.
    * **Umgebungsname**: Geben Sie einen Namen für Ihre Zeitreihenumgebung an.
    * **Standort**: Wählen Sie einen Standort aus.
    * **Tarif**: Wählen Sie den Tarif **Gen2(L1)** aus.
    * **Eigenschaftenname**: Geben Sie **$dtId** ein. (Weitere Informationen zur Auswahl eines ID-Werts finden Sie unter [*Bewährte Methoden für die Auswahl einer Time Series-ID*](../time-series-insights/how-to-select-tsid.md).)
    * **Speicherkontoname**: Geben Sie einen Speicherkontonamen an.
    * **Warm Storage aktivieren**: Lassen Sie dieses Feld auf *Ja* festgelegt.

    Sie können für andere Eigenschaften auf dieser Seite die Standardwerte übernehmen. Klicken Sie auf die Schaltfläche **Weiter: Ereignisquelle >** .

    :::image type="content" source="media/how-to-integrate-time-series-insights/create-time-series-insights-environment-1.png" alt-text="Screenshot: Erstellen einer Time Series Insights-Umgebung im Azure-Portal. Wählen Sie in den Dropdownlisten Ihr Abonnement, Ihre Ressourcengruppe und den Standort und anschließend einen Namen für Ihre Umgebung aus." lightbox="media/how-to-integrate-time-series-insights/create-time-series-insights-environment-1.png":::
        
    :::image type="content" source="media/how-to-integrate-time-series-insights/create-time-series-insights-environment-2.png" alt-text="Screenshot: Erstellen einer Time Series Insights-Umgebung im Azure-Portal. Der Tarif „Gen2(L1)“ ist ausgewählt, und der Eigenschaftenname der Zeitreihen-ID lautet „$dtId“." lightbox="media/how-to-integrate-time-series-insights/create-time-series-insights-environment-2.png":::

2. Wählen Sie auf der Registerkarte *Ereignisquelle* die folgenden Felder aus:

   * **Ereignisquelle erstellen?** : Wählen Sie *Ja* aus.
   * **Quelltyp**: Wählen Sie *Event Hub* aus.
   * **Name**: Geben Sie einen Namen für Ihre Ereignisquelle an.
   * **Abonnement**: Wählen Sie Ihr Azure-Abonnement aus.
   * **Event Hub-Namespace**: Wählen Sie den Namespace aus, den Sie weiter oben in diesem Artikel erstellt haben.
   * **Event Hub-Name**: Wählen Sie den Namen des **Zeitreihenhubs** aus, den Sie weiter oben in diesem Artikel erstellt haben.
   * **Name der Event Hub-Zugriffsrichtlinie**: Wählen Sie die *Zeitreihenhub-Authentifizierungsregel* aus, die Sie weiter oben in diesem Artikel erstellt haben.
   * **Event Hub-Consumergruppe**: Wählen Sie *Neu* aus, und geben Sie einen Namen für Ihre Event Hub-Consumergruppe an. Klicken Sie anschließend auf *Hinzufügen*.
   * **Eigenschaftenname**: Lassen Sie dieses Feld leer.
    
    Wählen Sie die Schaltfläche **Überprüfen + erstellen** aus, um alle Details zu überprüfen. Wählen Sie dann erneut die Schaltfläche **Überprüfen + erstellen** aus, um die Zeitreihenumgebung zu erstellen.

    :::image type="content" source="media/how-to-integrate-time-series-insights/create-tsi-environment-event-source.png" alt-text="Screenshot: Erstellen einer Time Series Insights-Umgebung im Azure-Portal. Sie erstellen eine Ereignisquelle mit den Event Hub-Informationen von oben. Außerdem erstellen Sie eine neue Consumergruppe." lightbox="media/how-to-integrate-time-series-insights/create-tsi-environment-event-source.png":::

## <a name="send-iot-data-to-azure-digital-twins"></a>Senden von IoT-Daten an Azure Digital Twins

Um mit dem Senden von Daten an Time Series Insights zu beginnen, müssen Sie zunächst die Eigenschaften des digitalen Zwillings in Azure Digital Twins mit sich ändernden Datenwerten aktualisieren.

Verwenden Sie den folgenden CLI-Befehl, um die *Temperature*-Eigenschaft des *Thermostat67*-Zwillings zu aktualisieren, den Sie Ihrer Instanz im Abschnitt [Voraussetzungen](#prerequisites) hinzugefügt haben.

```azurecli-interactive
az dt twin update -n <your-azure-digital-twins-instance-name> --twin-id thermostat67 --json-patch '{"op":"replace", "path":"/Temperature", "value": 20.5}'
```

**Wiederholen Sie den Befehl mindestens vier weitere Male mit unterschiedlichen Temperaturwerten**, um mehrere Datenpunkte zu erstellen, die später in der Time Series Insights-Umgebung beobachtet werden können.

> [!TIP]
> Wenn Sie diesen Artikel mit simulierten Livedaten abschließen möchten, anstatt die Werte des digitalen Zwillings manuell zu aktualisieren, stellen Sie zunächst sicher, dass Sie den Tipp aus dem Abschnitt [*Voraussetzungen*](#prerequisites) zum Einrichten einer Azure-Funktion abgeschlossen haben, die Zwillinge von einem simulierten Gerät aktualisiert.
Danach können Sie das Gerät nun ausführen, um mit dem Senden simulierter Daten und dem Aktualisieren Ihres digitalen Zwillings über diesen Datenfluss zu beginnen.

## <a name="visualize-your-data-in-time-series-insights"></a>Visualisieren Ihrer Daten in Time Series Insights

Nun sollten Daten in Ihre Time Series Insights-Instanz übertragen werden, die zur Analyse bereit sind. Führen Sie die folgenden Schritte aus, um die eingehenden Daten zu untersuchen.

1. Suchen Sie im [Azure-Portal](https://portal.azure.com) nach dem Namen Ihrer Zeitreihenumgebung, den Sie zuvor erstellt haben. Wählen Sie in den Menüoptionen auf der linken Seite *Übersicht* aus, um die *Time Series Insights-Explorer-URL* anzuzeigen. Wählen Sie die URL aus, um die Temperaturänderungen anzuzeigen, die sich in der Time Series Insights-Umgebung widerspiegeln.

    :::image type="content" source="media/how-to-integrate-time-series-insights/view-environment.png" alt-text="Screenshot: Auswählen der Time Series Insights-Explorer-URL im Azure-Portal auf der Übersichtsregisterkarte Ihrer Time Series Insights-Umgebung" lightbox="media/how-to-integrate-time-series-insights/view-environment.png":::

2. Im Explorer werden die Zwillinge in der Azure Digital Twins-Instanz auf der linken Seite angezeigt. Wählen Sie den Zwilling *thermostat67* und anschließend die Eigenschaft *Temperature* aus, und klicken Sie auf **Hinzufügen**.

    :::image type="content" source="media/how-to-integrate-time-series-insights/add-data.png" alt-text="Screenshot: Auswählen von „thermostat67“ im Time Series Insights-Explorer, Auswählen der Eigenschaftstemperatur und Klicken auf „Hinzufügen“" lightbox="media/how-to-integrate-time-series-insights/add-data.png":::

3. Sie sollten jetzt die anfänglichen Temperaturmesswerte Ihres Thermostats wie unten dargestellt sehen: 

    :::image type="content" source="media/how-to-integrate-time-series-insights/initial-data.png" alt-text="Screenshot: TSI-Explorer zum Anzeigen der anfänglichen Temperaturdaten. Es handelt sich um eine Zeile mit Zufallswerten zwischen 68 und 85." lightbox="media/how-to-integrate-time-series-insights/initial-data.png":::

Nach einer viel längeren Ausführung der Simulation sieht die Visualisierung etwa wie folgt aus:

:::image type="content" source="media/how-to-integrate-time-series-insights/day-data.png" alt-text="Screenshot: TSI-Explorer, in dem die Temperaturdaten für jeden Zwilling in drei parallelen Linien unterschiedlicher Farben dargestellt werden" lightbox="media/how-to-integrate-time-series-insights/day-data.png":::

## <a name="next-steps"></a>Nächste Schritte

Die digitalen Zwillinge werden standardmäßig als flache Hierarchie in Time Series Insights gespeichert, können jedoch für die Organisation mit Modellinformationen und einer Hierarchie mit mehreren Ebenen erweitert werden. Weitere Informationen zu diesem Vorgang finden Sie im 

* [*Tutorial: Definieren und Anwenden eines Modells*](../time-series-insights/tutorial-set-up-environment.md#define-and-apply-a-model) 

Sie können eine benutzerdefinierte Logik schreiben, um diese Informationen mithilfe der bereits in Azure Digital Twins gespeicherten Modell- und Diagrammdaten automatisch bereitzustellen. Weitere Informationen zum Verwalten, Aktualisieren und Abrufen von Informationen aus dem Zwillingsgraphen finden Sie in den folgenden Referenzen:

* [*Verwenden Verwalten digitaler Zwillinge*](./how-to-manage-twin.md)
* [*Verwenden Abfragen des Zwillingsgraphen*](./how-to-query-graph.md)