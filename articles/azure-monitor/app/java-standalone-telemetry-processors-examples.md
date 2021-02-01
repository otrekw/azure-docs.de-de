---
title: 'Beispiele für Telemetrieprozessoren: Azure Monitor Application Insights für Java'
description: Beispiele für Telemetrieprozessoren in Azure Monitor Application Insights für Java
ms.topic: conceptual
ms.date: 12/29/2020
author: kryalama
ms.custom: devx-track-java
ms.author: kryalama
ms.openlocfilehash: 9b29c9611359c97c4097ad0b90ee2673bb28f37c
ms.sourcegitcommit: 77afc94755db65a3ec107640069067172f55da67
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/22/2021
ms.locfileid: "98696311"
---
# <a name="telemetry-processors-examples---azure-monitor-application-insights-for-java"></a>Beispiele für Telemetrieprozessoren: Azure Monitor Application Insights für Java

## <a name="includeexclude-samples"></a>Einschließen/Ausschließen von Stichproben

### <a name="1-include-spans"></a>1. Einschließen von Spans

Das folgende Beispiel zeigt, wie Sie Spans für diesen Attributprozessor einschließen. Alle anderen Spans, die nicht den Eigenschaften entsprechen, werden von diesem Prozessor nicht verarbeitet.

Die folgenden Bedingungen müssen für eine Übereinstimmung erfüllt sein:
* Der Span-Name muss „spanA“ oder „spanB“ lauten. 

Die folgenden Spans stimmen mit den include-Eigenschaften überein, und die Prozessoraktionen werden angewandt:
* Span1 Name: 'spanA' Attributes: {env: dev, test_request: 123, credit_card: 1234}
* Span2 Name: 'spanB' Attributes: {env: dev, test_request: false}
* Span3 Name: 'spanA' Attributes: {env: 1, test_request: dev, credit_card: 1234}

Der folgende Span stimmt nicht mit den include-Eigenschaften überein, und die Prozessoraktionen werden nicht angewandt:
* Span4 Name: 'spanC' Attributes: {env: dev, test_request: false}

```json
{
  "connectionString": "InstrumentationKey=00000000-0000-0000-0000-000000000000",
  "preview": {
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
        "actions": [
          {
            "key": "credit_card",
            "action": "delete"
          }
        ]
      }
    ]
  }
}
```

### <a name="2-exclude-spans"></a>2. Ausschließen von Spans

Das folgende Beispiel zeigt, wie Sie Spans für diesen Attributprozessor ausschließen. Alle Spans, die den Eigenschaften entsprechen, werden von diesem Prozessor nicht verarbeitet.

Die folgenden Bedingungen müssen für eine Übereinstimmung erfüllt sein:
* Der Span-Name muss „spanA“ oder „spanB“ lauten. 

Die folgenden Spans stimmen mit den exclude-Eigenschaften überein, und die Prozessoraktionen werden nicht angewandt:
* Span1 Name: 'spanA' Attributes: {env: dev, test_request: 123, credit_card: 1234}
* Span2 Name: 'spanB' Attributes: {env: dev, test_request: false}
* Span3 Name: 'spanA' Attributes: {env: 1, test_request: dev, credit_card: 1234}

Der folgende Span stimmt nicht mit den exclude-Eigenschaften überein, und die Prozessoraktionen werden angewandt:
* Span4 Name: 'spanC' Attributes: {env: dev, test_request: false}

```json
{
  "connectionString": "InstrumentationKey=00000000-0000-0000-0000-000000000000",
  "preview": {
    "processors": [
      {
        "type": "attribute",
        "exclude": {
          "matchType": "strict",
          "spanNames": [
            "spanA",
            "spanB"
          ]
        },
        "actions": [
          {
            "key": "credit_card",
            "action": "delete"
          }
        ]
      }
    ]
  }
}
```

### <a name="3-excludemulti-spans"></a>3. ExcludeMulti-Spans

Das folgende Beispiel zeigt, wie Sie Spans für diesen Attributprozessor ausschließen. Alle Spans, die den Eigenschaften entsprechen, werden von diesem Prozessor nicht verarbeitet.

Die folgenden Bedingungen müssen für eine Übereinstimmung erfüllt sein:
* Für eine Übereinstimmung mit ein Attribut („env“, „dev“) im Span vorhanden sein.
* Solange ein Attribut mit dem Schlüssel „test_request“ im Span vorhanden ist, besteht eine Übereinstimmung.

Die folgenden Spans stimmen mit den exclude-Eigenschaften überein, und die Prozessoraktionen werden nicht angewandt:
* Span1 Name: 'spanB' Attributes: {env: dev, test_request: 123, credit_card: 1234}
* Span2 Name: 'spanA' Attributes: {env: dev, test_request: false}

