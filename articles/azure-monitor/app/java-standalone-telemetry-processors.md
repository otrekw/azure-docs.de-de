---
title: 'Telemetrieprozessoren (Vorschau): Azure Monitor Application Insights für Java'
description: Konfigurieren von Telemetrieprozessoren in Azure Monitor Application Insights für Java
ms.topic: conceptual
ms.date: 10/29/2020
author: kryalama
ms.custom: devx-track-java
ms.author: kryalama
ms.openlocfilehash: 39897e490e4653fbaad7a64ecc0b33f161d1264b
ms.sourcegitcommit: 16887168729120399e6ffb6f53a92fde17889451
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/13/2021
ms.locfileid: "98165789"
---
# <a name="telemetry-processors-preview---azure-monitor-application-insights-for-java"></a>Telemetrieprozessoren (Vorschau): Azure Monitor Application Insights für Java

> [!NOTE]
> Dieses Feature befindet sich noch in der Vorschauphase.

Der Java 3.0-Agent für Application Insights verfügt jetzt über Funktionen, um Telemetriedaten zu verarbeiten, bevor die Daten exportiert werden.

Hier einige Anwendungsfälle für Telemetrieprozessoren:
 * Maskieren vertraulicher Daten
 * Bedingtes Hinzufügen benutzerdefinierter Dimensionen
 * Aktualisieren des Namens, der für die Aggregation und Anzeige im Azure-Portal genutzt wird
 * Verwerfen von span-Attributen zur Senkung von Erfassungskosten

## <a name="terminology"></a>Begriff

Bevor wir uns mit Telemetrieprozessoren beschäftigen, müssen Sie verstehen, was „span“ bedeutet.

„span“ (Deutsch: Spanne bzw. Zeitspanne) ist eine allgemeine Bezeichnung für die folgenden drei Punkte:

* Eine eingehende Anforderung
* Eine ausgehende Abhängigkeit (z. B. ein Remoteaufruf eines anderen Diensts)
* Eine In-Process-Abhängigkeit (z. B. Vorgänge, die von Unterkomponenten des Diensts durchgeführt werden)

Bei Telemetrieprozessoren sollten die folgenden wichtigen Komponenten berücksichtigt werden:

* Name
* Attribute

Der span-Name ist das primäre Anzeigeelement, das im Azure-Portal für Anforderungen und Abhängigkeiten genutzt wird.

Die span-Attribute stehen sowohl für die standardmäßigen als auch die benutzerdefinierten Eigenschaften einer bestimmten Anforderung oder Abhängigkeit.

## <a name="telemetry-processor-types"></a>Arten von Telemetrieprozessoren

Derzeit gibt es zwei Arten von Telemetrieprozessoren.

#### <a name="attribute-processor"></a>attribute-Prozessor

Mit einem attribute-Prozessor können Attribute eingefügt, aktualisiert, gelöscht oder mit einem Hash versehen werden.
Darüber hinaus können auch ein oder mehrere Attribute (per regulärem Ausdruck) aus einem vorhandenen Attribut extrahiert werden.

#### <a name="span-processor"></a>span-Prozessor

Mit einem span-Prozessor kann der Telemetriename aktualisiert werden.
Außerdem können ein oder mehrere Attribute (per regulärem Ausdruck) aus dem span-Namen extrahiert werden.

> [!NOTE]
> Beachten Sie Folgendes: Derzeit werden von Telemetrieprozessoren nur Prozessattribute vom Typ „Zeichenfolge“ verarbeitet (nicht vom Typ „Boolesch“ oder „Zahl“).

## <a name="getting-started"></a>Erste Schritte

Erstellen Sie eine Konfigurationsdatei mit dem Namen `applicationinsights.json`, und legen Sie sie im gleichen Verzeichnis wie `applicationinsights-agent-*.jar` mit folgender Vorlage ab.

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

## <a name="includeexclude-criteria"></a>Include/Exclude-Kriterien

Für attribute-Prozessoren und span-Prozessoren werden jeweils optionale Kriterien für das Ein- und Ausschließen (`include` und `exclude`) unterstützt.
Ein Prozessor wird nur auf die span-Elemente angewendet, für die sich eine Übereinstimmung mit den `include`-Kriterien (falls angegeben) _und_ keine Übereinstimmung mit den `exclude`-Kriterien (falls angegeben) ergibt.

Um diese Option zu konfigurieren, ist unter `include` und/oder `exclude` mindestens ein `matchType` und ein Exemplar von `spanNames` oder `attributes` erforderlich.
Die Konfiguration für das Ein- bzw. Ausschließen wird unterstützt, damit mehr als eine angegebene Bedingung vorhanden ist.
Alle angegebenen Bedingungen müssen als „true“ ausgewertet werden, damit eine Entsprechung auftritt. 

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
      }
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
      }
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
      }
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
      }
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
      }
    ]
  }
]
```
Für die `extract`-Aktion wird Folgendes benötigt:
* `key`
* `pattern`
* `action` : `extract`

Weitere Informationen finden Sie in der Dokumentation mit [Beispielen für Telemetrieprozessoren](./java-standalone-telemetry-processors-examples.md).

## <a name="span-processor"></a>span-Prozessor

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

| attribute  | Typ | Beschreibung | 
|---|---|---|
| `http.method` | Zeichenfolge | HTTP-Anforderungsmethode.|
| `http.url` | Zeichenfolge | Vollständige Anforderungs-URL in HTTP in Form von `scheme://host[:port]/path?query[#fragment]`. Üblicherweise wird das Fragment nicht über HTTP übertragen. Wenn es jedoch bekannt ist, sollte es dennoch eingeschlossen werden.|
| `http.status_code` | number | [HTTP-Antwortstatuscode](https://tools.ietf.org/html/rfc7231#section-6).|
| `http.flavor` | Zeichenfolge | Der verwendete HTTP-Typ. |
| `http.user_agent` | Zeichenfolge | Der Wert des vom Client gesendeten [HTTP User-Agent](https://tools.ietf.org/html/rfc7231#section-5.5.3)-Headers. |

### <a name="jdbc-spans"></a>JDBC-span-Elemente

| attribute  | Typ | Beschreibung  |
|---|---|---|
| `db.system` | Zeichenfolge | Ein Bezeichner für das verwendete DBMS-Produkt (Datenbank-Managementsystem). |
| `db.connection_string` | Zeichenfolge | Die Verbindungszeichenfolge, mit der die Verbindung mit der Datenbank hergestellt wird. Es wird empfohlen, eingebettete Anmeldeinformationen zu entfernen.|
| `db.user` | Zeichenfolge | Der Benutzername für den Zugriff auf die Datenbank. |
| `db.name` | Zeichenfolge | Mit diesem Attribut wird der Name der Datenbank gemeldet, auf die zugegriffen wird. Bei Befehlen für einen Datenbankwechsel sollte dieses Attribut auf die Zieldatenbank festgelegt werden (auch wenn der Befehl nicht erfolgreich ausgeführt wird).|
| `db.statement` | Zeichenfolge | Die Datenbankanweisung, die ausgeführt wird.|
