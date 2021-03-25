---
title: 'Azure Service Fabric-CLI: sfctl container'
description: Erfahren Sie mehr über sfctl, die Azure Service Fabric-Befehlszeilenschnittstelle. Enthält eine Liste der Befehle für Container.
author: jeffj6123
ms.topic: reference
ms.date: 1/16/2020
ms.author: jejarry
ms.openlocfilehash: f82883b68ab911fb0b89fc117d9a9d77e05a781a
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "86245890"
---
# <a name="sfctl-container"></a>sfctl container
Führt containerbezogene Befehle auf einem Clusterknoten aus.

## <a name="commands"></a>Befehle

|Get-Help|BESCHREIBUNG|
| --- | --- |
| invoke-api | Ruft die Container-API für einen Container auf, der auf einem Service Fabric-Knoten für das jeweilige Codepaket bereitgestellt wurde. |
| logs | Ruft die Containerprotokolle für Container ab, die auf einem Service Fabric-Knoten bereitgestellt wurden. |

## <a name="sfctl-container-invoke-api"></a>sfctl container invoke-api
Ruft die Container-API für einen Container auf, der auf einem Service Fabric-Knoten für das jeweilige Codepaket bereitgestellt wurde.

### <a name="arguments"></a>Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --application-id           [erforderlich] | Die Identität (ID) der Anwendung. <br><br> Dies ist üblicherweise der vollständige Name der Anwendung ohne das URI-Schema „fabric\:“. Ab Version 6.0 wird für hierarchische Namen das Zeichen „\~“ als Trennzeichen verwendet. Hat eine Anwendung beispielsweise den Namen „fabric\:/meineapp/app1“, weist die Anwendungsidentität in 6.0 und höher den Wert „meineapp\~app1“ und in früheren Versionen den Wert „meineapp/app1“ auf. |
| --code-package-instance-id [erforderlich] | Eine ID, die eine Codepaketinstanz eindeutig identifiziert, die auf einem Service Fabric-Knoten bereitgestellt wird. <br><br> Kann über „service code-package-list“ abgerufen werden. |
| --code-package-name        [erforderlich] | Der Name des Codepakets, das im Dienstmanifest angegeben ist, das als Bestandteil eines Anwendungstyps in einem Service Fabric-Cluster registriert ist. |
| --container-api-uri-path   [erforderlich] | Pfad zum URI der Container-REST-API, verwenden Sie „{ID}“ statt des Namens bzw. der ID des Containers. |
| --node-name [erforderlich] | Der Name des Knotens. |
| --service-manifest-name [erforderlich] | Der Name eines Dienstmanifests, das als Bestandteil eines Anwendungstyps in einem Service Fabric-Cluster registriert ist. |
| --container-api-body | HTTP-Anforderungstext für die Container-REST-API. |
| --container-api-content-type | Inhaltstyp der Container-REST-API, Standardwert ist „application/json“. |
| --container-api-http-verb | HTTP-Verb für die Container-REST-API, Standardwert ist GET. |
| --timeout -t | Standardwert\: 60. |

### <a name="global-arguments"></a>Globale Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --debug | Ausführlichkeit der Protokollierung erhöhen, um alle Debugprotokolle anzuzeigen. |
| --help -h | Zeigen Sie diese Hilfemeldung an, und schließen Sie sie. |
| --output -o | Ausgabeformat.  Zulässige Werte\: „json“, „jsonc“, „table“, „tsv“.  Standardwert\: „json“. |
| --query | JMESPath-Abfragezeichenfolge. Weitere Informationen und Beispiele finden Sie unter „http\://jmespath.org/“. |
| --verbose | Ausführlichkeit der Protokollierung erhöhen. „--debug“ für vollständige Debugprotokolle verwenden. |

## <a name="sfctl-container-logs"></a>sfctl container logs
Ruft die Containerprotokolle für Container ab, die auf einem Service Fabric-Knoten bereitgestellt wurden.

### <a name="arguments"></a>Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --application-id           [erforderlich] | Die Identität (ID) der Anwendung. <br><br> Dies ist üblicherweise der vollständige Name der Anwendung ohne das URI-Schema „fabric\:“. Ab Version 6.0 wird für hierarchische Namen das Zeichen „\~“ als Trennzeichen verwendet. Hat eine Anwendung beispielsweise den Namen „fabric\:/meineapp/app1“, weist die Anwendungsidentität in 6.0 und höher den Wert „meineapp\~app1“ und in früheren Versionen den Wert „meineapp/app1“ auf. |
| --code-package-instance-id [erforderlich] | Instanz-ID des Codepakets, die mit „service code-package-list“ abgerufen werden kann. |
| --code-package-name        [erforderlich] | Der Name des Codepakets, das im Dienstmanifest angegeben ist, das als Bestandteil eines Anwendungstyps in einem Service Fabric-Cluster registriert ist. |
| --node-name [erforderlich] | Der Name des Knotens. |
| --service-manifest-name [erforderlich] | Der Name eines Dienstmanifests, das als Bestandteil eines Anwendungstyps in einem Service Fabric-Cluster registriert ist. |
| --tail | Die Anzahl der Zeilen, die am Ende der Protokolle angezeigt werden sollen. Der Standardwert ist 100. „all“, um die vollständige Protokolle anzuzeigen. |
| --timeout -t | Standardwert\: 60. |

### <a name="global-arguments"></a>Globale Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --debug | Ausführlichkeit der Protokollierung erhöhen, um alle Debugprotokolle anzuzeigen. |
| --help -h | Zeigen Sie diese Hilfemeldung an, und schließen Sie sie. |
| --output -o | Ausgabeformat.  Zulässige Werte\: „json“, „jsonc“, „table“, „tsv“.  Standardwert\: „json“. |
| --query | JMESPath-Abfragezeichenfolge. Weitere Informationen und Beispiele finden Sie unter „http\://jmespath.org/“. |
| --verbose | Ausführlichkeit der Protokollierung erhöhen. „--debug“ für vollständige Debugprotokolle verwenden. |


## <a name="next-steps"></a>Nächste Schritte
- [Einrichten](service-fabric-cli.md) der Service Fabric-Befehlszeilenschnittstelle
- Informationen zum Verwenden der Service Fabric-Befehlszeilenschnittstelle mit den [Beispielskripts](./scripts/sfctl-upgrade-application.md)
