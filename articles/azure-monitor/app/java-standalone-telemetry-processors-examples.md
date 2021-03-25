---
title: 'Beispiele für Telemetrieprozessoren: Azure Monitor Application Insights für Java'
description: Erkunden Sie Beispiele für Telemetrieprozessoren in Azure Monitor Application Insights für Java.
ms.topic: conceptual
ms.date: 12/29/2020
author: kryalama
ms.custom: devx-track-java
ms.author: kryalama
ms.openlocfilehash: 0978bd669855d264ed6dfa5eeddc45ad499aa2a5
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "101734586"
---
# <a name="telemetry-processor-examples---azure-monitor-application-insights-for-java"></a>Beispiele für Telemetrieprozessoren: Azure Monitor Application Insights für Java

Dieser Artikel stellt Beispiele für Telemetrieprozessoren in Application Insights für Java bereit. Hier finden Sie Beispiele für Einschluss- und Ausschlusskonfigurationen. Sie finden auch Beispiele für attribute-Prozessoren und span-Prozessoren.
## <a name="include-and-exclude-samples"></a>Beispiele für Ein- und Ausschlüsse

In diesem Abschnitt erfahren Sie, wie Sie span-Elemente einschließen und ausschließen. Sie erfahren auch, wie Sie mehrere span-Elemente ausschließen und eine selektive Verarbeitung anwenden.
### <a name="include-spans"></a>Einschließen von span-Elementen

Dieser Abschnitt zeigt, wie Sie span-Elemente für einen attribute-Prozessor einschließen. span-Elemente, die nicht mit den Eigenschaften übereinstimmen. werden vom Prozessor nicht verarbeitet.

Um eine Übereinstimmung zu erzielen, muss der Name des span-Elements `spanA` oder `spanB` entsprechen. 

Die folgenden span-Elemente stimmen mit den include-Eigenschaften überein, und die Prozessoraktionen werden angewendet:
* Span1 Name: 'spanA' Attributes: {env: dev, test_request: 123, credit_card: 1234}
* Span2 Name: 'spanB' Attributes: {env: dev, test_request: false}
* Span3 Name: 'spanA' Attributes: {env: 1, test_request: dev, credit_card: 1234}

Dieses span-Element stimmt nicht mit den include-Eigenschaften überein, und die Prozessoraktionen werden nicht angewendet:
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

### <a name="exclude-spans"></a>Ausschließen von span-Elementen

Dieser Abschnitt veranschaulicht, wie Sie span-Elemente für einen attribute-Prozessor ausschließen. span-Elemente, die mit den Eigenschaften übereinstimmen. werden vom Prozessor nicht verarbeitet.

Um eine Übereinstimmung zu erzielen, muss der Name des span-Elements `spanA` oder `spanB` entsprechen.

Die folgenden span-Elemente stimmen mit den exclude-Eigenschaften überein, und die Prozessoraktionen werden nicht angewendet:
* Span1 Name: 'spanA' Attributes: {env: dev, test_request: 123, credit_card: 1234}
* Span2 Name: 'spanB' Attributes: {env: dev, test_request: false}
* Span3 Name: 'spanA' Attributes: {env: 1, test_request: dev, credit_card: 1234}

Dieses span-Element stimmt nicht mit den exclude-Eigenschaften überein, und die Prozessoraktionen werden angewendet:
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

### <a name="exclude-spans-by-using-multiple-criteria"></a>Ausschließen von span-Elementen anhand mehrerer Kriterien

Dieser Abschnitt veranschaulicht, wie Sie span-Elemente für einen attribute-Prozessor ausschließen. span-Elemente, die mit den Eigenschaften übereinstimmen. werden vom Prozessor nicht verarbeitet.

Um eine Übereinstimmung zu erzielen, müssen die folgenden Bedingungen erfüllt sein:
* Im span-Element muss ein Attribut vorhanden sein (z. B. `env` oder `dev`).
* Das span-Element muss über ein Attribut mit dem Schlüssel `test_request` verfügen.

Die folgenden span-Elemente stimmen mit den exclude-Eigenschaften überein, und die Prozessoraktionen werden nicht angewendet:
* Span1 Name: 'spanB' Attributes: {env: dev, test_request: 123, credit_card: 1234}
* Span2 Name: 'spanA' Attributes: {env: dev, test_request: false}

Das folgende span-Element stimmt nicht mit den exclude-Eigenschaften überein, und die Prozessoraktionen werden angewendet:
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

### <a name="selective-processing"></a>Selektive Verarbeitung

