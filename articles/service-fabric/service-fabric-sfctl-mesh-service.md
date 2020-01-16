---
title: Azure Service Fabric-CLI – sfctl mesh service
description: Erfahren Sie mehr über sfctl, die Azure Service Fabric-Befehlszeilenschnittstelle. Enthält eine Liste der Befehle zum Abrufen von Dienstdetails für eine Anwendungsressource.
author: jeffj6123
ms.topic: reference
ms.date: 9/17/2019
ms.author: jejarry
ms.openlocfilehash: 1ae04d054a254746d59b85f4fe366cebf19e3faf
ms.sourcegitcommit: f788bc6bc524516f186386376ca6651ce80f334d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/03/2020
ms.locfileid: "75646091"
---
# <a name="sfctl-mesh-service"></a>sfctl mesh service
Ruft Dienstdetails ab und listet Dienste einer Anwendungsressource auf.

## <a name="commands"></a>Befehle

|Get-Help|BESCHREIBUNG|
| --- | --- |
| list | Listet alle Dienstressourcen auf. |
| show | Ruft die Dienstressource mit dem angegebenen Namen ab. |

## <a name="sfctl-mesh-service-list"></a>sfctl mesh service list
Listet alle Dienstressourcen auf.

Ruft die Informationen zu allen Diensten einer Anwendungsressource ab. Die Informationen umfassen die Beschreibung und andere Eigenschaften des Diensts.

### <a name="arguments"></a>Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --app-name --application-name [erforderlich] | Der Namen der Anwendung. |

### <a name="global-arguments"></a>Globale Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --debug | Ausführlichkeit der Protokollierung erhöhen, um alle Debugprotokolle anzuzeigen. |
| --help -h | Zeigen Sie diese Hilfemeldung an, und schließen Sie sie. |
| --output -o | Ausgabeformat.  Zulässige Werte\: „json“, „jsonc“, „table“, „tsv“.  Standardwert\: „json“. |
| --query | JMESPath-Abfragezeichenfolge. Weitere Informationen und Beispiele finden Sie unter „http\://jmespath.org/“. |
| --verbose | Ausführlichkeit der Protokollierung erhöhen. „--debug“ für vollständige Debugprotokolle verwenden. |

## <a name="sfctl-mesh-service-show"></a>sfctl mesh service show
Ruft die Dienstressource mit dem angegebenen Namen ab.

Ruft die Informationen zur Dienstressource mit dem angegebenen Namen ab. Die Informationen umfassen die Beschreibung und andere Eigenschaften des Diensts.

### <a name="arguments"></a>Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --app-name --application-name [erforderlich] | Der Namen der Anwendung. |
| --name -n                     [erforderlich] | Der Name des Diensts. |

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
- Informationen zum Verwenden der Service Fabric-Befehlszeilenschnittstelle mit den [Beispielskripts](/azure/service-fabric/scripts/sfctl-upgrade-application)