Der folgende Span stimmt nicht mit den exclude-Eigenschaften überein, und die Prozessoraktionen werden angewandt:
* Span3 Name: 'spanB' Attributes: {env: 1, test_request: dev, credit_card: 1234}
* Span4 Name: 'spanC' Attributes: {env: dev, dev_request: false}


```json
{
  "connectionString": "InstrumentationKey=00000000-0000-0000-0000-000000000000",
  "preview": {
    "processors": [
      {
        "type": "attribute",
        "exclude": {
          "matchType": "strict",
          "spanNames": [
            "spanA",
            "spanB"
          ],
          "attributes": [
            {
              "key": "env",
              "value": "dev"
            },
            {
              "key": "test_request"
            }
          ]
        },
        "actions": [
          {
            "key": "credit_card",
            "action": "delete"
          }
        ]
      }
    ]
  }
}
```

### <a name="4-selective-processing"></a>4. Selektive Verarbeitung

Das folgende Beispiel zeigt, wie Sie den Satz von Span-Eigenschaften festlegen, um anzugeben, auf welche Spans dieser Prozessor angewendet werden soll. Mit `include` werden Eigenschaften eingeschlossen, und `exclude` filtert Eigenschaften heraus, die nicht verarbeitet werden sollen.

Mit der obigen Konfiguration stimmen die folgenden Spans mit den Eigenschaften überein und Prozessoraktionen werden angewandt:

* Span1 Name: 'spanB' Attributes: {env: production, test_request: 123, credit_card: 1234, redact_trace: "false"}
* Span2 Name: 'spanA' Attributes: {env: staging, test_request: false, redact_trace: true}

Die folgenden Spans stimmen nicht mit den Include-Eigenschaften und Prozessoraktionen überein:
* Span3 Name: 'spanB' Attributes: {env: production, test_request: true, credit_card: 1234, redact_trace: false}
* Span4 Name: 'spanC' Attributes: {env: dev, test_request: false}

```json
{
  "connectionString": "InstrumentationKey=00000000-0000-0000-0000-000000000000",
  "preview": {
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
  }
}
```
## <a name="attribute-processor-samples"></a>Beispiele für Attributprozessoren

### <a name="insert"></a>Einfügen

Mit dem folgenden Code wird Spans, in denen der Schlüssel „attribute1“ nicht vorhanden ist, ein neues Attribut {"attribute1": "attributeValue1"} hinzugefügt.

```json
{
  "connectionString": "InstrumentationKey=00000000-0000-0000-0000-000000000000",
  "preview": {
    "processors": [
      {
        "type": "attribute",
        "actions": [
          {
            "key": "attribute1",
            "value": "attributeValue1",
            "action": "insert"
          }
        ]
      }
    ]
  }
}
```

### <a name="insert-from-another-key"></a>Einfügen von einem anderen Schlüssel

