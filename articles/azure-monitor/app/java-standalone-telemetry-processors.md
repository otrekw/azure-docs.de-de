---
title: 'Telemetrieprozessoren (Vorschau): Azure Monitor Application Insights für Java'
description: Hier erfahren Sie, wie Sie Telemetrieprozessoren in Azure Monitor Application Insights für Java konfigurieren.
ms.topic: conceptual
ms.date: 10/29/2020
author: kryalama
ms.custom: devx-track-java
ms.author: kryalama
ms.openlocfilehash: 79d46d7f967ec5d6a223b5ec8690536b3a84162a
ms.sourcegitcommit: 23040f695dd0785409ab964613fabca1645cef90
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/14/2021
ms.locfileid: "112061540"
---
# <a name="telemetry-processors-preview---azure-monitor-application-insights-for-java"></a>Telemetrieprozessoren (Vorschau): Azure Monitor Application Insights für Java

> [!NOTE]
> Das Feature „Telemetrieprozessoren“ befindet sich in der Vorschau.

Application Insights Java 3.x kann Telemetriedaten verarbeiten, bevor die Daten exportiert werden.

Hier sehen Sie einige Anwendungsfälle für Telemetrieprozessoren:
 * Maskieren vertraulicher Daten
 * Bedingtes Hinzufügen benutzerdefinierter Dimensionen
 * Aktualisieren des Namens des span-Elements, das zum Aggregieren ähnlicher Telemetriedaten im Azure-Portal verwendet wird
 * Verwerfen bestimmter span-Attribute zur Senkung von Erfassungskosten
 * Herausfiltern einiger Metriken zur Steuerung der Erfassungskosten

> [!NOTE]
> Wenn Sie bestimmte (ganze) Span-Elemente zur Senkung von Erfassungskosten verwerfen möchten, finden Sie Informationen dazu unter [Stichprobenüberschreibungen](./java-standalone-sampling-overrides.md).

## <a name="terminology"></a>Begriff

Bevor Sie mehr über Telemetrieprozessoren erfahren, müssen Sie wissen, was genau die Begriffe *span* und *log* bedeuten.

Als span bezeichnete Telemetriedaten stehen für Folgendes:

* Eine eingehende Anforderung
* Eine ausgehende Abhängigkeit (z. B. ein Remoteaufruf eines anderen Diensts)
* Eine In-Process-Abhängigkeit (z. B. Vorgänge, die von Unterkomponenten des Diensts durchgeführt werden)

Als log bezeichnete Telemetriedaten stehen für Folgendes:

* Von log4j, logback und java.util.logging erfasste Protokolldaten 

Bei Telemetrieprozessoren sind die folgenden span/log-Komponenten wichtig:

* Name
* Text
* Attribute

Der span-Name ist das primäre Anzeigeelement für Anforderungen und Abhängigkeiten im Azure-Portal. Die span-Attribute repräsentieren sowohl standardmäßige als auch benutzerdefinierte Eigenschaften einer bestimmten Anforderung oder Abhängigkeit.

Die Ablaufverfolgungsmeldung oder der Ablaufverfolgungstext ist die primäre Anzeige für Protokolle im Azure-Portal. Die log-Attribute repräsentieren sowohl standardmäßige als auch benutzerdefinierte Eigenschaften eines bestimmten Protokolls.

## <a name="telemetry-processor-types"></a>Arten von Telemetrieprozessoren

Derzeit sind vier Arten von Telemetrieprozessoren verfügbar: attribute-Prozessoren, span-Prozessoren, log-Prozessoren und Metrikfilter.

Mit einem attribute-Prozessor können Attribute von Telemetrieelementen (`span` oder `log`) eingefügt, aktualisiert, gelöscht oder mit einem Hash versehen werden.
Darüber hinaus können mithilfe eines regulären Ausdrucks ein oder mehrere Attribute aus einem vorhandenen Attribut extrahiert werden.

Mit einem span-Prozessor kann der Telemetriename von Anforderungen und Abhängigkeiten aktualisiert werden.
Außerdem können mithilfe eines regulären Ausdrucks ein oder mehrere Attribute aus dem span-Namen extrahiert werden.

Mit einem log-Prozessor kann der Name eines Telemetrieelements aktualisiert werden.
Außerdem können mithilfe eines regulären Ausdrucks ein oder mehrere Attribute aus dem log-Namen extrahiert werden.

