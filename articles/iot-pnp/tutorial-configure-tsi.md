---
title: Verwenden von Time Series Insights zum Speichern und Analysieren der Telemetriedaten Ihrer Azure IoT Plug & Play-Geräte | Microsoft-Dokumentation
description: Richten Sie eine Time Series Insights-Umgebung ein, und verbinden Sie Ihren IoT-Hub, um die Telemetriedaten Ihrer IoT Plug & Play-Geräte anzuzeigen und zu analysieren.
author: lyrana
ms.author: lyhughes
ms.date: 10/14/2020
ms.topic: tutorial
ms.service: iot-pnp
services: iot-pnp
ms.openlocfilehash: ca2319a78fb4c0c720a21e97944d5b75ada9d008
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/25/2020
ms.locfileid: "96014990"
---
# <a name="preview-tutorial-create-and-connect-to-time-series-insights-gen2-to-store-visualize-and-analyze-iot-plug-and-play-device-telemetry"></a>Tutorial für die Vorschauversion: Durchführen der Erstellung und Verbindungsherstellung für Time Series Insights Gen2 zum Speichern, Visualisieren und Analysieren der Telemetriedaten von IoT Plug & Play-Geräten

In diesem Tutorial wird beschrieben, wie Sie eine Umgebung vom Typ [Azure Time Series Insights Gen2](https://docs.microsoft.com/azure/time-series-insights/overview-what-is-tsi) (TSI) erstellen und richtig konfigurieren, um sie in Ihre IoT Plug & Play-Lösung zu integrieren. Verwenden Sie TSI, um Zeitreihendaten für das Internet der Dinge (Internet of Things, IoT) zu erfassen, zu verarbeiten, zu speichern, abzufragen und zu visualisieren.

Zunächst stellen Sie eine TSI-Umgebung bereit und verbinden Ihren IoT-Hub als Ereignisquelle für das Streaming. Anschließend führen Sie die Modellsynchronisierung durch, um Ihr [Zeitreihenmodell](../time-series-insights/concepts-model-overview.md) zu erstellen. Es basiert auf den Modelldateien des [DTDL-Beispiels (Digital Twins Definition Language)](https://github.com/Azure/opendigitaltwins-dtdl), die Sie für den Temperaturregler und das Thermostat verwendet haben.

> [!NOTE]
> Diese Integration befindet sich in der Vorschauphase. Die Art der Zuordnung von DTDL-Gerätemodellen zum Zeitreihenmodell kann sich unter Umständen ändern.

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [iot-pnp-prerequisites](../../includes/iot-pnp-prerequisites.md)]

An diesem Punkt verfügen Sie über Folgendes:

* Einen Azure IoT Hub.
* Eine DPS-Instanz, die mit Ihrem IoT-Hub verknüpft ist und eine individuelle Geräteregistrierung für Ihr IoT Plug & Play-Gerät aufweist.
* Eine Verbindung mit Ihrem IoT-Hub über ein Gerät, mit dem simulierte Daten gestreamt werden und das eine oder mehrere Komponenten umfasst.

Damit Sie die Azure-Befehlszeilenschnittstelle (Azure CLI) nicht lokal installieren müssen, können Sie die Clouddienste über die Azure Cloud Shell einrichten.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="prepare-your-event-source"></a>Vorbereiten Ihrer Ereignisquelle

Der von Ihnen zuvor erstellte IoT-Hub ist die [Ereignisquelle](https://docs.microsoft.com/azure/time-series-insights/concepts-streaming-ingestion-event-sources) Ihrer TSI-Umgebung.

> [!IMPORTANT]
> Deaktivieren Sie alle vorhandenen IoT Hub-Routen. Es besteht ein bekanntes Problem, wenn Sie einen IoT-Hub als TSI-Ereignisquelle mit konfiguriertem [Routing](../iot-hub/iot-hub-devguide-messages-d2c.md#routing-endpoints) verwenden. Deaktivieren Sie vorübergehend alle Routingendpunkte. Wenn für Ihren IoT-Hub die Verbindung mit TSI hergestellt wurde, können Sie sie wieder aktivieren.

Erstellen Sie eine eindeutige Consumergruppe für Ihren IoT-Hub, die von TSI verwendet werden kann. Ersetzen Sie `my-pnp-hub` durch den Namen des IoT-Hubs, den Sie zuvor verwendet haben:

```azurecli-interactive
az iot hub consumer-group create --hub-name my-pnp-hub --name tsi-consumer-group
```

## <a name="choose-your-time-series-id"></a>Auswählen Ihrer Zeitreihen-ID

Beim Bereitstellen Ihrer TSI-Umgebung müssen Sie eine *Zeitreihen-ID* auswählen. Es ist wichtig, dass Sie die richtige Zeitreihen-ID auswählen, da diese Eigenschaft unveränderlich ist und nach dem Festlegen nicht mehr geändert werden kann. Eine Zeitreihen-ID ähnelt dem Partitionsschlüssel einer Datenbank. Die Zeitreihen-ID fungiert als Primärschlüssel für das Zeitreihenmodell. Weitere Informationen finden Sie unter [Bewährte Methoden für die Auswahl einer Time Series-ID](../time-series-insights/how-to-select-tsid.md).

Geben Sie als IoT Plug & Play-Benutzer einen _zusammengesetzten Schlüssel_ ein, der `iothub-connection-device-id` und `dt-subject` als Zeitreihen-ID enthält. Diese Systemeigenschaften, die Ihre IoT Plug & Play-Geräte-ID bzw. Ihre Gerätekomponentennamen enthalten, werden von IoT Hub hinzugefügt.

Auch wenn für Ihre IoT Plug & Play-Gerätemodelle derzeit keine Komponenten genutzt werden, sollten Sie `dt-subject` als Teil des zusammengesetzten Schlüssels einfügen, damit die Nutzung in Zukunft möglich ist. Da Ihre Zeitreihen-ID unveränderlich ist, empfiehlt Microsoft die Aktivierung dieser Option für den Fall, dass Sie sie später einmal benötigen.

> [!NOTE]
> Die folgenden Beispiele beziehen sich auf das Gerät **TemperatureController** mit mehreren Komponenten, aber die Vorgehensweisen gelten auch für das Gerät **Thermostat** ohne Komponenten.

## <a name="provision-your-tsi-environment"></a>Bereitstellen Ihrer TSI-Umgebung

In diesem Abschnitt wird beschrieben, wie Sie Ihre Azure Time Series Insights Gen2-Umgebung bereitstellen.

Der unten angegebene Befehl bewirkt Folgendes:

* Erstellt ein Azure-Speicherkonto für den [kalten Speicher](https://docs.microsoft.com/azure/time-series-insights/concepts-storage#cold-store) (Cold Storage) Ihrer Umgebung, der für die Langzeitaufbewahrung und die Analyse von Verlaufsdaten ausgelegt ist.
  * Ersetzen Sie `mytsicoldstore` durch einen eindeutigen Namen für Ihr Cold Storage-Konto.
* Erstellt eine Azure Time Series Insights Gen2-Umgebung, einschließlich Warm Storage mit einem Aufbewahrungszeitraum von sieben Tagen und Cold Storage für die Aufbewahrung mit unbegrenzter Dauer.
  * Ersetzen Sie `my-tsi-env` durch einen eindeutigen Namen für Ihre TSI-Umgebung.
  * Ersetzen Sie `my-pnp-resourcegroup` durch den Namen der Ressourcengruppe, die Sie während der Einrichtung verwendet haben.
  * `iothub-connection-device-id, dt-subject` ist Ihre Eigenschaft für die Zeitreihen-ID.

```azurecli-interactive
storage=mytsicoldstore
rg=my-pnp-resourcegroup
az storage account create -g $rg -n $storage --https-only
key=$(az storage account keys list -g $rg -n $storage --query [0].value --output tsv)
az timeseriesinsights environment longterm create --name my-tsi-env --resource-group $rg --time-series-id-properties iothub-connection-device-id, dt-subject --sku-name L1 --sku-capacity 1 --data-retention 7 --storage-account-name $storage --storage-management-key $key --location eastus2
```

Stellen Sie eine Verbindung mit Ihrer IoT Hub-Ereignisquelle her. Ersetzen Sie `my-pnp-resourcegroup`, `my-pnp-hub` und `my-tsi-env` durch die Werte, die Sie ausgewählt haben. Im folgenden Befehl wird auf die Consumergruppe für TSI verwiesen, die Sie zuvor erstellt haben:

```azurecli-interactive
rg=my-pnp-resourcegroup
iothub=my-pnp-hub
env=my-tsi-env
es_resource_id=$(az iot hub create -g $rg -n $iothub --query id --output tsv)
shared_access_key=$(az iot hub policy list -g $rg --hub-name $iothub --query "[?keyName=='service'].primaryKey" --output tsv)
az timeseriesinsights event-source iothub create -g $rg --environment-name $env -n iot-hub-event-source --consumer-group-name tsi-consumer-group  --key-name iothubowner --shared-access-key $shared_access_key --event-source-resource-id $es_resource_id
```

Navigieren Sie im [Azure-Portal](https://portal.azure.com) zu Ihrer Ressourcengruppe, und wählen Sie Ihre neue Time Series Insights-Umgebung aus. Navigieren Sie zur in der Instanzübersicht angezeigten *Time Series Insights-Explorer-URL*:

![Portalübersichtsseite](./media/tutorial-configure-tsi/view-environment.png)

Im Explorer werden drei Instanzen angezeigt:

* &lt;Ihre PNP-Geräte-ID&gt;, thermostat1
* &lt;Ihre PNP-Geräte-ID&gt;, thermostat2
* &lt;Ihre PNP-Geräte-ID&gt;, `null`

> [!NOTE]
> Das dritte Tag steht für die Telemetriedaten des eigentlichen **TemperatureController**-Geräts, z. B. den Arbeitssatz des Gerätearbeitsspeichers. Da es sich hierbei um eine Eigenschaft der obersten Ebene handelt, lautet der Wert für den Komponentennamen „null“. In einem späteren Schritt aktualisieren Sie dies auf einen benutzerfreundlicheren Namen.

![Explorer-Ansicht 1](./media/tutorial-configure-tsi/tsi-env-first-view.png)

## <a name="configure-model-translation"></a>Konfigurieren der Modellübersetzung

Als Nächstes übersetzen Sie Ihr DTDL-Gerätemodell in das Objektmodell in Azure Time Series Insights (TSI). Das Zeitreihenmodell von TSI ist ein Tool für die semantische Modellierung, das für die Kontextualisierung von Daten in TSI eingesetzt wird. Das Zeitreihenmodell besteht aus drei Kernkomponenten:

* [Zeitreihenmodellinstanzen](../time-series-insights/concepts-model-overview.md#time-series-model-instances): Instanzen sind virtuelle Darstellungen der Zeitreihe selbst. Instanzen werden durch Ihre Zeitreihen-ID eindeutig identifiziert.
* [Zeitreihenmodellhierarchien](../time-series-insights/concepts-model-overview.md#time-series-model-hierarchies): Hierarchien organisieren Instanzen durch Angabe von Eigenschaftsnamen und deren Beziehungen.
* [Zeitreihenmodelltypen](../time-series-insights/concepts-model-overview.md#time-series-model-types): Diese Typen dienen Ihnen als Hilfe beim Definieren von [Variablen](../time-series-insights/concepts-variables.md) oder Formeln für Berechnungen. Typen werden einer bestimmten-Instanz zugeordnet.

### <a name="define-your-types"></a>Definieren Ihrer Typen

Sie können mit dem Erfassen von Daten in Azure Time Series Insights Gen2 beginnen, ohne vorher ein Modell zu definieren. Wenn Telemetriedaten eintreffen, versucht TSI, Zeitreiheninstanzen basierend auf Ihren Eigenschaftswerten der Zeitreihen-ID automatisch aufzulösen. Allen Instanzen wird der *Standardtyp* zugewiesen. Sie müssen manuell einen neuen Typ erstellen, um Ihre Instanzen richtig zu kategorisieren. Unten ist die einfachste Methode zum Synchronisieren Ihrer DTDL-Gerätemodelle mit Ihren Zeitreihenmodell-Typen dargestellt:

* Ihr Bezeichner für das Modell für digitale Zwillinge wird zu Ihrer Typ-ID.
* Der Typname kann entweder der Name oder der Anzeigename des Modells sein.
* Die Modellbeschreibung wird zur Beschreibung des Typs.
* Mindestens eine Typvariable wird für jedes Telemetriedatenelement mit einem numerischen Schema erstellt.
  * Für Variablen können nur numerische Datentypen verwendet werden. Wenn ein Wert aber als anderer Typ gesendet wird, der konvertiert werden kann (z. B. `"0"`), können Sie eine [Konvertierungsfunktion](/rest/api/time-series-insights/reference-time-series-expression-syntax.md#conversion-functions) wie `toDouble` verwenden.
* Der Variablenname kann entweder der Name oder der Anzeigename des Telemetriedatenelements sein.
* Beim Definieren des Zeitreihenausdrucks sollten Sie den Telemetrienamen des Netzwerks und den zugehörigen Datentyp verwenden.

| DTDL (JSON) | Zeitreihenmodelltyp (JSON) | Beispielwert |
|-----------|------------------|-------------|
| `@id` | `id` | `dtmi:com:example:TemperatureController;1` |
| `displayName`    | `name`   |   `Temperature Controller`  |
| `description`  |  `description`  |  `Device with two thermostats and remote reboot.` |  
|`contents` (Array)| `variables` (Objekt)  | Siehe Beispiel unten

![DTDL zu Zeitreihenmodelltyp](./media/tutorial-configure-tsi/DTDL-to-TSM-Type.png)

> [!NOTE]
> Dieses Beispiel enthält nur drei Variablen, aber es können für jeden Typ bis zu 100 Variablen verwendet werden. In Variablen kann jeweils auf denselben Telemetriedatenwert verwiesen werden, um je nach Bedarf unterschiedliche Berechnungen durchzuführen. Eine vollständige Liste mit Filtern, Aggregaten und Skalarfunktionen finden Sie unter [Syntax für Zeitreihenausdrücke in Time Series Insights Gen2](/rest/api/time-series-insights/reference-time-series-expression-syntax.md).

Öffnen Sie einen Text-Editor, und speichern Sie den folgenden JSON-Code auf Ihrem lokalen Laufwerk:

```JSON
{
  "put": [
    {
      "id": "dtmi:com:example:TemperatureController;1",
      "name": "Temperature Controller",
      "description": "Device with two thermostats and remote reboot.",
      "variables": {
        "workingSet": {
          "kind": "numeric",
          "value": {
            "tsx": "coalesce($event.workingSet.Long, toLong($event.workingSet.Double))"
          }, 
          "aggregation": {
            "tsx": "avg($value)"
          }
        },
        "temperature": {
          "kind": "numeric",
          "value": {
            "tsx": "coalesce($event.temperature.Long, toLong($event.temperature.Double))"
          },
          "aggregation": {
            "tsx": "avg($value)"
          }
        },
        "eventCount": {
          "kind": "aggregate",
          "aggregation": {
            "tsx": "count()"
          }
        }
      }
    }
  ]
}
```

Navigieren Sie im Time Series Insights-Explorer zur Registerkarte **Modell**, indem Sie links das Modellsymbol auswählen. Wählen Sie die Option **Typen** und dann **JSON hochladen** aus:

![Upload](./media/tutorial-configure-tsi/upload-type.png)

Wählen Sie die Option **Datei auswählen**, den oben gespeicherten JSON-Code und dann **Hochladen** aus.

Der neu definierte Typ **Temperature Controller** wird angezeigt.

### <a name="create-a-hierarchy"></a>Erstellen einer Hierarchie

Erstellen Sie eine Hierarchie zum Organisieren der Tags unter dem übergeordneten Element **TemperatureController**. Das folgende einfache Beispiel verfügt nur über eine Ebene. IoT-Lösungen weisen häufig aber viele Schachtelungsebenen auf, damit Tags anhand ihrer physischen und semantischen Position innerhalb einer Organisation kontextualisiert werden können.

Wählen Sie die Option **Hierarchien** und dann **Hierarchie hinzufügen** aus. Geben Sie *Device Fleet* als Name ein, und erstellen Sie eine Ebene mit dem Namen *Device Name*. Klicken Sie auf **Speichern**.

![Hinzufügen einer Hierarchie](./media/tutorial-configure-tsi/add-hierarchy.png)

### <a name="assign-your-instances-to-the-correct-type"></a>Zuweisen Ihrer Instanzen zum richtigen Typ

Als Nächstes ändern Sie den Typ Ihrer Instanzen und ordnen sie in der Hierarchie an.

Wählen Sie die Registerkarte **Instanzen** aus, suchen Sie nach der Instanz, die für den Arbeitssatz des Geräts steht, und wählen Sie ganz rechts das Symbol **Bearbeiten** aus:

![Bearbeiten von Instanzen](./media/tutorial-configure-tsi/edit-instance.png)

Wählen Sie die Dropdownliste **Typ** und dann **Temperature Controller** aus. Geben Sie *defaultComponent, <your device name>* ein, um den Namen der Instanz zu aktualisieren, die für alle Tags der obersten Ebene Ihres Geräts steht.

![Ändern des Instanztyps](./media/tutorial-configure-tsi/change-type.png)

Wählen Sie vor dem Auswählen von „Speichern“ die Registerkarte **Instanzfelder** aus, und aktivieren Sie das Kontrollkästchen **Device Fleet**. Geben Sie `<your device name> - Temp Controller` ein, um die Telemetriedaten zu gruppieren, und wählen Sie anschließend die Option **Speichern** aus.

![Zuweisen zur Hierarchie](./media/tutorial-configure-tsi/assign-to-hierarchy.png)

Wiederholen Sie die vorherigen Schritte, um für Ihre Thermostat-Tags den richtigen Typ und die richtige Hierarchie zuzuweisen.

## <a name="view-your-data"></a>Prüfen der Daten

Navigieren Sie zurück zum Diagrammbereich, und erweitern Sie **Device Fleet > Ihr Gerät**. Wählen Sie nacheinander **thermostat1**, die Variable **Temperatur** und die Option **Hinzufügen** aus, um den Wert im Diagramm darzustellen. Gehen Sie genauso für **thermostat2** und den **workingSet**-Wert **defaultComponent** vor.

![Ändern des Instanztyps für „thermostat2“](./media/tutorial-configure-tsi/charting-values.png)

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zu den verschiedenen Diagrammoptionen, z. B. Intervallgröße und Steuerelemente für die Y-Achse, finden Sie unter [Azure Time Series Insights-Explorer](../time-series-insights/concepts-ux-panels.md).

* Eine ausführliche Übersicht über das Zeitreihenmodell Ihrer Umgebung finden Sie im Artikel [Zeitreihenmodell in Azure Time Series Insights Gen2](../time-series-insights/concepts-model-overview.md).

* Ausführliche Informationen zu den Abfrage-APIs und zur Syntax für den Zeitreihenausdruck finden Sie unter [Azure Time Series Insights Gen2-Abfrage-APIs](/rest/api/time-series-insights/reference-query-apis.md).