Im Folgenden wird der Wert des Attributs „anotherkey“ verwendet, um in Spans, in denen der Schlüssel „newKey“ nicht vorhanden ist, ein neues Attribut {"newKey": "Wert aus Attribut 'anotherkey'“} einzufügen. Wenn das Attribut „anotherkey“ nicht vorhanden ist, wird in den Spans kein neues Attribut eingefügt.

```json
{
  "connectionString": "InstrumentationKey=00000000-0000-0000-0000-000000000000",
  "preview": {
    "processors": [
      {
        "type": "attribute",
        "actions": [
          {
            "key": "newKey",
            "fromAttribute": "anotherKey",
            "action": "insert"
          }
        ]
      }
    ]
  }
}
```

### <a name="update"></a>Aktualisieren

Im Folgenden wird das-Attribut in {"db.secret": "redacted"} geändert und das Attribut „boo“ mithilfe des Werts aus dem Attribut „foo“ aktualisiert. Spans ohne das Attribut „boo“ werden nicht geändert.

```json
{
  "connectionString": "InstrumentationKey=00000000-0000-0000-0000-000000000000",
  "preview": {
    "processors": [
      {
        "type": "attribute",
        "actions": [
          {
            "key": "db.secret",
            "value": "redacted",
            "action": "update"
          },
          {
            "key": "boo",
            "fromAttribute": "foo",
            "action": "update" 
          }
        ]
      }
    ]
  }
}
```

### <a name="delete"></a>Löschen

Das folgende Beispiel veranschaulicht das Löschen eines Attributs mit dem Schlüssel „credit_card“.

```json
{
  "connectionString": "InstrumentationKey=00000000-0000-0000-0000-000000000000",
  "preview": {
    "processors": [
      {
        "type": "attribute",
        "actions": [
          {
            "key": "credit_card",
            "action": "delete"
          }
        ]
      }
    ]
  }
}
```

### <a name="hash"></a>Hash

Im folgenden Beispiel wird das Erstellen eines Hash aus vorhandenen Attributwerten veranschaulicht.

```json
{
  "connectionString": "InstrumentationKey=00000000-0000-0000-0000-000000000000",
  "preview": {
    "processors": [
      {
        "type": "attribute",
        "actions": [
          {
            "key": "user.email",
            "action": "hash"
          }
        ]
      }
    ]
  }
}
```

### <a name="extract"></a>Extract

Im folgenden Beispiel wird gezeigt, wie Sie mit regulären Ausdrücken neue Attribute auf der Grundlage des Werts eines anderen Attributs erstellen.
Wenn z. B. „http.url“ = „http://example.com/path?queryParam1=value1,queryParam2=value2“, werden die folgenden Attribute eingefügt:
* httpProtocol: http
* httpDomain: example.com
* httpPath: Pfad
* httpQueryParams: queryParam1=value1,queryParam2=value2
* Der Wert von „http.url“ ändert sich NICHT.

```json
{
  "connectionString": "InstrumentationKey=00000000-0000-0000-0000-000000000000",
  "preview": {
    "processors": [
      {
        "type": "attribute",
        "actions": [
          {
            "key": "http.url",
            "pattern": "^(?<httpProtocol>.*):\\/\\/(?<httpDomain>.*)\\/(?<httpPath>.*)(\\?|\\&)(?<httpQueryParams>.*)",
            "action": "extract"
          }
        ]
      }
    ]
  }
}
```

Im folgenden Beispiel wird veranschaulicht, wie Spans verarbeitet werden, deren Span-Name RegExp-Mustern entspricht.
Dieser Prozessor entfernt das Attribut „token“ und verschleiert das Attribut „password“ in Spans, deren Span-Name mit „auth.\*“ übereinstimmt und nicht mit „login.\*“.

```json
{
  "connectionString": "InstrumentationKey=00000000-0000-0000-0000-000000000000",
  "preview": {
    "processors": [
      {
        "type": "attribute",
        "include": {
          "matchType": "regexp",
          "spanNames": [
            "auth.*"
          ]
        },
        "exclude": {
          "matchType": "regexp",
          "spanNames": [
            "login.*"
          ]
        },
        "actions": [
          {
            "key": "password",
            "value": "obfuscated",
            "action": "update"
          },
          {
            "key": "token",
            "action": "delete"
          }
        ]
      }
    ]
  }
}
```


## <a name="span-processor-samples"></a>Beispiele für Span-Prozessoren

### <a name="name-a-span"></a>Name einer Span

Das folgende Beispiel zeigt, dass die Werte der Attribute „db.svc“, „operation“ und „id“ den neuen Namen der Span bilden, in dieser Reihenfolge, getrennt durch den Wert „::“.
```json
{
  "connectionString": "InstrumentationKey=00000000-0000-0000-0000-000000000000",
  "preview": {
    "processors": [
      {
        "type": "span",
        "name": {
          "fromAttributes": [
            "db.svc",
            "operation",
            "id"
          ],
          "separator": "::"
        }
      }
    ]
  }
}
```

### <a name="extract-attributes-from-span-name"></a>Extrahieren von Attributen aus dem Span-Namen

Nehmen wir an, der Eingabe-Span-Name sei „/api/v1/document/12345678/update“. Durch das Anwenden der folgenden Ergebnisse im Ausgabe-Span-Namen „/api/v1/document/{documentId}/update“ wird der Span ein neues Attribut „documentId“=„12345678“ hinzugefügt.
```json
{
  "connectionString": "InstrumentationKey=00000000-0000-0000-0000-000000000000",
  "preview": {
    "processors": [
      {
        "type": "span",
        "name": {
          "toAttributes": {
            "rules": [
              "^/api/v1/document/(?<documentId>.*)/update$"
            ]
          }
        }
      }
    ]
  }
}
```

### <a name="extract-attributes-from-span-name-with-include-and-exclude"></a>Extrahieren von Attributen aus dem Span-Namen mit include und exclude

Im Folgenden wird das Umbenennen des Span-Namens in „{operation_website}“ und Hinzufügen des Attributs „{Key: operation_website, Value: oldSpanName}", wenn die Span die folgenden Eigenschaften aufweist, veranschaulicht:
- Der Span-Name enthält „/“ an beliebiger Stelle in der Zeichenfolge.
- Der Span-Name ist nicht „donot/change“.
```json
{
  "connectionString": "InstrumentationKey=00000000-0000-0000-0000-000000000000",
  "preview": {
    "processors": [
      {
        "type": "span",
        "include": {
          "matchType": "regexp",
          "spanNames": [
            "^(.*?)/(.*?)$"
          ]
        },
        "exclude": {
          "matchType": "strict",
          "spanNames": [
            "donot/change"
          ]
        },
        "name": {
          "toAttributes": {
            "rules": [
              "(?<operation_website>.*?)$"
            ]
          }
        }
      }
    ]
  }
}
```