Mit einem Metrikfilter können Metriken herausgefiltert werden, die dazu beitragen, die Erfassungskosten zu steuern.

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
      },
      {
        "type": "log",
        ...
      },
      {
        "type": "metric-filter",
        ...
      }
    ]
  }
}
```

## <a name="attribute-processor"></a>Attributprozessor

Vom attribute-Prozessor werden `span`- oder `log`-Attribute geändert. Er kann die Fähigkeit unterstützen, `span`-Elemente oder `log`-Elemente ein- oder auszuschließen. Er akzeptiert eine Liste von Aktionen, die in der Reihenfolge ausgeführt werden, die in der Konfigurationsdatei angegeben ist. Der Prozessor unterstützt die folgenden Aktionen:

- `insert`
- `update`
- `delete`
- `hash`
- `extract`

### `insert`

Mit der `insert`-Aktion wird ein neues Attribut in Telemetrieelemente eingefügt, in denen der `key` noch nicht vorhanden ist.   

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

Mit der `update`-Aktion wird ein Attribut im Telemetrieelement aktualisiert, in dem der `key` bereits vorhanden ist.

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

Mit der `delete`-Aktion wird ein Attribut aus einem Telemetrieelement gelöscht.

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

### <a name="include-criteria-and-exclude-criteria"></a>Einschluss- und Ausschlusskriterien (include und exclude)

Attribute-Prozessoren unterstützen optionale `include`- und `exclude`-Kriterien.
Ein attribute-Prozessor wird nur auf Telemetrieelemente angewendet, die mit den `include`-Kriterien übereinstimmen (sofern angegeben) _und_ nicht mit den `exclude`-Kriterien übereinstimmen (sofern angegeben).

Zum Konfigurieren dieser Option geben Sie unter `include` oder `exclude` (oder beidem) mindestens einen `matchType` sowie entweder `spanNames` oder `attributes` an.
Die include-/exclude-Konfiguration ermöglicht mehr als eine angegebene Bedingung.
Alle angegebenen Bedingungen müssen als TRUE ausgewertet werden, damit eine Übereinstimmung erzielt wird. 

* **Erforderliches Feld**: `matchType` steuert, wie Elemente in `spanNames`-Arrays und `attributes`-Arrays interpretiert werden. Mögliche Werte sind `regexp` und `strict`. 

* **Optionale Felder**: 
    * `spanNames` muss mindestens einem der Elemente entsprechen. 
    * `attributes` gibt die Liste der Attribute an, für die eine Übereinstimmung erzielt werden muss. All diese Attribute müssen exakt übereinstimmen.
    
> [!NOTE]
> Wenn sowohl `include` als auch `exclude` angegeben ist, werden die `include`-Eigenschaften vor den `exclude`-Eigenschaften geprüft.

> [!NOTE]
> Wenn für die `include`- oder `exclude`-Konfiguration keine `spanNames` festgelegt wurden, werden die Abgleichskriterien sowohl auf `spans` als auch auf `logs` angewendet.

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
2. Wenn der reguläre Ausdruck übereinstimmt, werden alle benannten Teilausdrücke des Ausdrucks als Attribute extrahiert. 
3. Die extrahierten Attribute werden dem span-Element hinzugefügt. 
4. Jeder Teilausdruckname wird zu einem Attributnamen. 
5. Der übereinstimmende Teil des Teilausdrucks wird zum Attributwert. 
6. Der übereinstimmende Teil im Namen des span-Elements wird durch den extrahierten Attributnamen ersetzt. Wenn die Attribute im span-Element bereits vorhanden sind, werden sie überschrieben. 
 
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

| attribute  | Typ | BESCHREIBUNG | 
|---|---|---|
| `http.method` | Zeichenfolge | HTTP-Anforderungsmethode.|
| `http.url` | Zeichenfolge | Vollständige Anforderungs-URL in HTTP in Form von `scheme://host[:port]/path?query[#fragment]`. Das Fragment wird üblicherweise nicht über HTTP übertragen. Wenn das Fragment jedoch bekannt ist, sollte es eingeschlossen werden.|
| `http.status_code` | number | [HTTP-Antwortstatuscode](https://tools.ietf.org/html/rfc7231#section-6).|
| `http.flavor` | Zeichenfolge | Der Typ des HTTP-Protokolls. |
| `http.user_agent` | Zeichenfolge | Der Wert des vom Client gesendeten [HTTP User-Agent](https://tools.ietf.org/html/rfc7231#section-5.5.3)-Headers. |

### <a name="jdbc-spans"></a>JDBC-span-Elemente

| attribute  | Typ | BESCHREIBUNG  |
|---|---|---|
| `db.system` | Zeichenfolge | Der Bezeichner für das verwendete DBMS-Produkt (Datenbank-Managementsystem). |
| `db.connection_string` | Zeichenfolge | Verbindungszeichenfolge, mit der die Verbindung zur Datenbank hergestellt wird. Es empfiehlt sich, eingebettete Anmeldeinformationen zu entfernen.|
| `db.user` | Zeichenfolge | Der Benutzername für den Zugriff auf die Datenbank. |
| `db.name` | Zeichenfolge | Die Zeichenfolge, die den Namen der Datenbank angibt, auf die zugegriffen wird. Bei Befehlen für einen Datenbankwechsel sollte dieses Attribut auf die Zieldatenbank festgelegt werden, auch wenn der Befehl nicht erfolgreich ausgeführt wird.|
| `db.statement` | Zeichenfolge | Die Datenbankanweisung, die ausgeführt wird.|

### <a name="include-criteria-and-exclude-criteria"></a>Einschluss- und Ausschlusskriterien (include und exclude)

Span-Prozessoren unterstützen optionale `include`- und `exclude`-Kriterien.
Ein span-Prozessor wird nur auf Telemetrieelemente angewendet, die mit den `include`-Kriterien übereinstimmen (sofern angegeben) _und_ nicht mit den `exclude`-Kriterien übereinstimmen (sofern angegeben).

Zum Konfigurieren dieser Option geben Sie unter `include` oder `exclude` (oder beiden) mindestens einen `matchType` sowie entweder `spanNames` oder `attributes` an.
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
    "type": "span",
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
          "key": "attribute1",
          "value": "attributeValue1"
        }
      ]
    },
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
Weitere Informationen finden Sie unter [Beispiele für Telemetrieprozessoren](./java-standalone-telemetry-processors-examples.md).

