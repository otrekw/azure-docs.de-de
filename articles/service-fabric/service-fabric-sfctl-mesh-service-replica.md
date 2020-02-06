---
title: 'Azure Service Fabric CLI: sfctl mesh service-replica'
description: Erfahren Sie mehr über sfctl, die Azure Service Fabric-Befehlszeilenschnittstelle. Enthält eine Liste der Befehle zum Abrufen von Replikatdetails für eine Anwendungsressource.
author: jeffj6123
ms.topic: reference
ms.date: 1/16/2020
ms.author: jejarry
ms.openlocfilehash: ac604ec0a854964746ef34627b054e853662265f
ms.sourcegitcommit: 67e9f4cc16f2cc6d8de99239b56cb87f3e9bff41
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/31/2020
ms.locfileid: "76905938"
---
# <a name="sfctl-mesh-service-replica"></a>sfctl mesh service-replica
Ruft Replikatdetails ab und listet Replikate eines angegebenen Diensts in einer Anwendungsressource auf.

## <a name="commands"></a>Befehle

|Get-Help|Beschreibung|
| --- | --- |
| list | Listet alle Replikate eines Diensts auf. |
| show | Ruft das angegebene Replikat des Diensts einer Anwendung ab. |

## <a name="sfctl-mesh-service-replica-list"></a>sfctl mesh service-replica list
Listet alle Replikate eines Diensts auf.

Ruft die Informationen zu allen Replikaten eines Diensts ab. Die Informationen umfassen die Beschreibung und andere Eigenschaften des Dienstreplikats.

### <a name="arguments"></a>Argumente

|Argument|Beschreibung|
| --- | --- |
| --app-name --application-name [erforderlich] | Der Namen der Anwendung. |
| --service-name                [erforderlich] | Der Name des Diensts. |

### <a name="global-arguments"></a>Globale Argumente

|Argument|Beschreibung|
| --- | --- |
| --debug | Ausführlichkeit der Protokollierung erhöhen, um alle Debugprotokolle anzuzeigen. |
| --help -h | Zeigen Sie diese Hilfemeldung an, und schließen Sie sie. |
| --output -o | Ausgabeformat.  Zulässige Werte\: „json“, „jsonc“, „table“, „tsv“.  Standardwert\: „json“. |
| --query | JMESPath-Abfragezeichenfolge. Weitere Informationen und Beispiele finden Sie unter „http\://jmespath.org/“. |
| --verbose | Ausführlichkeit der Protokollierung erhöhen. „--debug“ für vollständige Debugprotokolle verwenden. |

## <a name="sfctl-mesh-service-replica-show"></a>sfctl mesh service-replica show
Ruft das angegebene Replikat des Diensts einer Anwendung ab.

Ruft die Informationen zum Dienstreplikat mit dem angegebenen Namen ab. Die Informationen umfassen die Beschreibung und andere Eigenschaften des Dienstreplikats.

### <a name="arguments"></a>Argumente

|Argument|Beschreibung|
| --- | --- |
| --app-name --application-name [erforderlich] | Der Namen der Anwendung. |
| --name -n                     [erforderlich] | Der Name des Dienstreplikats. |
| --service-name                [erforderlich] | Der Name des Diensts. |

### <a name="global-arguments"></a>Globale Argumente

|Argument|Beschreibung|
| --- | --- |
| --debug | Ausführlichkeit der Protokollierung erhöhen, um alle Debugprotokolle anzuzeigen. |
| --help -h | Zeigen Sie diese Hilfemeldung an, und schließen Sie sie. |
| --output -o | Ausgabeformat.  Zulässige Werte\: „json“, „jsonc“, „table“, „tsv“.  Standardwert\: „json“. |
| --query | JMESPath-Abfragezeichenfolge. Weitere Informationen und Beispiele finden Sie unter „http\://jmespath.org/“. |
| --verbose | Ausführlichkeit der Protokollierung erhöhen. „--debug“ für vollständige Debugprotokolle verwenden. |


## <a name="next-steps"></a>Nächste Schritte
- [Einrichten](service-fabric-cli.md) der Service Fabric-Befehlszeilenschnittstelle
- Informationen zum Verwenden der Service Fabric-Befehlszeilenschnittstelle mit den [Beispielskripts](/azure/service-fabric/scripts/sfctl-upgrade-application)