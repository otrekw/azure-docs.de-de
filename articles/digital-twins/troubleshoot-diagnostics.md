---
title: Einrichten der Diagnose
titleSuffix: Azure Digital Twins
description: Erfahren Sie, wie Sie die Protokollierung mit Diagnoseeinstellungen aktivieren.
author: baanders
ms.author: baanders
ms.date: 7/28/2020
ms.topic: troubleshooting
ms.service: digital-twins
ms.openlocfilehash: f4abf78c153bd3d61068e4b7607794d6ccf1ed04
ms.sourcegitcommit: 2e72661f4853cd42bb4f0b2ded4271b22dc10a52
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/14/2020
ms.locfileid: "92047674"
---
# <a name="troubleshooting-azure-digital-twins-diagnostics-logging"></a>Problembehandlung von Azure Digital Twins: Diagnoseprotokollierung

Azure Digital Twins sammelt [Metriken](troubleshoot-metrics.md) für Ihre Dienstinstanz, die Informationen zum Zustand Ihrer Ressourcen bereitstellen. Mit diesen Metriken können Sie die allgemeine Integrität des Azure Digital Twins-Diensts und der damit verbundenen Ressourcen bewerten. Anhand dieser benutzerorientierten Statistiken können Sie Vorgänge in Azure Digital Twins erkennen und die Ursache von Problemen analysieren, ohne sich an den Azure-Support wenden zu müssen.

In diesem Artikel erfahren Sie, wie Sie die **Diagnoseprotokollierung** für Ihre Metrikdaten von Ihrer Azure Digital Twins-Instanz aus aktivieren. Sie können diese Protokolle verwenden, um Probleme mit dem Dienst zu beheben, und Diagnoseeinstellungen konfigurieren, um Metriken von Azure Digital Twins an verschiedene Ziele zu senden. Weitere Informationen zu diesen Einstellungen finden Sie unter [*Erstellen von Diagnoseeinstellungen zum Senden von Plattformprotokollen und Metriken an verschiedene Ziele*](../azure-monitor/platform/diagnostic-settings.md).

## <a name="turn-on-diagnostic-settings-with-the-azure-portal"></a>Aktivieren der Diagnoseeinstellungen über das Azure-Portal

Im Folgenden wird beschrieben, wie Sie Diagnoseeinstellungen für Ihre Azure Digital Twins-Instanz aktivieren:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an, und navigieren Sie zu Ihrer Azure Digital Twins-Instanz. Sie finden sie, indem Sie ihren Namen auf der Suchleiste des Portals eingeben. 

2. Wählen Sie im Menü die Option **Diagnoseeinstellungen** und dann **Diagnoseeinstellung hinzufügen** aus.

    :::image type="content" source="media/troubleshoot-diagnostics/diagnostic-settings.png" alt-text="Screenshot der Seite „Diagnoseeinstellungen“ und der Schaltfläche zum Hinzufügen":::