## <a name="log-processor"></a>Log-Prozessor

> [!NOTE]
> Log-Prozessoren sind ab Version 3.1.1 verfügbar.

Vom log-Prozessor werden entweder die Meldungstexte von log-Elementen oder die Attribute eines log-Elements basierend auf dem Meldungstext des log-Elements geändert. Er kann die Fähigkeit unterstützen, log-Elemente ein- oder auszuschließen.

### <a name="update-log-message-body"></a>Aktualisieren des Meldungstexts von log-Elementen

Der `body`-Abschnitt erfordert die Einstellung `fromAttributes`. Die Werte dieser Attribute werden dazu verwendet, neuen Text zu erstellen, verkettet in der Reihenfolge, die in der Konfiguration angegeben ist. Vom Prozessor wird der Name des log-Elementtexts nur geändert, wenn all diese Attribute im log-Element vorhanden sind.

Die Einstellung `separator` ist optional. Diese Einstellung ist eine Zeichenfolge. Sie wird zum Aufteilen von Werten angegeben.
> [!NOTE]
> Wenn es zum Umbenennen von Attributen erforderlich ist, dass die Attribute durch den attribute-Prozessor geändert werden, stellen Sie sicher, dass der log-Prozessor in der Pipelinespezifikation nach dem attribute-Prozessor angegeben wird.

```json
"processors": [
  {
    "type": "log",
    "body": {
      "fromAttributes": [
        "attributeKey1",
        "attributeKey2",
      ],
      "separator": "::"
    }
  }
] 
```

### <a name="extract-attributes-from-the-log-message-body"></a>Extrahieren von Attributen aus dem Meldungstext von log-Elementen

Im Abschnitt `toAttributes` sind die regulären Ausdrücke aufgeführt, mit denen der Meldungstext des log-Elements abgeglichen werden soll. Attribute werden basierend auf Teilausdrücken extrahiert.

Die Einstellung `rules` ist erforderlich. In dieser Einstellung werden die Regeln aufgeführt, die zum Extrahieren von Attributwerten aus dem Text verwendet werden. 

