---
title: Sammeln und Transportieren von Metriken - Azure IoT Edge
description: Verwenden Sie Azure Monitor zur Remoteüberwachung der integrierten Metriken von IoT Edge
author: veyalla
manager: philmea
ms.author: veyalla
ms.date: 06/09/2021
ms.topic: conceptual
ms.reviewer: kgremban
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: b7c849bd00991eb243c9620c1b08bede7f239170
ms.sourcegitcommit: f9e368733d7fca2877d9013ae73a8a63911cb88f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/10/2021
ms.locfileid: "111904389"
---
# <a name="collect-and-transport-metrics-preview"></a>Sammeln und Transportieren von Metriken (Vorschau)

[!INCLUDE [iot-edge-version-all-supported](../../includes/iot-edge-version-all-supported.md)]

Sie können Ihre IoT Edge-Flotte mithilfe von Azure Monitor und den integrierten Metriken remote überwachen. Um diese Funktion auf Ihrem Gerät zu aktivieren, fügen Sie Ihrer Bereitstellung das Modul „Metrikensammler“ hinzu, und konfigurieren Sie es so, dass Modulmetriken gesammelt und an Azure Monitor übertragen werden.

## <a name="architecture"></a>Aufbau

[![Architektur zur Metriküberwachung](./media/how-to-collect-and-transport-metrics/arch.png)](./media/how-to-collect-and-transport-metrics/arch.png#lightbox)

| Hinweis | BESCHREIBUNG |
|-|-|
|  1 | Alle Module müssen mithilfe des [Prometheus-Datenmodells](https://prometheus.io/docs/concepts/data_model/) Metriken ausgeben. [Integrierte Metriken](how-to-access-built-in-metrics.md) ermöglichen zwar standardmäßig eine umfassende Sichtbarkeit von Workloads, aber es können zusätzlich benutzerdefinierte Module verwendet werden, um szenariospezifische Metriken auszugeben und somit die Überwachungslösung zu verbessern. Informationen über das Instrumentieren benutzerdefinierter Module mithilfe von Open-Source-Bibliotheken finden Sie im Artikel [Hinzufügen benutzerdefinierter Metriken](how-to-add-custom-metrics.md). |
|  2️ | Das [Metrikensammler-Modul](https://aka.ms/edgemon-metric-collector) ist ein von Microsoft bereitgestelltes IoT Edge-Modul, das Modulmetriken von Workloads sammelt und diese von den Geräten abtransportiert. Der Metrikensammler verwendet ein *Pullmodell*. Die Sammlungshäufigkeit sowie Endpunkte und Filter können so konfiguriert werden, dass die vom Modul ausgegebenen Daten kontrolliert werden. Weitere Informationen finden Sie im Abschnitt [Konfiguration des Metrikensammlers](#metrics-collector-configuration) weiter unten in diesem Artikel. |
|  3️ | Sie haben zwei Optionen, um Metriken aus dem Metrikensammler-Modul an die Cloud zu senden. *Option 1* sendet die Metriken an Log Analytics. <sup>1</sup> Die gesammelten Metriken werden mithilfe einer festen, nativen Tabelle namens `InsightsMetrics` im angegebenen Log Analytics-Arbeitsbereich erfasst. Das Schema dieser Tabelle ist mit dem Prometheus-Metrikdatenmodell kompatibel.<br><br> Diese Option erfordert Zugriff auf den Arbeitsbereich am ausgehenden Port 443. Die ID und der Schlüssel des Log Analytics-Arbeitsbereichs müssen als Teil der Modulkonfiguration angegeben werden. Informationen über das Aktivieren in eingeschränkten Netzwerken finden Sie weiter unten in diesem Artikel unter [Aktivieren in Szenarien mit eingeschränktem Netzwerkzugriff](#enable-in-restricted-network-access-scenarios).
|  4️ | Jeder Metrikeintrag enthält die `ResourceId`, die als Teil der [Modulkonfiguration](#metrics-collector-configuration) angegeben wurde. Diese Zuordnung verknüpft die Metrik automatisch mit der angegebenen Ressource (z. B. IoT Hub). Auf diese Weise können die [kuratierten IoT Edge-Arbeitsmappenvorlagen](how-to-explore-curated-visualizations.md) Metriken abrufen, indem sie Abfragen für die Ressourcen ausführen. <br><br> Mit diesem Ansatz ist es ebenfalls möglich, dass mehrere IoT Hubs einen einzelnen Log Analytics-Arbeitsbereich als Metrikdatenbank sicher gemeinsam nutzen. |
|  5️ | *Option 2* sendet die Metriken an IoT Hub. <sup>1</sup> Das Sammlermodul kann so konfiguriert werden, dass die gesammelten Metriken über das Modul `edgeHub` als UTF-8-codierte JSON-[Gerät-zu-Cloud-Nachrichten](../iot-hub/iot-hub-devguide-messages-d2c.md) gesendet werden. Diese Option ermöglicht die Überwachung von gesperrten IoT Edge-Geräten, die nur auf den IoT Hub-Endpunkt externen Zugriff erhalten. Außerdem wird auf diese Weise die Überwachung von untergeordneten IoT Edge-Geräten in einer geschachtelten Konfiguration ermöglicht, bei der untergeordnete Geräte nur auf ihr jeweils übergeordnetes Gerät zugreifen können. |
|  6️ | Wenn Metriken über IoT Hub weitergeleitet werden, muss ein (einmaliger) Cloudworkflow eingerichtet werden. Der Workflow verarbeitet Nachrichten, die vom Metrikensammler-Modul eintreffen und sendet diese an den Log Analytics-Arbeitsbereich. Der Workflow ermöglicht die Funktionen [kuratierte Visualisierungen](how-to-explore-curated-visualizations.md) und [Warnungen](how-to-create-alerts.md) auch für Metriken, die über diesen optionalen Pfad eintreffen. Weitere Informationen über das Einrichten dieses Cloudworkflows finden Sie im Abschnitt [Routen von Metriken durch IoT Hub](#route-metrics-through-iot-hub). |

<sup>1</sup> Derzeit ist der Einsatz von *Option 1* für eine direkte Übertragung von Metriken vom IoT Edge-Gerät an Log Analytics der einfachere Weg, bei dem nur ein minimaler Setup-Aufwand erforderlich ist. Die erste Option wird demnach bevorzugt, es sei denn, Ihr spezifisches Szenario erfordert *Option 2*, bei der das IoT Edge-Gerät nur mit IoT Hub kommuniziert.

## <a name="metrics-collector-module"></a>Metrikensammler-Modul

Ein von Microsoft bereitgestelltes Metrikensammler-Modul kann einer IoT Edge-Bereitstellung hinzugefügt werden, um Modulmetriken zu sammeln und an Azure Monitor zu senden. Der Code des Moduls ist ein Open-Source-Code und im [IoT Edge GitHub-Repository](https://github.com/Azure/iotedge/tree/release/1.1/edge-modules/azure-monitor) verfügbar.

Das Metrikensammler-Modul wird als Docker-Containerimage mit mehreren Arches bereitgestellt, das Linux X64, ARM32, ARM64 und Windows X64 (Version 1809) unterstützt. Es ist öffentlich über **[`mcr.microsoft.com/azureiotedge-metrics-collector`](https://aka.ms/edgemon-metrics-collector)** verfügbar.

Es ist ebenfalls im [IoT Edge-Modul-Marketplace](https://aka.ms/edgemon-module-marketplace) verfügbar.

## <a name="metrics-collector-configuration"></a>Konfiguration des Metrikensammlers

Die Konfiguration des Metrikensammlers erfolgt mithilfe von Umgebungsvariablen. Die in der folgenden Tabelle als **Erforderlich** gekennzeichneten Variablen müssen in jedem Fall angegeben werden.

| Name der Umgebungsvariable | BESCHREIBUNG |
|-|-|
| `ResourceId` | Ressourcen-ID des IoT Hubs, mit dem das Gerät kommuniziert. Die Schritte zum Abrufen der Ressourcen-ID finden Sie im Abschnitt [Ressourcen-ID](#resource-id).  <br><br>  **Erforderlich** <br><br> Standardwert: *none* |
| `UploadTarget` |  Steuert, ob Metriken über HTTPS direkt an Azure Monitor oder als D2C-Nachrichten an IoT Hub gesendet werden. Weitere Informationen finden Sie unter [Uploadziel](#upload-target). <br><br>Kann entweder **AzureMonitor** oder **IoTMessage** sein.  <br><br>  **Not required** <br><br> Standardwert: *AzureMonitor* |
| `LogAnalyticsWorkspaceId` | [ID des Log Analytics-Arbeitsbereichs](../azure-monitor/agents/log-analytics-agent.md#workspace-id-and-key) <br><br>Nur **erforderlich**, wenn *UploadTarget* *AzureMonitor* ist <br><br>Standardwert: *none* |
| `LogAnalyticsSharedKey` | [Schlüssel des Log Analytics-Arbeitsbereichs](../azure-monitor/agents/log-analytics-agent.md#workspace-id-and-key) <br><br>Nur **erforderlich**, wenn *UploadTarget* *AzureMonitor* ist   <br><br> Standardwert: *none* |
| `MetricsEndpointsCSV` | Durch Trennzeichen getrennte Liste mit Endpunkten, von denen Prometheus-Metriken gesammelt werden. Alle Modulendpunkte, von denen Metriken gesammelt werden sollen, müssen in dieser Liste aufgeführt werden.<br><br>  Beispiel: *http://edgeAgent:9600/metrics , http://edgeHub:9600/metrics, http://MetricsSpewer:9417/metrics* <br><br>  **Not required** <br><br> Standardwert: *http://edgeHub:9600/metrics , http://edgeAgent:9600/metrics* |
| `AllowedMetrics` | Liste der zu erfassenden Metriken. Alle anderen Metriken werden ignoriert. Legen Sie diese auf eine leere Zeichenfolge fest, wenn Sie sie deaktivieren wollen. Weitere Informationen finden Sie unter [Listen zulassen oder nicht zulassen](#allow-and-disallow-lists). <br><br>Beispiel: *metricToScrape{quantile=0.99}[endpoint= http://MetricsSpewer:9417/metrics ]*<br><br>  **Not required** <br><br> Standardwert: *empty* |
| `BlockedMetrics` | Liste der zu ignorierenden Metriken. Überschreibt *AllowedMetrics*, d. h. dass eine Metrik, die in beiden Listen enthalten ist, nicht gemeldet wird. Weitere Informationen finden Sie unter [Listen zulassen oder nicht zulassen](#allow-and-disallow-lists). <br><br>   Beispiel: *metricToIgnore{quantile=0.5}[endpoint=http://VeryNoisyModule:9001/metrics ], docker_container_disk_write_bytes*<br><br>  **Not required**  <br><br>Standardwert: *empty* |
| `CompressForUpload` | Steuerelemente für die Komprimierung, die beim Hochladen von Metriken verwendet werden sollten. Gilt für alle Uploadziele.<br><br>  Beispiel: *true* <br><br>    **Not required** <br><br>  Standardwert: *true* |

### <a name="resource-id"></a>Ressourcen-ID

Das Metrikensammler-Modul benötigt die Azure Resource Manager-ID des IoT Hubs, zu dem das IoT Edge-Gerät gehört. Geben Sie diese ID als Wert für die **ResourceID**-Umgebungsvariable an.

Die Ressourcen-ID hat das folgende Format:

```input
/subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/Microsoft.Devices/IoTHubs/<iot hub name>
```

Sie finden die Ressourcen-ID auf der Seite **Eigenschaften** des IoT Hubs im Azure-Portal.

:::image type="content" source="./media/how-to-collect-and-transport-metrics/resource-id.png" alt-text="Abrufen der Ressourcen-ID aus den IoT Hub-Eigenschaften.":::

Oder Sie rufen die ID mit dem Befehl [az resource show](/cli/azure/resource#az_resource_show) ab:

```azurecli-interactive
az resource show -g \<group> -n \<name> --resource-type "Microsoft.Devices/IoTHubs"`
```

### <a name="upload-target"></a>Uploadziel

Die Konfigurationsoption **UploadTarget** steuert, ob Metriken direkt an Azure Monitor oder an IoT Hub gesendet werden.

Wenn Sie **UploadTarget** auf **IoTMessage** festlegen, werden Ihre Modulmetriken als IoT-Nachrichten veröffentlicht. Diese Nachrichten werden als UTF8-codierter JSON-Code vom `/messages/modules/<module name>/outputs/metricOutput`-Endpunkt ausgegeben. Das Format sieht folgendermaßen aus:

```json
[{
    "TimeGeneratedUtc": "<time generated>",
    "Name": "<prometheus metric name>",
    "Value": <decimal value>,
    "Label": {
        "<label name>": "<label value>"
    }
}, {
    "TimeGeneratedUtc": "2020-07-28T20:00:43.2770247Z",
    "Name": "docker_container_disk_write_bytes",
    "Value": 0.0,
    "Label": {
        "name": "AzureMonitorForIotEdgeModule"
    }
}]
```

### <a name="allow-and-disallow-lists"></a>Listen zulassen oder nicht zulassen

Die Konfigurationsoptionen `AllowedMetrics` und `BlockedMetrics` enthalten durch Leerzeichen oder Komma getrennte Listen mit Metrikselektoren. Wenn eine Metrik mit der Liste übereinstimmt, wird sie einbezogen. Wenn sie mit einer oder mehreren Metriken in einer der Listen übereinstimmt, wird sie ausgeschlossen.

Metrikselektoren verwenden ein Format, das einer Untergruppe der [PromQL](https://prometheus.io/docs/prometheus/latest/querying/basics/)-Abfragesprache ähnelt.

```query
metricToSelect{quantile=0.5,otherLabel=~Re[ge]*|x}[http://VeryNoisyModule:9001/metrics]
```

Metrikselektoren bestehen aus drei Teilen:

Metrikname (`metricToSelect`).

* Platzhalter `*` (beliebige Zeichen) und `?` (beliebiges einzelnes Zeichen) können in Metriknamen verwendet werden. Z. B. würde `*CPU` mit `maxCPU` und `minCPU` übereinstimmen, aber nicht mit `CPUMaximum`. `???CPU` würde mit `maxCPU` und `minCPU` übereinstimmen, aber nicht mit `maximumCPU`.
* Diese Komponente ist bei einem Metrikselektor erforderlich.

Bezeichnungsbasierte Selektoren (`{quantile=0.5,otherLabel=~Re[ge]*|x}`).

* In den geschweiften Klammern können mehrere Metrikwerte enthalten sein. Die Werte sollten durch Trennzeichen getrennt werden.
* Eine Metrik wird entsprechend zugeordnet, wenn alle Bezeichnungen des Selektors vorhanden sind und übereinstimmen.
* Wie bei PromQL sind die folgenden Operatoren für den Abgleich zulässig.
  * `=` Abgleich von Bezeichnungen, die genau mit der angegebenen Zeichenfolge übereinstimmen (Groß- und Kleinschreibung wird beachtet).
  * `!=` Abgleich von Bezeichnungen, die nicht genau mit der angegebenen Zeichenfolge übereinstimmen.
  * `=~` Abgleich von Bezeichnungen mit einem bereitgestellten RegEx. Bsp.: `label=~CPU|Mem|[0-9]*`
  * `!=` Abgleich von Bezeichnungen, die nicht zu einem bereitgestellten RegEx passen.
  * RegEx ist vollständig verankert (A ^ und $ werden automatisch am Anfang und am Ende jedes RegEx hinzugefügt)
  * Diese Komponente ist bei einem Metrikselektor optional.

Endpunktselektor (`[http://VeryNoisyModule:9001/metrics]`).

* Die URL sollte genau mit einer URL übereinstimmen, die in `MetricsEndpointsCSV` aufgelistet ist.
* Diese Komponente ist bei einem Metrikselektor optional.

Eine Metrik muss mit allen Teilen eines jeweiligen Selektors übereinstimmen, um ausgewählt zu werden. Sie muss mit dem Namen übereinstimmen *und* über dieselben Tags mit übereinstimmenden Werten verfügen *und* vom angegebenen Endpunkt stammen. `mem{quantile=0.5,otherLabel=foobar}[http://VeryNoisyModule:9001/metrics]` würde beispielsweise nicht mit dem Selektor `mem{quantile=0.5,otherLabel=~foo|bar}[http://VeryNoisyModule:9001/metrics]` übereinstimmen. Es sollten mehrere Selektoren verwendet werden, damit die Auswahl nicht eingeschränkt (AND-Operatoren) sondern erweitert wird (OR-Operatoren).

Um z. B. die Metrik `mem` aus einem Modul `module1` unabhängig von den zugehörigen Tags zuzulassen, aber die gleiche Metrik aus `module2` nur mit dem Tag `agg=p99` zuzulassen, kann der folgende Selektor zu `AllowedMetrics` hinzugefügt werden:

```query
mem{}[http://module1:9001/metrics] mem{agg="p99"}[http://module2:9001/metrics]
```

Oder fügen Sie Folgendes zu `AllowedMetrics` hinzu, um die Metriken `mem` und `cpu` unabhängig von den Tags oder Endpunkten zuzulassen:

```query
mem cpu
```

## <a name="enable-in-restricted-network-access-scenarios"></a>Aktivieren in Szenarien mit eingeschränktem Netzwerkzugriff

Wenn Sie Metriken direkt an den Log Analytics-Arbeitsbereich senden, lassen Sie einen ausgehenden Zugriff auf die folgenden URLs zu:

* `https://<LOG_ANALYTICS_WORKSPACE_ID>.ods.opinsights.azure.com/*`
* `https://<LOG_ANALYTICS_WORKSPACE_ID>.oms.opinsights.azure.com/*`

### <a name="proxy-considerations"></a>Proxy-Aspekte

Das Metrikensammler-Modul ist in .NET Core geschrieben. Verwenden Sie daher die gleiche Anleitung wie für Systemmodule, um [die Kommunikation über einen Proxyserver zu ermöglichen](how-to-configure-proxy-support.md#configure-deployment-manifests).

Für die Metriksammlung aus lokalen Modulen wird das HTTP-Protokoll verwendet. Schließen Sie eine lokale Kommunikation vom Proxyserver aus, indem Sie die `NO_PROXY`-Umgebungsvariable einstellen.

Legen Sie den `NO_PROXY`-Wert auf eine durch Trennzeichen getrennte Liste von Hostnamen fest, die ausgeschlossen werden sollen. Verwenden Sie Modulnamen als Hostnamen. Beispiel: *edgeHub,edgeAgent,myCustomModule*.

## <a name="route-metrics-through-iot-hub"></a>Routen von Metriken durch IoT Hub

Manchmal ist es erforderlich, Metriken in IoT Hub zu erfassen, anstatt sie direkt an Log Analytics zu senden. Dies ist z. B. der Fall, wenn Sie [IoT Edge-Geräte in einer geschachtelten Konfiguration](tutorial-nested-iot-edge.md) überwachen, bei der untergeordnete Geräte nur Zugriff auf den IoT Edge-Hub ihres übergeordneten Geräts haben. Ein weiteres Beispiel ist die Bereitstellung eines IoT Edge-Geräts, das nur über einen ausgehenden Netzwerkzugriff auf IoT Hub verfügt.

Um die Überwachung in diesem Szenario zu ermöglichen, kann das Metrikensammler-Modul so konfiguriert werden, dass Metriken als D2C-Nachrichten (Device-to-Cloud) über das edgeHub-Modul gesendet werden. Die Funktion wird aktiviert, indem die `UploadTarget`-Umgebungsvariable bei der [Konfiguration](#metrics-collector-configuration) des Sammlers auf `IoTMessage`festgelegt wird.

>[!TIP]
>Denken Sie daran, eine edgeHub-Route hinzuzufügen, um Metriknachrichten aus dem Sammlermodul an IoT Hub zu übermitteln. Dieser entspricht dem Format `FROM /messages/modules/replace-with-collector-module-name/* INTO $upstream`.

Für diese Option ist eine zusätzliche Einstellung erforderlich, damit Metriknachrichten, die bei IoT Hub eingehen, an den Log Analytics-Arbeitsbereich gesendet werden können. Ohne diese Einstellung werden die anderen Bestandteile der Integration wie [kuratierte Visualisierungen](how-to-explore-curated-visualizations.md) und [Warnungen](how-to-create-alerts.md) nicht funktionieren.

>[!NOTE]
>Bei dieser Option treten zusätzliche Kosten auf. Metriknachrichten werden auf Ihr IoT Hub-Nachrichtenkontingent angerechnet. Außerdem werden Ihnen die Log Analytics-Erfassung und die Cloudworkflowressourcen in Rechnung gestellt.

### <a name="sample-cloud-workflow"></a>Beispiel für einen Cloudworkflow

Ein Cloudworkflow, der Metriknachrichten von IoT Hub an Log Analytics übermittelt, ist als Bestandteil des [IoT Edge-Protokollierungs- und -Überwachungbeispiels](https://github.com/Azure-Samples/iotedge-logging-and-monitoring-solution#monitoring-architecture-reference) verfügbar. Das Beispiel kann in vorhandenen Cloudressourcen bereitgestellt werden oder als Referenz für eine Produktionsbereitstellung dienen.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über die verschiedenen Arten von [kuratierten Visualisierungen](how-to-explore-curated-visualizations.md), die mit Azure Monitor möglich sind.
