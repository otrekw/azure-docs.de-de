---
title: 'Telemetrieprozessoren (Vorschau): Azure Monitor Application Insights für Java'
description: Konfigurieren von Telemetrieprozessoren in Azure Monitor Application Insights für Java
ms.topic: conceptual
ms.date: 10/29/2020
author: kryalama
ms.custom: devx-track-java
ms.author: kryalama
ms.openlocfilehash: ba4e6b8b5e9db494ab4c0c372c2086087a2d58cb
ms.sourcegitcommit: 431bf5709b433bb12ab1f2e591f1f61f6d87f66c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/12/2021
ms.locfileid: "98133173"
---
# <a name="telemetry-processors-preview---azure-monitor-application-insights-for-java"></a>Telemetrieprozessoren (Vorschau): Azure Monitor Application Insights für Java

> [!NOTE]
> Dieses Feature befindet sich noch in der Vorschauphase.

Der Java 3.0-Agent für Application Insights verfügt jetzt über Funktionen, um Telemetriedaten zu verarbeiten, bevor die Daten exportiert werden.

Hier einige Anwendungsfälle für Telemetrieprozessoren:
 * Maskieren vertraulicher Daten
 * Bedingtes Hinzufügen benutzerdefinierter Dimensionen
 * Aktualisieren des Telemetrienamens für Aggregation und Anzeige
 * Verwerfen oder Filtern von span-Attributen zur Senkung von Erfassungskosten

## <a name="terminology"></a>Begriff

Bevor wir uns mit Telemetrieprozessoren beschäftigen, müssen Sie verstehen, was trace-Elemente (Ablaufverfolgungen) und span-Elemente (Spannen) sind.

### <a name="traces"></a>Ablaufverfolgungen

`trace`-Elemente (Ablaufverfolgungen) verfolgen den Status einer einzelnen Anforderung nach, wenn diese durch die Dienste verarbeitet wird, aus denen eine Anwendung besteht. Die Anforderung kann dabei von einem Benutzer oder einer Anwendung initiiert werden. Jede Arbeitseinheit in einem `trace`-Element wird als `span`-Element bezeichnet; ein `trace`-Element ist eine Struktur aus span-Elementen. Ein `trace`-Element besteht aus einem einzelnen span-Element auf Stammebene und einer beliebigen Anzahl von untergeordneten span-Elementen.

### <a name="span"></a>span

span-Element repräsentieren die Aufgaben einzelner Dienste oder Komponenten die an einer Anforderung beteiligt sind, während diese ein System durchläuft. Ein `span`-Element enthält einen `span context`, eine Reihe von global eindeutigen Bezeichnern, die die eindeutige Anforderung repräsentieren, zu der jedes span-Element gehört. 

span-Element kapseln Folgendes:

