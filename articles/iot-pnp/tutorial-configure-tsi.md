---
title: Verwenden von Time Series Insights zum Speichern und Analysieren der Telemetriedaten Ihrer Azure IoT Plug & Play-Geräte | Microsoft-Dokumentation
description: Richten Sie eine Time Series Insights-Umgebung ein, und verbinden Sie Ihre IoT Hub-Instanz, um die Telemetriedaten Ihrer IoT Plug & Play-Geräte anzuzeigen und zu analysieren.
author: lyrana
ms.author: lyhughes
ms.date: 10/14/2020
ms.topic: tutorial
ms.service: iot-pnp
services: iot-pnp
ms.openlocfilehash: aa99b9059fe8e3cd5b0dfe6f7e62bd02012fd144
ms.sourcegitcommit: 7cc10b9c3c12c97a2903d01293e42e442f8ac751
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2020
ms.locfileid: "93422262"
---
# <a name="tutorial-create-and-connect-to-time-series-insights-gen2-to-store-visualize-and-analyze-iot-plug-and-play-device-telemetry"></a>Tutorial: Durchführen der Erstellung und Verbindungsherstellung für Time Series Insights Gen2 zum Speichern, Visualisieren und Analysieren der Telemetriedaten von IoT Plug & Play-Geräten