Dieser Abschnitt zeigt, wie Sie die Gruppe von span-Eigenschaften angeben, die festlegen, auf welche span-Elemente dieser Prozessor angewendet werden soll. Die include-Eigenschaften geben an, welche span-Elemente verarbeitet werden sollen. Die exclude-Eigenschaften filtern span-Elemente heraus, die nicht verarbeitet werden sollen.

In der folgenden Konfiguration stimmen diese span-Elemente mit den Eigenschaften überein, und die Prozessoraktionen werden angewendet:

* Span1 Name: 'spanB' Attributes: {env: production, test_request: 123, credit_card: 1234, redact_trace: "false"}
* Span2 Name: 'spanA' Attributes: {env: staging, test_request: false, redact_trace: true}

Diese span-Elemente stimmen nicht mit den include-Eigenschaften überein, und die Prozessoraktionen werden nicht angewendet:
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
## <a name="attribute-processor-samples"></a>Beispiele für attribute-Prozessoren

### <a name="insert"></a>Einfügen

Das folgende Beispiel fügt das neue Attribut `{"attribute1": "attributeValue1"}` in span-Elemente ein, in denen der Schlüssel `attribute1` nicht vorhanden ist.

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

Das folgende Beispiel verwendet den Wert aus dem Attribut `anotherkey`, um das neue Attribut `{"newKey": "<value from attribute anotherkey>"}` in span-Elemente einzufügen, in denen der Schlüssel `newKey` nicht vorhanden ist. Wenn das Attribut `anotherkey` nicht vorhanden ist, wird kein neues Attribut in die span-Elemente eingefügt.

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

Das folgende Beispiel aktualisiert das Attribut zu `{"db.secret": "redacted"}`. Das Attribut `boo` wird unter Verwendung des Werts aus dem Attribut `foo` aktualisiert. span-Elemente, die das Attribut `boo` nicht enthalten, werden nicht geändert.

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

Das folgende Beispiel zeigt, wie ein Attribut mit dem Schlüssel `credit_card` gelöscht wird.

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

Das folgende Beispiel zeigt, wie vorhandene Attributwerte mit einem Hash versehen werden.

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

Das folgende Beispiel zeigt, wie ein regulärer Ausdruck (RegEx) verwendet wird, um neue Attribute basierend auf dem Wert eines anderen Attributs erstellen.
Für `http.url = http://example.com/path?queryParam1=value1,queryParam2=value2` werden beispielsweise die folgenden Attribute eingefügt:
* httpProtocol: `http`
* httpDomain: `example.com`
* httpPath: `path`
* httpQueryParams: `queryParam1=value1,queryParam2=value2`
* http.url: *keine* Änderung

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

Das folgende Beispiel zeigt, wie span-Elemente verarbeitet werden, deren span-Name mit Mustern regulärer Ausdrücke übereinstimmt.
Dieser Prozessor entfernt das Attribut `token`. Er verschleiert das Attribut `password` in span-Elementen, deren span-Name mit `auth.*` übereinstimmt und mit `login.*` nicht übereinstimmt.

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


## <a name="span-processor-samples"></a>Beispiele für span-Prozessoren

### <a name="name-a-span"></a>Name einer Span

Das folgende Beispiel gibt die Werte der Attribute `db.svc`, `operation` und `id` an. Es formt einen neuen Namen für das span-Element, indem diese Elemente in der angegebenen Reihenfolge und durch den Wert `::` getrennt verwendet werden.
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

### <a name="extract-attributes-from-a-span-name"></a>Extrahieren von Attributen aus dem Namen eines span-Elements

Angenommen, der Eingabename des span-Elements lautet `/api/v1/document/12345678/update`. Das folgende Beispiel führt zum Ausgabenamen `/api/v1/document/{documentId}/update` des span-Elements. Dem span-Element wird das neue Attribut `documentId=12345678` hinzugefügt.
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

### <a name="extract-attributes-from-a-span-name-by-using-include-and-exclude"></a>Extrahieren von Attributen aus dem Namen eines span-Elements mithilfe von „include“ und „exclude“

Das folgende Beispiel zeigt, wie der Name eines span-Elements zu `{operation_website}` geändert wird. Ein Attribut mit dem Schlüssel `operation_website` und dem Attribut `{oldSpanName}` wird hinzugefügt, wenn das span-Element die folgenden Eigenschaften aufweist:
- Der Name des span-Elements enthält an einer beliebigen Stelle in der Zeichenfolge das Zeichen `/`.
- Der Name des span-Elements lautet nicht `donot/change`.
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