* Den Namen des span-Elements
* Einen unveränderlichen `SpanContext`, der das span-Element eindeutig identifiziert
* Ein übergeordnetes span-Element in folgender Form: `Span`, `SpanContext` oder NULL
* Eine `SpanKind`
* Einen Zeitstempel für den Start
* Einen Zeitstempel für das Ende
* [`Attributes`](#attributes)
* Eine Liste von Ereignissen mit Zeitstempel
* Ein `Status`.

Im Allgemeinen ähnelt der Lebenszyklus eines span-Elements folgendem Ablauf:

* Ein Dienst empfängt eine Anforderung. Der span-Kontext wird aus den Anforderungsheadern extrahiert, sofern vorhanden.
* Ein neues span-Element wird als untergeordnetes Element des extrahierten span-Kontexts erstellt. Wenn kein solcher Kontext vorhanden ist, wird ein neues span-Element auf Stammebene erstellt.
* Der Dienst verarbeitet die Anforderung. Dem span-Element werden weitere Attribute und Ereignisse hinzugefügt, die für das Verständnis des Anforderungskontexts nützlich sind. Hierzu gehören beispielsweise der Hostname des Computers, der die Anforderung verarbeitet, und Kundenbezeichner.
* Möglicherweise werden neue span-Elemente erstellt, um Aufgaben zu repräsentieren, die von Unterkomponenten des Diensts ausgeführt werden.
* Wenn der Dienst einen Remoteaufruf an einen anderen Dienst sendet, wird der aktuelle span-Kontext serialisiert und an den nächsten Dienst weitergeleitet, indem der span-Kontext in die Header oder den Nachrichtenumschlag eingefügt wird.
* Die Aufgaben des Diensts werden erfolgreich oder nicht erfolgreich abgeschlossen. Der span-Status wird entsprechend festgelegt, und das span-Element wird als abgeschlossen markiert.

### <a name="attributes"></a>Attribute

`Attributes` sind eine Liste aus null, einem oder mehreren Schlüssel-Wert-Paaren, die in einem `span`-Element gekapselt sind. Ein Attribut MUSS die folgenden Eigenschaften besitzen:

Den Attributschlüssel, bei dem es sich um eine nicht leere Nicht-NULL-Zeichenfolge handeln MUSS.
Den Attributwert, der Folgendes sein kann:
* Ein primitiver Typ: Zeichenfolge, boolescher Wert, Gleitkommawert mit doppelter Genauigkeit (IEEE 754-1985) oder 64-Bit-Ganzzahl mit Vorzeichen.
* Ein Array aus Werten mit primitivem Typ. Das Array MUSS homogen sein, es DARF also KEINE Werte mit unterschiedlichen Typen enthalten. Bei Protokollen, die Arraywerte nicht nativ unterstützen, MÜSSEN solche Werte als JSON-Zeichenfolgen dargestellt werden.

## <a name="supported-processors"></a>Unterstützte Prozessoren:
 * Attributprozessor
 * Span-Prozessor

## <a name="to-get-started"></a>Einführung

Erstellen Sie eine Konfigurationsdatei mit dem Namen `applicationinsights.json`, und legen Sie sie im gleichen Verzeichnis wie `applicationinsights-agent-***.jar` mit folgender Vorlage ab.

```json
{
  "connectionString": "InstrumentationKey=00000000-0000-0000-0000-000000000000",
  "preview": {
    "processors": [
      {
        "type": "attribute",
        ...
      },
      {
        "type": "attribute",
        ...
      },
      {
        "type": "span",
        ...
      }
    ]
  }
}
```

## <a name="includeexclude-spans"></a>Einschließen/Ausschließen von span-Elementen

Der Attributprozessor und der span-Prozessor machen die Option verfügbar, einen Satz von Eigenschaften eines span-Elements zum Vergleich bereitzustellen. So lässt sich bestimmen, ob das span-Element in den Prozessor eingeschlossen oder davon ausgeschlossen werden sollte. Um diese Option zu konfigurieren, ist unter `include` und/oder `exclude` mindestens ein `matchType` und ein Exemplar von `spanNames` oder `attributes` erforderlich. Die Einschluss/Ausschluss-Konfiguration wird unterstützt, damit mehrere angegebene Bedingungen vorliegen. Alle angegebenen Bedingungen müssen als „true“ ausgewertet werden, damit eine Entsprechung auftritt. 

**Pflichtfeld**: 
* `matchType` steuert, wie Elemente in den `spanNames`- und `attributes`-Arrays interpretiert werden. Mögliche Werte sind `regexp` oder `strict`. 

**Optionale Felder**: 
* `spanNames` muss mindestens einem der Elemente entsprechen. 
* `attributes` gibt die Liste der Attribute für den Vergleich an. Voraussetzung einer Übereinstimmung ist, dass alle diese Attribute genau zutreffen.

> [!NOTE]
> Wenn sowohl `include` als auch `exclude` angegeben wird, werden die `include`-Eigenschaften vor den `exclude`-Eigenschaften geprüft.

#### <a name="sample-usage"></a>Beispielverwendung

```json

"processors": [
  {
    "type": "attribute",
    "include": {
      "matchType": "strict",
      "spanNames": [
        "spanA",
        "spanB"
      ]
    },
    "exclude": {
      "matchType": "strict",
      "attributes": [
        {
          "key": "redact_trace",
          "value": "false"
        }
      ]
    },
    "actions": [
      {
        "key": "credit_card",
        "action": "delete"
      },
      {
        "key": "duplicate_key",
        "action": "delete"
      }
    ]
  }
]
```
Weitere Informationen finden Sie in der Dokumentation mit [Beispielen für Telemetrieprozessoren](./java-standalone-telemetry-processors-examples.md).

## <a name="attribute-processor"></a>Attributprozessor 

Der Attributprozessor ändert Attribute einer Span. Er unterstützt optional die Möglichkeit zum Einschließen/Ausschließen von Spans. Eine Liste von Aktionen ist erforderlich, deren Ausführungsreihenfolge in der Konfigurationsdatei angegeben wird. Die unterstützten Aktionen sind:

### `insert`

Fügt ein neues Attribut in span-Elemente ein, wenn der Schlüssel noch nicht vorhanden ist.   

```json
"processors": [
  {
    "type": "attribute",
    "actions": [
      {
        "key": "attribute1",
        "value": "value1",
        "action": "insert"
      },
    ]
  }
]
```
Für die `insert`-Aktion wird Folgendes benötigt:
  * `key`
  * Entweder `value` oder `fromAttribute`
  * `action`:`insert`

### `update`

Aktualisiert ein Attribut in Spans, wo der Schlüssel vorhanden ist.

```json
"processors": [
  {
    "type": "attribute",
    "actions": [
      {
        "key": "attribute1",
        "value": "newValue",
        "action": "update"
      },
    ]
  }
]
```
Für die `update`-Aktion wird Folgendes benötigt:
  * `key`
  * Entweder `value` oder `fromAttribute`
  * `action`:`update`


### `delete` 

Löscht ein Attribut aus einer Span.

```json
"processors": [
  {
    "type": "attribute",
    "actions": [
      {
        "key": "attribute1",
        "action": "delete"
      },
    ]
  }
]
```
Für die `delete`-Aktion wird Folgendes benötigt:
  * `key`
  * `action`: `delete`

### `hash`

Versieht einen vorhandenen Attributwert mit einem Hash (SHA1).

```json
"processors": [
  {
    "type": "attribute",
    "actions": [
      {
        "key": "attribute1",
        "action": "hash"
      },
    ]
  }
]
```
Für die `hash`-Aktion wird Folgendes benötigt:
* `key`
* `action` : `hash`

### `extract`

> [!NOTE]
> Dieses Feature ist erst ab Version 3.0.1 verfügbar.

Hiermit werden Werte mithilfe einer regEx-Regel aus dem Eingabeschlüssel in den in der Regel angegebenen Zielschlüssel extrahiert. Wenn ein Zielschlüssel bereits vorhanden ist, wird er überschrieben. Dieses Attribut verhält sich ähnlich wie die `toAttributes`-Einstellung des [span-Prozessors](#extract-attributes-from-span-name), wobei das vorhandene Attribut als Quelle dient.

```json
"processors": [
  {
    "type": "attribute",
    "actions": [
      {
        "key": "attribute1",
        "pattern": "<regular pattern with named matchers>",
        "action": "extract"
      },
    ]
  }
]
```
Für die `extract`-Aktion wird Folgendes benötigt:
* `key`
* `pattern`
* `action` : `extract`

Weitere Informationen finden Sie in der Dokumentation mit [Beispielen für Telemetrieprozessoren](./java-standalone-telemetry-processors-examples.md).

## <a name="span-processors"></a>Span-Prozessoren

Der Span-Prozessor ändert entweder den Span-Namen oder die Attribute einer Span basierend auf dem Span-Namen. Er unterstützt optional die Möglichkeit zum Einschließen/Ausschließen von Spans.

### <a name="name-a-span"></a>Name einer Span

Die folgende Einstellung ist als Teil des Namensabschnitts erforderlich:

* `fromAttributes`: Der Attributwert für die Schlüssel wird verwendet, um einen neuen Namen in der in der Konfiguration angegebenen Reihenfolge zu erstellen. Alle Attributschlüssel müssen in der Span angegeben werden, damit der Prozessor sie umbenennen kann.

Die folgende Einstellung kann optional konfiguriert werden:

* `separator`: Eine angegebene Zeichenfolge, die verwendet wird, um Werte aufzuteilen.
> [!NOTE]
> Wenn das Umbenennen von Attributen abhängig ist, die durch den Attributprozessor geändert werden, stellen Sie sicher, dass der Span-Prozessor in der Pipelinespezifikation nach dem Attributprozessor angegeben wird.

```json
"processors": [
  {
    "type": "span",
    "name": {
      "fromAttributes": [
        "attributeKey1",
        "attributeKey2",
      ],
      "separator": "::"
    }
  }
] 
```

### <a name="extract-attributes-from-span-name"></a>Extrahieren von Attributen aus dem Span-Namen

Eine Liste von regulären Ausdrücken ist erforderlich, um Span-Namen abzugleichen und Attribute auf der Grundlage von Teilausdrücken daraus zu extrahieren. Sie muss im Abschnitt `toAttributes` angegeben werden.

Die folgenden Einstellungen sind erforderlich:

`rules` : Eine Liste von Regeln zum Extrahieren von Attributwerten aus dem Span-Namen. Die Werte im Span-Namen werden durch extrahierte Attributnamen ersetzt. Jede Regel in der Liste ist eine RegEx-Musterzeichenfolge. Der Span-Name wird anhand der RegEx-Musterzeichenfolge überprüft. Wenn die RegEx-Musterzeichenfolge übereinstimmt, werden alle benannten Teilausdrücke der RegEx-Musterzeichenfolge als Attribute extrahiert und der Span hinzugefügt. Jeder Teilausdruckname wird zu einem Attributnamen, und der übereinstimmende Anteil des Teilausdrucks wird zum Attributwert. Der übereinstimmende Anteil im Span-Namen wird durch den extrahierten Attributnamen ersetzt. Wenn die Attribute bereits in der Span vorhanden sind, werden sie überschrieben. Der Prozess wird für alle Regeln in der Reihenfolge wiederholt, in der sie angegeben sind. Jede nachfolgende Regel funktioniert bei dem Span-Namen, der nach der Verarbeitung der vorherigen Regel ausgegeben wird.

```json

"processors": [
  {
    "type": "span",
    "name": {
      "toAttributes": {
        "rules": [
          "rule1",
          "rule2",
          "rule3"
        ]
      }
    }
  }
]

```

## <a name="list-of-attributes"></a>Attributliste

Im Folgenden finden Sie eine Liste mit einigen gängigen span-Attributen, die in Telemetrieprozessoren verwendet werden können.

### <a name="http-spans"></a>HTTP-span-Elemente

| attribute  | type | Beschreibung | 
|---|---|---|
| `http.method` | Zeichenfolge | HTTP-Anforderungsmethode.|
| `http.url` | Zeichenfolge | Vollständige Anforderungs-URL in HTTP in Form von `scheme://host[:port]/path?query[#fragment]`. Üblicherweise wird das Fragment nicht über HTTP übertragen. Wenn es jedoch bekannt ist, sollte es dennoch eingeschlossen werden.|
| `http.status_code` | number | [HTTP-Antwortstatuscode](https://tools.ietf.org/html/rfc7231#section-6).|
| `http.flavor` | Zeichenfolge | Der verwendete HTTP-Typ. |
| `http.user_agent` | Zeichenfolge | Der Wert des vom Client gesendeten [HTTP User-Agent](https://tools.ietf.org/html/rfc7231#section-5.5.3)-Headers. |

### <a name="jdbc-spans"></a>JDBC-span-Elemente

| attribute  | type | Beschreibung  |
|---|---|---|
| `db.system` | Zeichenfolge | Ein Bezeichner für das verwendete DBMS-Produkt (Datenbank-Managementsystem). |
| `db.connection_string` | Zeichenfolge | Die Verbindungszeichenfolge, mit der die Verbindung mit der Datenbank hergestellt wird. Es wird empfohlen, eingebettete Anmeldeinformationen zu entfernen.|
| `db.user` | Zeichenfolge | Der Benutzername für den Zugriff auf die Datenbank. |
| `db.name` | Zeichenfolge | Mit diesem Attribut wird der Name der Datenbank gemeldet, auf die zugegriffen wird. Bei Befehlen für einen Datenbankwechsel sollte dieses Attribut auf die Zieldatenbank festgelegt werden (auch wenn der Befehl nicht erfolgreich ausgeführt wird).|
| `db.statement` | Zeichenfolge | Die Datenbankanweisung, die ausgeführt wird.|