In diesem Tutorial wird beschrieben, wie Sie eine Umgebung vom Typ [Azure Time Series Insights Gen2](https://docs.microsoft.com/azure/time-series-insights/overview-what-is-tsi) (TSI) erstellen und richtig konfigurieren, um sie in Ihre IoT Plug & Play-Lösung zu integrieren. Mit TSI können Sie Zeitreihendaten für das Internet der Dinge (Internet of Things, IoT) erfassen, verarbeiten, speichern, abfragen und visualisieren.

Zunächst stellen Sie eine TSI-Umgebung bereit und verbinden Ihre IoT Hub-Instanz als Ereignisquelle für das Streaming. Anschließend führen Sie die Modellsynchronisierung durch, um das Zeitreihenmodell Ihrer TSI-Umgebung zu erstellen. Es basiert auf den Modelldateien des [DTDL-Beispiels (Digital Twins Definition Language)](https://github.com/Azure/opendigitaltwins-dtdl), die Sie für den Temperaturregler und das Thermostat verwendet haben.

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [iot-pnp-prerequisites](../../includes/iot-pnp-prerequisites.md)]

Damit Sie die Azure-Befehlszeilenschnittstelle (Azure CLI) nicht lokal installieren müssen, können Sie die Clouddienste über die Azure Cloud Shell einrichten.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

### <a name="time-series-id-selection"></a>Auswahl der Zeitreihen-ID

Beim Bereitstellen Ihrer TSI-Umgebung müssen Sie eine Zeitreihen-ID auswählen. Die Auswahl der richtigen Zeitreihen-ID ist wichtig, da die Eigenschaft unveränderlich ist und nach dem Festlegen nicht mehr bearbeitet werden kann. Das Auswählen einer Zeitreihen-ID ähnelt dem Auswählen eines Partitionsschlüssels für eine Datenbank. Normalerweise ist Ihre Zeitreihen-ID der Blattknoten Ihres Objektmodells. Anders ausgedrückt: Sie sollten in der Regel die ID-Eigenschaft des detailliertesten Objekts oder Sensors auswählen, von dem Telemetriedaten ausgegeben werden.

Als IoT Plug & Play-Benutzer sollten Sie sich beim Auswählen Ihrer Zeitreihen-ID die Frage stellen, ob in Ihren Gerätemodellen [Komponenten](https://github.com/Azure/opendigitaltwins-dtdl/blob/master/DTDL/v2/dtdlv2.md#component) vorhanden sind. 

![Auswahl der Zeitreihen-ID](./media/tutorial-configure-tsi/ts-id-selection-pnp.png)

* Wenn Sie die Schnellstartanleitung durchgearbeitet haben und Ihr IoT Hub-Gerät das [Thermostat](https://raw.githubusercontent.com/Azure/opendigitaltwins-dtdl/master/DTDL/v2/samples/Thermostat.json) ist, sollten Sie `iot-hub-connection-device-id` als Zeitreihen-ID verwenden.

* Falls Sie eines der Tutorials für den Temperaturregler ([TemperatureController](https://raw.githubusercontent.com/Azure/opendigitaltwins-dtdl/master/DTDL/v2/samples/TemperatureController.json)) durchgearbeitet haben, sollten Sie im Abschnitt unten einen zusammengesetzten Schlüssel im Format `iot-hub-connection-device-id, dt-subject` verwenden.

## <a name="provision-your-azure-time-series-insights-gen2-environment"></a>Bereitstellen Ihrer Azure Time Series Insights Gen2-Umgebung

Der hier angegebene Befehl bewirkt Folgendes:

* Erstellt ein Azure-Speicherkonto für den [kalten Speicher](https://docs.microsoft.com/azure/time-series-insights/concepts-storage#cold-store) (Cold Storage) Ihrer Umgebung, der für die Langzeitaufbewahrung und die Analyse von Verlaufsdaten ausgelegt ist.
  * Ersetzen Sie `mytsicoldstore` durch einen eindeutigen Namen für Ihr Konto.
* Erstellt eine Azure Time Series Insights Gen2-Umgebung, einschließlich Warm Storage mit einem Aufbewahrungszeitraum von sieben Tagen und Cold Storage für die Aufbewahrung mit unbegrenzter Dauer. 
  * Ersetzen Sie `my-tsi-env` durch einen eindeutigen Namen für Ihre TSI-Umgebung. 
  * Ersetzen Sie `my-pnp-resourcegroup` durch den Namen der Ressourcengruppe, die Sie während der Einrichtung verwendet haben.
  * Ersetzen Sie `my-ts-id-property` basierend auf den obigen Auswahlkriterien durch den Eigenschaftswert der Zeitreihen-ID.

```azurecli-interactive
storage=mytsicoldstore
rg=my-pnp-resourcegroup
az storage account create -g $rg -n $storage --https-only
key=$(az storage account keys list -g $rg -n $storage --query [0].value --output tsv)
az timeseriesinsights environment longterm create --name my-tsi-env --resource-group $rg --time-series-id-properties my-ts-id-property --sku-name L1 --sku-capacity 1 --data-retention 7 --storage-account-name $storage --storage-management-key $key --location eastus2
```

Nun konfigurieren Sie die IoT Hub-Instanz, die Sie weiter oben erstellt haben, als [Ereignisquelle](https://docs.microsoft.com/azure/time-series-insights/concepts-streaming-ingestion-event-sources) Ihrer Umgebung. Nach der Verbindungsherstellung für Ihre Ereignisquelle beginnt TSI mit dem Indizieren der Ereignisse Ihres Hubs. Hierbei wird mit dem ältesten Ereignis der Warteschlange begonnen.

Erstellen Sie zunächst eine eindeutige Consumergruppe für die IoT Hub-Instanz Ihrer TSI-Umgebung. Ersetzen Sie `my-pnp-hub` durch den Namen der IoT Hub-Instanz, die Sie zuvor verwendet haben.

```azurecli-interactive
az iot hub consumer-group create --hub-name my-pnp-hub --name tsi-consumer-group 
```

Stellen Sie die Verbindung mit der IoT Hub-Instanz her. Ersetzen Sie `my-pnp-resourcegroup`, `my-pnp-hub` und `my-tsi-env` durch Ihre entsprechenden Werte.

```azurecli-interactive
rg=my-pnp-resourcegroup
iothub=my-pnp-hub
env=my-tsi-env
es_resource_id=$(az iot hub create -g $rg -n $iothub --query id --output tsv)
shared_access_key=$(az iot hub policy list -g $rg --hub-name $iothub --query "[?keyName=='service'].primaryKey" --output tsv)
az timeseriesinsights event-source iothub create -g $rg --environment-name $env -n iot-hub-event-source --consumer-group-name tsi-consumer-group  --key-name iothubowner --shared-access-key $shared_access_key --event-source-resource-id $es_resource_id
```
Navigieren Sie im [Azure-Portal](https://portal.azure.com) zu Ihrer Ressourcengruppe, und wählen Sie die neu erstellte Time Series Insights-Umgebung aus. Besuchen Sie die in der Instanzübersicht angezeigte *Time Series Insights-Explorer-URL*.

![Portalübersichtsseite](./media/tutorial-configure-tsi/view-environment.png)

Im Explorer sollten Ihre beiden Thermostate unter „Alle Hierarchien“ angezeigt werden. Als Nächstes stellen Sie Ihr Zeitreihenmodell basierend auf Ihrem Gerätemodell zusammen.

![Explorer-Ansicht 1](./media/tutorial-configure-tsi/tsi-env-first-view.png)

## <a name="model-synchronization-between-digital-twins-definition-language-and-time-series-insights-gen2"></a>Modellsynchronisierung zwischen Digital Twins Definition Language und Time Series Insights Gen2

Nun übersetzen Sie Ihr DTDL-Gerätemodell in das Objektmodell in Azure Time Series Insights (TSI). Das Zeitreihenmodell von TSI ist ein Tool für die semantische Modellierung, das für die Kontextualisierung von Daten in TSI eingesetzt wird. Das Zeitreihenmodell besteht aus drei Kernkomponenten:

* [Zeitreihenmodellinstanzen](https://docs.microsoft.com/azure/time-series-insights/concepts-model-overview#time-series-model-instances): Instanzen sind virtuelle Darstellungen der Zeitreihe selbst. Durch Ihre Zeitreihen-ID werden Instanzen eindeutig identifiziert.
* [Zeitreihenmodellhierarchien](https://docs.microsoft.com/azure/time-series-insights/concepts-model-overview#time-series-model-hierarchies): Hierarchien organisieren Instanzen durch Angabe von Eigenschaftsnamen und deren Beziehungen.
* [Zeitreihenmodelltypen](https://docs.microsoft.com/azure/time-series-insights/concepts-model-overview#time-series-model-types): Diese Typen dienen Ihnen als Hilfe beim Definieren von [Variablen](https://docs.microsoft.com/azure/time-series-insights/concepts-variables) oder Formeln zur Durchführung von Berechnungen. Typen werden einer bestimmten-Instanz zugeordnet.

> [!NOTE]
> Die unten angegebenen Beispiele gelten für den Temperaturregler (TemperatureController) mit mehreren Komponenten.

### <a name="define-your-types"></a>Definieren Ihrer Typen

Sie können mit dem Erfassen von Daten in Azure Time Series Insights Gen2 beginnen, ohne vorher ein Modell zu definieren. Beim Empfang von Telemetriedaten versucht TSI, Zeitreiheninstanzen basierend auf dem Eigenschaftswert der Zeitreihen-ID automatisch aufzulösen. Allen Instanzen wird der *Standardtyp* zugewiesen. Sie müssen manuell einen neuen Typ erstellen, um Ihre Modelle darzustellen. In der folgenden Abbildung ist eine einfache Methode zum Synchronisieren eines DTDL-Modells und eines TSM-Typs dargestellt:

![Typ für „DTDL zu TSM“](./media/tutorial-configure-tsi/DTDL-to-TSM-Type.png)

* Ihr Bezeichner für das Modell für digitale Zwillinge (Digital Twin Model Identifier, DTMI) wird zu Ihrer Typ-ID.
* Der Typname kann entweder der Name oder Anzeigename des Modells sein.
* Die Modellbeschreibung wird zur Beschreibung des Typs.
* Mindestens eine Typvariable wird für jede Telemetriekomponente erstellt, die über ein numerisches Schema verfügt. 
  * Für Variablen können nur numerische Datentypen verwendet werden. Wenn ein Wert aber als Zeichenfolge gesendet wird, die analysierbar ist (z. B. `"0"`), können Sie eine [Konvertierungsfunktion](https://docs.microsoft.com/rest/api/time-series-insights/reference-time-series-expression-syntax#conversion-functions) wie `toDouble` verwenden.
* Der Variablenname kann entweder der Telemetriename oder -anzeigename sein.
* Beim Definieren des Zeitreihenausdrucks (Time Series Expression, TSX) sollten Sie den Telemetrienamen des Netzwerks und den zugehörigen Datentyp verwenden.

> [!NOTE]
> In diesem Beispiel werden nur zwei Variablen angezeigt: ein Aggregatwert und ein numerischer Wert. Es ist aber möglich, für jeden Typ bis zu 100 Variablen zu verwenden. In Variablen kann jeweils auf denselben Telemetriedatenwert verwiesen werden, um je nach Bedarf unterschiedliche Berechnungen durchzuführen. Eine vollständige Liste mit Filtern, Aggregaten und Skalarfunktionen finden Sie in der Dokumentation zur [Syntax für Zeitreihenausdrücke in Time Series Insights Gen2](https://docs.microsoft.com/rest/api/time-series-insights/reference-time-series-expression-syntax).

Öffnen Sie den Text-Editor Ihrer Wahl, und speichern Sie den folgenden JSON-Code auf Ihrem lokalen Laufwerk:

```JSON
{
  "put": [
    {
      "id": "dtmi:com:example:Thermostat;1",
      "name": "Thermostat",
      "description": "Reports current temperature and provides desired temperature control.",
      "variables": {
        "EventCount": {
          "kind": "aggregate",
          "aggregation": {
            "tsx": "count()"
          }
        },
        "Temperature": {
          "kind": "numeric",
          "value": {
            "tsx": "$event.temperature.Double"
          },
          "aggregation": {
            "tsx": "avg($value)"
          }
        }
      }
    }
  ]
}
```

Navigieren Sie im Time Series Insights-Explorer zur Registerkarte „Modell“, indem Sie links auf das Modellsymbol klicken. Klicken Sie auf **Typen** und dann auf **JSON hochladen**:

![Upload](./media/tutorial-configure-tsi/upload-type.png)

Wählen Sie die Option **Datei auswählen** und den oben gespeicherten JSON-Code aus, und klicken Sie auf **Hochladen**.

Der neu definierte Thermostattyp sollte angezeigt werden.

### <a name="optional---create-a-hierarchy"></a>Optional: Erstellen einer Hierarchie

Optional können Sie eine Hierarchie erstellen, um die beiden Thermostatkomponenten unter dem übergeordneten TemeraptureController-Objekt zu organisieren.

Klicken Sie auf *Hierarchien*, und wählen Sie die Option *Hierarchie hinzufügen* aus. Geben Sie `Device Fleet` als Namen ein, und erstellen Sie eine Ebene mit dem Namen `Device Name`. Klicken Sie anschließend auf *Speichern*.

![Hinzufügen einer Hierarchie](./media/tutorial-configure-tsi/add-hierarchy.png)

### <a name="assign-your-instances-to-the-correct-type"></a>Zuweisen Ihrer Instanzen zum richtigen Typ

Als Nächstes ändern Sie den Typ Ihrer Instanzen und ordnen diese optional in der Hierarchie an.

Wählen Sie die Registerkarte *Instanzen* aus, und klicken Sie ganz rechts auf das Symbol *Bearbeiten*.

![Bearbeiten von Instanzen](./media/tutorial-configure-tsi/edit-instance.png)

Klicken Sie auf die Dropdownliste „Typ“, und wählen Sie `Thermostat` aus. 

![Ändern des Instanztyps](./media/tutorial-configure-tsi/change-type.png)

Wenn Sie eine Hierarchie erstellt haben, wählen Sie die Option *Instanzfelder* aus und aktivieren das Kontrollkästchen `Device Fleet`. Geben Sie `Temperature Controller` als Wert des übergeordneten Geräts ein, und klicken Sie anschließend auf *Speichern*.

![Zuweisen zur Hierarchie](./media/tutorial-configure-tsi/assign-to-hierarchy.png)

Wiederholen Sie die obigen Schritte für den zweiten Thermostat.

### <a name="view-your-data"></a>Prüfen der Daten

Navigieren Sie zurück zum Diagrammbereich, und erweitern Sie „Device Fleet“ und „TemperatureController“. Klicken Sie auf „thermostat1“, wählen Sie die Variable `Temperature` aus, und klicken Sie dann auf *Hinzufügen*, um den Wert im Diagramm anzuzeigen. Führen Sie dies auch für „thermostat2“ durch.

![Ändern des Instanztyps für „thermostat2“](./media/tutorial-configure-tsi/charting-values.png)

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zu den verschiedenen Diagrammoptionen, z. B. Intervallgröße und Steuerelemente für die Y-Achse, finden Sie in der Dokumentation zum [Azure Time Series Insights-Explorer](https://docs.microsoft.com/azure/time-series-insights/concepts-ux-panels).

* Eine ausführliche Übersicht über das Zeitreihenmodell Ihrer Umgebung finden Sie in [diesem Artikel](https://docs.microsoft.com/azure/time-series-insights/concepts-model-overview).

* Informationen zu den Abfrage-APIs und zur Syntax für Zeitreihenausdrücke finden Sie [hier](https://docs.microsoft.com/rest/api/time-series-insights/reference-query-apis).