Die Werte im Meldungstext des log-Elements werden durch extrahierte Attributnamen ersetzt. Jede Regel in der Liste ist eine Musterzeichenfolge eines regulären Ausdrucks (RegEx). 

Die Werte werden wie folgt durch extrahierte Attributnamen ersetzt:

1. Der Meldungstext des log-Elements wird anhand des regulären Ausdrucks überprüft. 
2. Wenn der reguläre Ausdruck übereinstimmt, werden alle benannten Teilausdrücke des Ausdrucks als Attribute extrahiert. 
3. Die extrahierten Attribute werden dem log-Element hinzugefügt. 
4. Jeder Teilausdruckname wird zu einem Attributnamen. 
5. Der übereinstimmende Teil des Teilausdrucks wird zum Attributwert. 
6. Der übereinstimmende Teil im Namen des log-Elements wird durch den extrahierten Attributnamen ersetzt. Wenn die Attribute im log-Element bereits vorhanden sind, werden sie überschrieben. 
 
Der Prozess wird für alle Regeln in der Reihenfolge wiederholt, in der sie angegeben sind. Jede nachfolgende Regel wird auf den Namen des log-Elements angewendet, der die Ausgabe der vorherigen Regelverarbeitung ist.

```json
"processors": [
  {
    "type": "log",
    "body": {
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

### <a name="include-criteria-and-exclude-criteria"></a>Einschluss- und Ausschlusskriterien (include und exclude)

Log-Prozessoren unterstützen optionale `include`- und `exclude`-Kriterien.
Ein log-Prozessor wird nur auf Telemetrieelemente angewendet, die mit den `include`-Kriterien übereinstimmen (sofern angegeben) _und_ nicht mit den `exclude`-Kriterien übereinstimmen (sofern angegeben).

Geben Sie zum Konfigurieren dieser Option unter `include` oder `exclude` (oder beiden) den `matchType` und die `attributes` an.
Die include-/exclude-Konfiguration ermöglicht mehr als eine angegebene Bedingung.
Alle angegebenen Bedingungen müssen als TRUE ausgewertet werden, damit eine Übereinstimmung erzielt wird. 

* **Pflichtfeld**: 
  * `matchType` steuert, wie Elemente in den `attributes`-Arrays interpretiert werden. Mögliche Werte sind `regexp` und `strict`. 
  * `attributes` gibt die Liste der Attribute an, für die eine Übereinstimmung erzielt werden muss. All diese Attribute müssen exakt übereinstimmen.
    
> [!NOTE]
> Wenn sowohl `include` als auch `exclude` angegeben ist, werden die `include`-Eigenschaften vor den `exclude`-Eigenschaften geprüft.

> [!NOTE]
> Log-Prozessoren bieten keine Unterstützung für `spanNames`.

### <a name="sample-usage"></a>Beispielverwendung

```json
"processors": [
  {
    "type": "log",
    "include": {
      "matchType": "strict",
      "attributes": [
        {
          "key": "attribute1",
          "value": "value1"
        }
      ]
    },
    "exclude": {
      "matchType": "strict",
      "attributes": [
        {
          "key": "attribute2",
          "value": "value2"
        }
      ]
    },
    "body": {
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
Weitere Informationen finden Sie unter [Beispiele für Telemetrieprozessoren](./java-standalone-telemetry-processors-examples.md).

## <a name="metric-filter"></a>Metrikfilter

> [!NOTE]
> Metrikfilter sind ab Version 3.1.1 verfügbar.

Metrikfilter werden dazu verwendet, einige Metriken auszuschließen, damit die Steuerung der Erfassungskosten unterstützt werden kann.

Metrikfilter unterstützen nur `exclude`-Kriterien. Metriken, die den `exclude`-Kriterien entsprechen, werden nicht exportiert.

Geben Sie zum Konfigurieren dieser Option unter `exclude` den `matchType` für einen oder mehrere `metricNames` an.

* **Pflichtfeld**:
  * `matchType` steuert, wie Elemente in `metricNames` abgeglichen werden. Mögliche Werte sind `regexp` und `strict`.
  * `metricNames` muss mindestens einem der Elemente entsprechen.

### <a name="sample-usage"></a>Beispielverwendung

```json
"processors": [
  {
    "type": "metric-filter",
    "exclude": {
      "matchType": "strict",
      "metricNames": [
        "metricA",
        "metricB"
      ]
    }
  }
]
```
