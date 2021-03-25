---
title: 'Telemetrieprozessoren (Vorschau): Azure Monitor Application Insights für Java'
description: Hier erfahren Sie, wie Sie Telemetrieprozessoren in Azure Monitor Application Insights für Java konfigurieren.
ms.topic: conceptual
ms.date: 10/29/2020
author: kryalama
ms.custom: devx-track-java
ms.author: kryalama
ms.openlocfilehash: 35e53454e5b2c6265082bbedb4a8b60e82df7191
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "101734569"
---
# <a name="telemetry-processors-preview---azure-monitor-application-insights-for-java"></a>Telemetrieprozessoren (Vorschau): Azure Monitor Application Insights für Java

> [!NOTE]
> Das Feature „Telemetrieprozessoren“ befindet sich in der Vorschau.

Der Java 3.0-Agent für Application Insights kann Telemetriedaten verarbeiten, bevor die Daten exportiert werden.

Hier sehen Sie einige Anwendungsfälle für Telemetrieprozessoren:
 * Erstellen vertraulicher Daten
 * Bedingtes Hinzufügen benutzerdefinierter Dimensionen
 * Aktualisieren des Namens des span-Elements, das zum Aggregieren ähnlicher Telemetriedaten im Azure-Portal verwendet wird
 * Verwerfen von span-Attributen zur Senkung von Erfassungskosten

## <a name="terminology"></a>Terminologie

Bevor Sie mehr über Telemetrieprozessoren erfahren, müssen Sie wissen, was genau der Begriff *span* bedeutet. Ein span ist eine allgemeine Bezeichnung für Folgendes:

* Eine eingehende Anforderung
* Eine ausgehende Abhängigkeit (z. B. ein Remoteaufruf eines anderen Diensts)
* Eine In-Process-Abhängigkeit (z. B. Vorgänge, die von Unterkomponenten des Diensts durchgeführt werden)

Bei Telemetrieprozessoren sind die folgenden span-Komponenten wichtig:

* Name
* Attribute

Der span-Name ist das primäre Anzeigeelement für Anforderungen und Abhängigkeiten im Azure-Portal. Die span-Attribute repräsentieren sowohl standardmäßige als auch benutzerdefinierte Eigenschaften einer bestimmten Anforderung oder Abhängigkeit.

## <a name="telemetry-processor-types"></a>Arten von Telemetrieprozessoren

Derzeit gibt es zwei Arten von Telemetrieprozessoren: attribute-Prozessoren und span-Prozessoren.

Mit einem attribute-Prozessor können Attribute eingefügt, aktualisiert, gelöscht oder mit einem Hash versehen werden.
Darüber hinaus können mithilfe eines regulären Ausdrucks ein oder mehrere Attribute aus einem vorhandenen Attribut extrahiert werden.

Mit einem span-Prozessor kann der Name einer Telemetrie aktualisiert werden.
Außerdem können mithilfe eines regulären Ausdrucks ein oder mehrere Attribute aus dem span-Namen extrahiert werden.

> [!NOTE]
> Derzeit verarbeiten Telemetrieprozessoren nur Attribute des Typs „string“. Boolesche oder Zahlenattribute werden nicht verarbeitet.

## <a name="getting-started"></a>Erste Schritte

Zunächst erstellen Sie eine Konfigurationsdatei namens *applicationinsights.json*. Speichern Sie diese im gleichen Verzeichnis, in dem sich auch *applicationinsights-agent-\*.jar* befindet. Verwenden Sie hierfür die folgende Vorlage.

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

## <a name="include-criteria-and-exclude-criteria"></a>Einschluss- und Ausschlusskriterien (include und exclude)

Für attribute-Prozessoren und span-Prozessoren werden jeweils optionale Kriterien für das Ein- und Ausschließen (`include` und `exclude`) unterstützt.
Ein Prozessor wird nur auf span-Elemente angewendet, die mit den `include`-Kriterien übereinstimmen (sofern angegeben) _und_ nicht mit den `exclude`-Kriterien übereinstimmen (sofern angegeben).

Zum Konfigurieren dieser Option geben Sie unter `include` oder `exclude` (oder beidem) mindestens einen `matchType` sowie entweder `spanNames` oder `attributes` an.
Die include-/exclude-Konfiguration ermöglicht mehr als eine angegebene Bedingung.
Alle angegebenen Bedingungen müssen als TRUE ausgewertet werden, damit eine Übereinstimmung erzielt wird. 

