---
title: 'Azure Service Fabric CLI: sfctl mesh code-package-log'
description: Erfahren Sie mehr über sfctl, die Azure Service Fabric-Befehlszeilenschnittstelle. Enthält eine Liste mit Befehlen zum Abrufen von Protokollen für ein angegebenes Codepaket.
author: jeffj6123
ms.topic: reference
ms.date: 9/17/2019
ms.author: jejarry
ms.openlocfilehash: 78edc9bb36b711f72300942bc9900b0fde7c51d2
ms.sourcegitcommit: f788bc6bc524516f186386376ca6651ce80f334d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/03/2020
ms.locfileid: "75646142"
---
# <a name="sfctl-mesh-code-package-log"></a>sfctl mesh code-package-log
Ruft die Protokolle für den Container des angegebenen Codepakets für das angegebene Dienstreplikat ab.

## <a name="commands"></a>Befehle

|Get-Help|BESCHREIBUNG|
| --- | --- |
| get | Ruft die Protokolle aus dem Container ab. |

## <a name="sfctl-mesh-code-package-log-get"></a>sfctl mesh code-package-log get
Ruft die Protokolle aus dem Container ab.

Ruft die Protokolle für den Container des angegebenen Codepakets des Dienstreplikats ab.

### <a name="arguments"></a>Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --app-name --application-name [erforderlich] | Der Namen der Anwendung. |
| --code-package-name           [erforderlich] | Der Name des Codepakets des Diensts. |
| --replica-name                [erforderlich] | Der Service Fabric-Replikatname. |
| --service-name                [erforderlich] | Der Name des Diensts. |
| --tail | Die Anzahl der Zeilen, die am Ende der Protokolle angezeigt werden sollen. Der Standardwert ist 100. „all“, um die vollständige Protokolle anzuzeigen. |

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