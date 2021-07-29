---
title: 'Schnellstart: Exportieren von Daten aus Azure IoT Central'
description: In diesem Schnellstart erfahren Sie, wie Sie das Datenexportfeature von IoT Central für die Integration mit anderen Clouddiensten verwenden.
author: dominicbetts
ms.author: dobett
ms.date: 05/27/2021
ms.topic: quickstart
ms.service: iot-central
services: iot-central
ms.custom: mvc
ms.openlocfilehash: 06e387e4742d86f4176d262ae2e2e9db863386ca
ms.sourcegitcommit: 91fdedcb190c0753180be8dc7db4b1d6da9854a1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/17/2021
ms.locfileid: "112300894"
---
# <a name="quickstart-export-data-from-an-iot-central-application"></a>Schnellstart: Exportieren von Daten aus einer IoT Central-Anwendung

In diesem Schnellstart erfahren Sie, wie Sie Daten mit einem fortlaufenden Export aus Ihrer Azure IoT Central-Anwendung in einen anderen Clouddienst exportieren. Für einen schnelleren Einstieg wird in diesem Schnellstart der [Azure Data Explorer](/azure/data-explorer/data-explorer-overview) verwendet, mit dem Sie Telemetriedaten von der Smartphone-App **IoT Plug and Play** speichern, abfragen und verarbeiten können.

In dieser Schnellstartanleitung führen Sie die folgenden Schritte aus:

- Sie verwenden das Datenexportfeature von IoT Central, um die von der Smartphone-App gesendeten Telemetriedaten in eine Azure Event Hubs-Warteschlange zu exportieren.
- Sie erfassen die Telemetriedaten aus der Event Hubs-Warteschlange in einer Azure Data Explorer-Datenbank.
- Sie verwenden Azure Data Explorer zum Ausführen von Abfragen für die Telemetriedaten.

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie beginnen, sollten Sie den ersten Schnellstart zum [Erstellen einer Azure IoT Central-Anwendung](./quick-deploy-iot-central.md) abgeschlossen haben. Der zweite Schnellstart zum [Konfigurieren von Regeln und Aktionen für Ihr Gerät](quick-configure-rules.md) ist optional.

## <a name="install-azure-services"></a>Installieren von Azure-Diensten

Bevor Sie Daten aus Ihrer IoT Central-Anwendung exportieren können, müssen Sie eine Event Hubs-Warteschlange und eine Azure Data Explorer-Datenbank erstellen.

Wählen Sie die folgende Schaltfläche aus, um die Dienste bereitzustellen:

[![Schaltfläche „In Azure bereitstellen“ für den fortlaufenden Datenexport](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure-Samples%2Fiot-central-docs-samples%2Fmaster%2Fquickstart-cde%2Fdeploy-azure-services.json)

Gehen Sie im Formular **Benutzerdefinierte Bereitstellung** wie folgt vor:

- Erstellen Sie eine neue Ressourcengruppe mit dem Namen *central-quickstart*.
- Wählen Sie die Ihnen am nächsten gelegene Region aus.
- Geben Sie einen eindeutigen Azure Data Explorer-Namen ein, der nur aus Kleinbuchstaben und Zahlen besteht. Beispiel: *contosocentraladx*.
- Geben Sie einen eindeutigen Azure Event Hubs-Namespace ein, der nur aus Kleinbuchstaben und Zahlen besteht. Beispiel; *contosocentraleh*.

Klicken Sie auf **Überprüfen + erstellen** und dann auf **Erstellen**.

> [!TIP]
> Die Bereitstellung dauert etwa 10 Minuten.

Notieren Sie sich nach Abschluss der Bereitstellung die zurückgegebene Verbindungszeichenfolge für Azure Event Hubs:

1. Warten Sie, bis die Bereitstellung abgeschlossen ist.
1. Klicken Sie auf **Ausgaben**.
1. Notieren Sie sich den Wert von **eventHubConnectionString**, da Sie ihn später noch verwenden:

    :::image type="content" source="media/quick-export-data/connection-string.png" alt-text="Event Hubs-Verbindungszeichenfolge":::

## <a name="configure-data-export"></a>Konfigurieren des Datenexports

In diesem Szenario wird eine Azure Event Hubs-Warteschlange verwendet, um Telemetriedaten an Azure Data Explorer zu übergeben, die von Ihrer IoT Central-Anwendung gesammelt wurden.

So konfigurieren Sie den Datenexport aus IoT Central

1. Navigieren Sie in Ihrer IoT Central-Anwendung zur Seite **Datenexport**.
1. Wählen Sie die Registerkarte **Ziele** und dann **Ziel hinzufügen** aus.
1. Geben Sie als Zielnamen *Event Hub 1* ein. Wählen Sie als Zieltyp **Azure Event Hubs** aus.
1. Geben Sie die Verbindungszeichenfolge der Event Hubs-Warteschlange ein, die Sie im vorherigen Abschnitt gespeichert haben. Der Wert für **Event Hub** wird automatisch mit *centraltelemetry* ausgefüllt.
1. Wählen Sie **Speichern** aus.
1. Wählen Sie auf der Seite **Datenexport** die Registerkarte **Exporte** und dann **Export hinzufügen** aus.
1. Geben Sie als Namen für den Export *Telemetry Export* (Telemetrieexport) ein.
1. Wählen Sie als Typ der zu exportierenden Daten **Telemetrie** aus.
1. Fügen Sie **Event Hub 1** als Ziel hinzu.
1. Wählen Sie **Speichern** aus.

Wenn der Export ausgeführt wird, lautet der Status auf der Seite **Datenexport** **Fehlerfrei**:

:::image type="content" source="media/quick-export-data/healthy-export.png" alt-text="Screenshot eines ausgeführten Datenexports mit dem Status „Fehlerfrei“":::

## <a name="configure-data-ingestion"></a>Konfigurieren der Datenerfassung

Stellen Sie vor dem Fortfahren sicher, dass die Smartphone-App mit Ihrer IoT Central-Anwendung verbunden ist und Daten sendet.

Ihre IoT Central-Anwendung exportiert fortlaufend Telemetriedaten in die Event Hubs-Warteschlange. In diesem Abschnitt konfigurieren Sie Ihren Azure Data Explorer-Cluster für die kontinuierliche Erfassung der Telemetriedaten in einer Tabelle, in der Sie dann abgefragt werden können.

So konfigurieren Sie die Datenerfassung

1. Navigieren Sie im Azure-Portal in der Ressourcengruppe **central-quickstart** zu Ihrem Azure Data Explorer-Cluster:

    :::image type="content" source="media/quick-export-data/azure-data-explorer-portal.png" alt-text="Screenshot der Übersichtsseite von Azure Data Explorer":::

1. Wählen Sie **Neue Daten erfassen** aus.
1. Wählen Sie auf der Seite **Neue Daten erfassen** Ihren Cluster und die Datenbank **iotcentraldata** aus.
1. Wählen Sie **Neu erstellen** aus, um eine neue Tabelle mit dem Namen *telemetry* zu erstellen.
1. Wählen Sie als Ressourcentyp **Event Hubs** aus.
1. Geben Sie als Namen für die Datenverbindung *IoT-Central-connection* ein.
1. Verwenden Sie die Informationen in der folgenden Tabelle, um den Rest des Formulars auszufüllen:

    | Feld                   | Wert                            |
    |-------------------------|----------------------------------|
    | Subscription            | Auswählen des Azure-Abonnements   |
    | Event Hub-Namespace     | Wählen Sie Ihren Event Hubs-Namespace aus. |
    | Event Hub               | `centraltelemetry`               |
    | Consumergruppe          | `$Default`                       |
    | Datenformat             | JSON                             |
    | Komprimierung             | Keine                             |
    | Ereignissystemeigenschaften | Nicht ausfüllen                      |

1. Wählen Sie **Schema bearbeiten** aus.
1. Auf der Seite **Schema** wird eine Datenvorschau der Nachrichten in der Event Hubs-Warteschlange angezeigt.
1. Ändern Sie den Wert für die Schachtelungsebenen in `3`, um den JSON-Code zu erweitern und jeden Telemetriewert in einer eigenen Spalte anzuzeigen.
1. Wählen Sie **Erfassung starten** aus. Warten Sie, bis die Datenerfassung abgeschlossen ist:

:::image type="content" source="media/quick-export-data/data-ingestion-complete.png" alt-text="Screenshot der abgeschlossenen Datenerfassung in Azure Data Explorer":::

Lassen Sie diese Seite geöffnet, da Sie sie im nächsten Abschnitt verwenden.

## <a name="query-exported-data"></a>Abfragen exportierter Daten

Ihr Azure Data Explorer-Cluster erfasst nun fortlaufend Daten von Ihrer IoT Central-Anwendung. So fragen Sie die Daten ab

1. Wählen Sie auf der Azure Data Explorer-Seite aus dem vorherigen Abschnitt die Schnellabfrage **10 zurückgeben** aus. Diese Abfrage wählt zehn Datensätze aus der Tabelle **telemetry** aus.
1. Ersetzen Sie die Abfrage durch folgende Abfrage:

    ```kusto
    ['telemetry'] 
    | where isnotnull(telemetry_magnetometer_x)
    | project Time=todatetime(enqueuedTime), deviceId, telemetry_magnetometer_x, telemetry_magnetometer_y, telemetry_magnetometer_z
    | render timechart 
    ```

    Diese Abfrage zeichnet die Telemetriewerte des Magnetometers aus der Smartphone-App auf einer Zeitachse ein.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

[!INCLUDE [iot-central-clean-up-resources](../../../includes/iot-central-clean-up-resources.md)]

Um die Azure Data Explorer-Instanz und den Event Hubs-Namespace zur Vermeidung unnötiger Kosten aus Ihrem Abonnement zu entfernen, löschen Sie die Ressourcengruppe **central-quickstarts** im [Azure-Portal](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceGroups).

## <a name="next-steps"></a>Nächste Schritte

In diesem Schnellstart haben Sie erfahren, wie Sie Daten fortlaufend aus IoT Central in einen anderen Azure-Dienst exportieren.

Nachdem Sie nun wissen, wie Sie Ihre Daten exportieren, wird als Nächstes der folgende Schritt empfohlen:

> [!div class="nextstepaction"]
> [Einrichten und Verwalten einer Gerätevorlage (Previewfunktionen)](howto-set-up-template.md)