* **Erforderliches Feld**: `matchType` steuert, wie Elemente in `spanNames`-Arrays und `attributes`-Arrays interpretiert werden. Mögliche Werte sind `regexp` und `strict`. 

* **Optionale Felder**: 
    * `spanNames` muss mindestens einem der Elemente entsprechen. 
    * `attributes` gibt die Liste der Attribute an, für die eine Übereinstimmung erzielt werden muss. All diese Attribute müssen exakt übereinstimmen.
    
> [!NOTE]
> Wenn sowohl `include` als auch `exclude` angegeben ist, werden die `include`-Eigenschaften vor den `exclude`-Eigenschaften geprüft.

### <a name="sample-usage"></a>Beispielverwendung

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
Weitere Informationen finden Sie unter [Beispiele für Telemetrieprozessoren](./java-standalone-telemetry-processors-examples.md).

## <a name="attribute-processor"></a>Attributprozessor

Der Attributprozessor ändert Attribute eines span-Elements. Er kann die Fähigkeit unterstützen, span-Elemente ein- oder auszuschließen. Er akzeptiert eine Liste von Aktionen, die in der Reihenfolge ausgeführt werden, die in der Konfigurationsdatei angegeben ist. Der Prozessor unterstützt die folgenden Aktionen:

- `insert`
- `update`
- `delete`
- `hash`
- `extract`
### `insert`

Die `insert`-Aktion fügt ein neues Attribut in span-Elemente ein, in denen der Schlüssel noch nicht vorhanden ist.   

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
Die `insert`-Aktion erfordert die folgenden Einstellungen:
* `key`
* Entweder `value` oder `fromAttribute`
* `action`: `insert`

### `update`

Die `update`-Aktion aktualisiert ein Attribut in span-Elementen, in denen der Schlüssel bereits vorhanden ist.

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
Die `update`-Aktion erfordert die folgenden Einstellungen:
* `key`
* Entweder `value` oder `fromAttribute`
* `action`: `update`


### `delete` 

Die `delete`-Aktion löscht ein Attribut aus einem span-Element.

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
Die `delete`-Aktion erfordert die folgenden Einstellungen:
* `key`
* `action`: `delete`

### `hash`

Die `hash`-Aktion versieht einen vorhandenen Attributwert mit einem Hash (SHA1).

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
Die `hash`-Aktion erfordert die folgenden Einstellungen:
* `key`
* `action`: `hash`

### `extract`

> [!NOTE]
> Das `extract`-Feature ist erst ab Version 3.0.2 verfügbar.