3. Geben Sie auf der daraufhin geöffneten Seite die folgenden Werte ein:
     * **Name der Diagnoseeinstellung:** Benennen Sie die Diagnoseeinstellungen.
     * **Kategoriedetails:** Wählen Sie aus, welche Vorgänge Sie überwachen möchten, und aktivieren Sie die Kontrollkästchen für die Diagnose dieser Vorgänge. Die Vorgänge, für die über die Diagnoseeinstellungen Berichte erstellt werden können, sind die folgenden:
        - DigitalTwinsOperation
        - EventRoutesOperation
        - ModelsOperation
        - QueryOperation
        - AllMetrics
        
        Weitere Informationen zu diesen Optionen finden Sie im Abschnitt [*Kategoriedetails*](#category-details) weiter unten.
     * **Zieldetails:** Wählen Sie aus, wohin die Protokolle gesendet werden sollen. Sie können eine beliebige Kombination der drei Optionen auswählen:
        - An Log Analytics senden
        - In einem Speicherkonto archivieren
        - An einen Event Hub streamen

        Möglicherweise werden Sie aufgefordert, weitere Details einzugeben, wenn diese für Ihr ausgewähltes Ziel erforderlich sind.  
    
4. Speichern Sie die neuen Einstellungen. 

    :::image type="content" source="media/troubleshoot-diagnostics/diagnostic-settings-details.png" alt-text="Screenshot der Seite „Diagnoseeinstellungen“ und der Schaltfläche zum Hinzufügen":::

Neue Einstellungen werden in etwa zehn Minuten wirksam. Anschließend werden die Protokolle im konfigurierten Ziel auf der Seite **Diagnoseeinstellungen** für Ihre Instanz angezeigt. 

## <a name="category-details"></a>Kategoriedetails

Hier finden Sie weitere Informationen zu den Protokollkategorien, die beim Einrichten der Diagnoseeinstellungen unter **Kategoriedetails** ausgewählt werden können.

| Protokollkategorie | BESCHREIBUNG |
| --- | --- |
| ADTModelsOperation | Protokolliert alle API-Aufrufe im Zusammenhang mit Modellen. |
| ADTQueryOperation | Protokolliert alle API-Aufrufe im Zusammenhang mit Abfragen. |
| ADTEventRoutesOperation | Protokolliert alle API-Aufrufe im Zusammenhang mit Ereignisrouten sowie ausgehende Ereignisse von Azure Digital Twins an einen Endpunktdienst wie Event Grid, Event Hubs und Service Bus. |
| ADTDigitalTwinsOperation | Protokolliert alle API-Aufrufe im Zusammenhang mit Azure Digital Twins. |

Jede Protokollkategorie besteht aus Schreib-, Lese-, Lösch- und Aktionsvorgängen.  Diese lassen sich REST-API-Aufrufen wie folgt zuordnen:

| Ereignistyp | REST-API-Vorgänge |
| --- | --- |
| Schreiben | PUT und PATCH |
| Lesen | GET |
| Löschen | Delete |
| Aktion | POST |

Im Folgenden finden Sie eine umfassende Liste der Vorgänge und entsprechenden [REST-API-Aufrufe für Azure Digital Twins](/rest/api/azure-digitaltwins/), die in jeder Kategorie protokolliert werden. 

>[!NOTE]
> Jede Protokollkategorie enthält mehrere Vorgänge/REST-API-Aufrufe. In der folgenden Tabelle ist jede Protokollkategorie allen darunter aufgeführten Vorgänge/REST-API-Aufrufen zugeordnet, anschließend folgt die nächste Kategorie. 

| Protokollkategorie | Vorgang | REST-API-Aufrufe und andere Ereignisse |
| --- | --- | --- |
| ADTModelsOperation | Microsoft.DigitalTwins/models/write | API zum Aktualisieren von Digital Twin-Modellen |
|  | Microsoft.DigitalTwins/models/read | APIs zum Abrufen nach ID und Auflisten von Digital Twin-Modellen |
|  | Microsoft.DigitalTwins/models/delete | API zum Löschen von Digital Twin-Modellen |
|  | Microsoft.DigitalTwins/models/action | API zum Hinzufügen von Digital Twin-Modellen |
| ADTQueryOperation | Microsoft.DigitalTwins/query/action | API zum Abfragen von Digital Twin-Instanzen |
| ADTEventRoutesOperation | Microsoft.DigitalTwins/eventroutes/write | API zum Hinzufügen von Ereignisrouten |
|  | Microsoft.DigitalTwins/eventroutes/read | APIs zum Abrufen nach ID und Auflisten von Ereignisrouten |
|  | Microsoft.DigitalTwins/eventroutes/delete | API zum Löschen von Ereignisrouten |
|  | Microsoft.DigitalTwins/eventroutes/action | Fehler beim Veröffentlichen von Ereignissen in einem Endpunktdienst (kein API-Aufruf). |
| ADTDigitalTwinsOperation | Microsoft.DigitalTwins/digitaltwins/write | Vorgänge zum Hinzufügen, Hinzufügen von Beziehungen, Aktualisieren und Aktualisieren von Komponenten von Digital Twins |
|  | Microsoft.DigitalTwins/digitaltwins/read | Vorgänge zum Abrufen nach ID, Abrufen von Komponenten, Abrufen der Beziehung nach ID, Auflisten eingehender Beziehungen und Auflisten von Beziehungen von Digital Twins |
|  | Microsoft.DigitalTwins/digitaltwins/delete | Vorgänge zum Löschen und Löschen von Beziehungen von Digital Twins |
|  | Microsoft.DigitalTwins/digitaltwins/action | Senden von Komponententelemetrie und Senden von Telemetrie von Digital Twins |

## <a name="log-schemas"></a>Protokollschemas 

Jede Protokollkategorie verfügt über ein Schema, das definiert, wie Ereignisse in dieser Kategorie gemeldet werden. Jeder einzelne Protokolleintrag wird als Text gespeichert und als JSON-Blob formatiert. Die Felder im Protokoll und die JSON-Beispieltexte werden für jeden unten angegebenen Protokolltyp bereitgestellt. 

`ADTDigitalTwinsOperation`, `ADTModelsOperation` und `ADTQueryOperation` verwenden ein konsistentes API-Protokollschema. `ADTEventRoutesOperation` verfügt über ein eigenes separates Schema.

### <a name="api-log-schemas"></a>API-Protokollschemas

Dieses Protokollschema ist für `ADTDigitalTwinsOperation`, `ADTModelsOperation` und `ADTQueryOperation` konsistent. Es enthält Informationen, die für API-Aufrufe an eine Azure Digital Twins-Instanz relevant sind.

Im Folgenden finden Sie die Feld- und Eigenschaftsbeschreibungen für API-Protokolle.

| Feldname | Datentyp | BESCHREIBUNG |
|-----|------|-------------|
| `Time` | Datetime | Datum und Uhrzeit des Auftretens dieses Ereignisses in UTC |
| `ResourceID` | String | Azure Resource Manager-Ressourcen-ID für die Ressource, in der das Ereignis aufgetreten ist |
| `OperationName` | String  | Typ der Aktion, die während des Ereignisses ausgeführt wurde |
| `OperationVersion` | String | Während des Ereignisses verwendete API-Version |
| `Category` | String | Typ der auszugebenden Ressource |
| `ResultType` | String | Ergebnis des Ereignisses |
| `ResultSignature` | String | HTTP-Statuscode für das Ereignis |
| `ResultDescription` | String | Weitere Details zum Ereignis |
| `DurationMs` | String | Zeit für die Ausführung des Ereignisses in Millisekunden |
| `CallerIpAddress` | String | Maskierte Quell-IP-Adresse für das Ereignis |
| `CorrelationId` | Guid | Vom Kunden angegebener eindeutiger Bezeichner für das Ereignis |
| `Level` | String | Protokollschweregrad des Ereignisses |
| `Location` | String | Region, in der das Ereignis aufgetreten ist |
| `RequestUri` | Uri | Während des Ereignisses verwendeter Endpunkt |

Im Folgenden finden Sie Beispiel-JSON-Texte für diese Arten von Protokollen.

#### <a name="adtdigitaltwinsoperation"></a>ADTDigitalTwinsOperation

```json
{
  "time": "2020-03-14T21:11:14.9918922Z",
  "resourceId": "/SUBSCRIPTIONS/BBED119E-28B8-454D-B25E-C990C9430C8F/RESOURCEGROUPS/MYRESOURCEGROUP/PROVIDERS/MICROSOFT.DIGITALTWINS/DIGITALTWINSINSTANCES/MYINSTANCENAME",
  "operationName": "Microsoft.DigitalTwins/digitaltwins/write",
  "operationVersion": "2020-05-31-preview",
  "category": "DigitalTwinOperation",
  "resultType": "Success",
  "resultSignature": "200",
  "resultDescription": "",
  "durationMs": "314",
  "callerIpAddress": "13.68.244.*",
  "correlationId": "2f6a8e64-94aa-492a-bc31-16b9f0b16ab3",
  "level": "4",
  "location": "southcentralus",
  "uri": "https://myinstancename.api.scus.digitaltwins.azure.net/digitaltwins/factory-58d81613-2e54-4faa-a930-d980e6e2a884?api-version=2020-05-31-preview"
}
```

#### <a name="adtmodelsoperation"></a>ADTModelsOperation

```json
{
  "time": "2020-10-29T21:12:24.2337302Z",
  "resourceId": "/SUBSCRIPTIONS/BBED119E-28B8-454D-B25E-C990C9430C8F/RESOURCEGROUPS/MYRESOURCEGROUP/PROVIDERS/MICROSOFT.DIGITALTWINS/DIGITALTWINSINSTANCES/MYINSTANCENAME",
  "operationName": "Microsoft.DigitalTwins/models/write",
  "operationVersion": "2020-05-31-preview",
  "category": "ModelsOperation",
  "resultType": "Success",
  "resultSignature": "201",
  "resultDescription": "",
  "durationMs": "935",
  "callerIpAddress": "13.68.244.*",
  "correlationId": "9dcb71ea-bb6f-46f2-ab70-78b80db76882",
  "level": "4",
  "location": "southcentralus",
  "uri": "https://myinstancename.api.scus.digitaltwins.azure.net/Models?api-version=2020-05-31-preview",
}
```

#### <a name="adtqueryoperation"></a>ADTQueryOperation

```json
{
  "time": "2020-12-04T21:11:44.1690031Z",
  "resourceId": "/SUBSCRIPTIONS/BBED119E-28B8-454D-B25E-C990C9430C8F/RESOURCEGROUPS/MYRESOURCEGROUP/PROVIDERS/MICROSOFT.DIGITALTWINS/DIGITALTWINSINSTANCES/MYINSTANCENAME",
  "operationName": "Microsoft.DigitalTwins/query/action",
  "operationVersion": "2020-05-31-preview",
  "category": "QueryOperation",
  "resultType": "Success",
  "resultSignature": "200",
  "resultDescription": "",
  "durationMs": "255",
  "callerIpAddress": "13.68.244.*",
  "correlationId": "1ee2b6e9-3af4-4873-8c7c-1a698b9ac334",
  "level": "4",
  "location": "southcentralus",
  "uri": "https://myinstancename.api.scus.digitaltwins.azure.net/query?api-version=2020-05-31-preview",
}
```

### <a name="egress-log-schemas"></a>Ausgangsprotokollschemas

Dies ist das Schema für `ADTEventRoutesOperation`-Protokolle. Diese enthalten Details zu Ausnahmen und den API-Vorgängen für Ausgangsendpunkte, die mit einer Instanz von Azure Digital Twins verbunden sind.

|Feldname | Datentyp | BESCHREIBUNG |
|-----|------|-------------|
| `Time` | Datetime | Datum und Uhrzeit des Auftretens dieses Ereignisses in UTC |
| `ResourceId` | String | Azure Resource Manager-Ressourcen-ID für die Ressource, in der das Ereignis aufgetreten ist |
| `OperationName` | String  | Typ der Aktion, die während des Ereignisses ausgeführt wurde |
| `Category` | String | Typ der auszugebenden Ressource |
| `ResultDescription` | String | Weitere Details zum Ereignis |
| `Level` | String | Protokollschweregrad des Ereignisses |
| `Location` | String | Region, in der das Ereignis aufgetreten ist |
| `EndpointName` | String | Name des Ausgangsendpunkts, der in Azure Digital Twins erstellt wurde |

Im Folgenden finden Sie Beispiel-JSON-Texte für diese Arten von Protokollen.

#### <a name="adteventroutesoperation"></a>ADTEventRoutesOperation

```json
{
  "time": "2020-11-05T22:18:38.0708705Z",
  "resourceId": "/SUBSCRIPTIONS/BBED119E-28B8-454D-B25E-C990C9430C8F/RESOURCEGROUPS/MYRESOURCEGROUP/PROVIDERS/MICROSOFT.DIGITALTWINS/DIGITALTWINSINSTANCES/MYINSTANCENAME",
  "operationName": "Microsoft.DigitalTwins/eventroutes/action",
  "category": "EventRoutesOperation",
  "resultDescription": "Unable to send EventGrid message to [my-event-grid.westus-1.eventgrid.azure.net] for event Id [f6f45831-55d0-408b-8366-058e81ca6089].",
  "correlationId": "7f73ab45-14c0-491f-a834-0827dbbf7f8e",
  "level": "3",
  "location": "southcentralus",
  "properties": {
    "endpointName": "endpointEventGridInvalidKey"
  }
}
```

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zum Konfigurieren von Diagnosen finden Sie unter [*Erfassen und Nutzen von Protokolldaten aus Ihren Azure-Ressourcen*](../azure-monitor/platform/platform-logs-overview.md).
* Informationen zu den Azure Digital Twins-Metriken finden Sie unter [*Problembehandlung: Anzeigen von Metriken mit Azure Monitor*](troubleshoot-metrics.md).
* Informationen zur Aktivierung von Warnungen für Ihre Metriken finden Sie unter [*Problembehandlung: Einrichten von Warnungen*](troubleshoot-alerts.md).