Die `extract`-Aktion extrahiert Werte mithilfe einer RegEx-Regel aus dem Eingabeschlüssel in die in der Regel angegebenen Zielschlüssel. Wenn ein Zielschlüssel bereits vorhanden ist, wird er überschrieben. Diese Aktion verhält sich wie die `toAttributes`-Einstellung [span-Prozessor](#extract-attributes-from-the-span-name), wobei das vorhandene Attribut die Quelle ist.

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
Die `extract`-Aktion erfordert die folgenden Einstellungen:
* `key`
* `pattern`
* `action`: `extract`

Weitere Informationen finden Sie unter [Beispiele für Telemetrieprozessoren](./java-standalone-telemetry-processors-examples.md).

## <a name="span-processor"></a>span-Prozessor

Der Span-Prozessor ändert entweder den Span-Namen oder die Attribute einer Span basierend auf dem Span-Namen. Er kann die Fähigkeit unterstützen, span-Elemente ein- oder auszuschließen.

### <a name="name-a-span"></a>Name einer Span

Der `name`-Abschnitt erfordert die Einstellung `fromAttributes`. Die Werte dieser Attribute werden verwendet, um einen neuen Namen zu erstellen, verkettet in der Reihenfolge, die in der Konfiguration angegeben ist. Der Prozessor ändert den Namen des span-Elements nur, wenn all diese Attribute im span-Element vorhanden sind.

Die Einstellung `separator` ist optional. Diese Einstellung ist eine Zeichenfolge. Sie wird zum Aufteilen von Werten angegeben.
> [!NOTE]
> Wenn es zum Umbenennen von Attributen erforderlich ist, dass die Attribute durch den Attributprozessor geändert werden, stellen Sie sicher, dass der span-Prozessor in der Pipelinespezifikation nach dem Attributprozessor angegeben wird.

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

### <a name="extract-attributes-from-the-span-name"></a>Extrahieren von Attributen aus dem Namen des span-Elements

Im Abschnitt `toAttributes` sind die regulären Ausdrücke aufgeführt, mit denen der Name des span-Elements abgeglichen werden soll. Attribute werden basierend auf Teilausdrücken extrahiert.

Die Einstellung `rules` ist erforderlich. Diese Einstellung führt die Regeln auf, die zum Extrahieren von Attributwerten aus dem Namen des span-Elements verwendet werden. 

Die Werte im Span-Namen werden durch extrahierte Attributnamen ersetzt. Jede Regel in der Liste ist eine Musterzeichenfolge eines regulären Ausdrucks (RegEx). 

Die Werte werden wie folgt durch extrahierte Attributnamen ersetzt:

1. Der Name des span-Elements wird anhand des regulären Ausdrucks überprüft. 
1. Wenn der reguläre Ausdruck übereinstimmt, werden alle benannten Teilausdrücke des Ausdrucks als Attribute extrahiert. 
1. Die extrahierten Attribute werden dem span-Element hinzugefügt. 
1. Jeder Teilausdruckname wird zu einem Attributnamen. 
1. Der übereinstimmende Teil des Teilausdrucks wird zum Attributwert. 
1. Der übereinstimmende Teil im Namen des span-Elements wird durch den extrahierten Attributnamen ersetzt. Wenn die Attribute im span-Element bereits vorhanden sind, werden sie überschrieben. 
 
Der Prozess wird für alle Regeln in der Reihenfolge wiederholt, in der sie angegeben sind. Jede nachfolgende Regel wird auf den Namen des span-Elements angewendet, der die Ausgabe der vorherigen Regelverarbeitung ist.

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

## <a name="common-span-attributes"></a>Gängige span-Attribute

In diesem Abschnitt werden einige gängige span-Attribute aufgelistet, die von Telemetrieprozessoren verwendet werden können.

### <a name="http-spans"></a>HTTP-span-Elemente

| attribute  | type | BESCHREIBUNG | 
|---|---|---|
| `http.method` | Zeichenfolge | HTTP-Anforderungsmethode.|
| `http.url` | Zeichenfolge | Vollständige Anforderungs-URL in HTTP in Form von `scheme://host[:port]/path?query[#fragment]`. Das Fragment wird üblicherweise nicht über HTTP übertragen. Wenn das Fragment jedoch bekannt ist, sollte es eingeschlossen werden.|
| `http.status_code` | number | [HTTP-Antwortstatuscode](https://tools.ietf.org/html/rfc7231#section-6).|
| `http.flavor` | Zeichenfolge | Der Typ des HTTP-Protokolls. |
| `http.user_agent` | Zeichenfolge | Der Wert des vom Client gesendeten [HTTP User-Agent](https://tools.ietf.org/html/rfc7231#section-5.5.3)-Headers. |

### <a name="jdbc-spans"></a>JDBC-span-Elemente

| attribute  | type | BESCHREIBUNG  |
|---|---|---|
| `db.system` | Zeichenfolge | Der Bezeichner für das verwendete DBMS-Produkt (Datenbank-Managementsystem). |
| `db.connection_string` | Zeichenfolge | Verbindungszeichenfolge, mit der die Verbindung zur Datenbank hergestellt wird. Es empfiehlt sich, eingebettete Anmeldeinformationen zu entfernen.|
| `db.user` | Zeichenfolge | Der Benutzername für den Zugriff auf die Datenbank. |
| `db.name` | Zeichenfolge | Die Zeichenfolge, die den Namen der Datenbank angibt, auf die zugegriffen wird. Bei Befehlen für einen Datenbankwechsel sollte dieses Attribut auf die Zieldatenbank festgelegt werden, auch wenn der Befehl nicht erfolgreich ausgeführt wird.|
| `db.statement` | Zeichenfolge | Die Datenbankanweisung, die ausgeführt wird.|
