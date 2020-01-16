---
title: 'Azure Service Fabric-Befehlszeilenschnittstelle: sfctl mesh network'
description: Erfahren Sie mehr über sfctl, die Azure Service Fabric-Befehlszeilenschnittstelle. Enthält eine Liste der Befehle zum Abrufen und Löschen von Service Fabric Mesh-Netzwerkressourcen.
author: jeffj6123
ms.topic: reference
ms.date: 9/17/2019
ms.author: jejarry
ms.openlocfilehash: 2d9d86d41dc031cca2730011c2322e9feb30c827
ms.sourcegitcommit: f788bc6bc524516f186386376ca6651ce80f334d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/03/2020
ms.locfileid: "75646125"
---
# <a name="sfctl-mesh-network"></a>sfctl mesh network
Ruft die Mesh-Netzwerkressourcen ab bzw. löscht diese.

## <a name="commands"></a>Befehle

|Get-Help|BESCHREIBUNG|
| --- | --- |
| delete | Löscht die Netzwerkressource. |
| list | Listet alle Netzwerkressourcen auf. |
| show | Ruft die Netzwerkressource mit dem angegebenen Namen ab. |

## <a name="sfctl-mesh-network-delete"></a>sfctl mesh network delete
Löscht die Netzwerkressource.

Löscht die Netzwerkressource, die durch den Namen identifiziert wird.

### <a name="arguments"></a>Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --name -n [Erforderlich] | Der Name des Netzwerks. |

### <a name="global-arguments"></a>Globale Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --debug | Ausführlichkeit der Protokollierung erhöhen, um alle Debugprotokolle anzuzeigen. |
| --help -h | Zeigen Sie diese Hilfemeldung an, und schließen Sie sie. |
| --output -o | Ausgabeformat.  Zulässige Werte\: „json“, „jsonc“, „table“, „tsv“.  Standardwert\: „json“. |
| --query | JMESPath-Abfragezeichenfolge. Weitere Informationen und Beispiele finden Sie unter „http\://jmespath.org/“. |
| --verbose | Ausführlichkeit der Protokollierung erhöhen. „--debug“ für vollständige Debugprotokolle verwenden. |

## <a name="sfctl-mesh-network-list"></a>sfctl mesh network list
Listet alle Netzwerkressourcen auf.

Ruft die Informationen zu allen Netzwerkressourcen in einer bestimmten Ressourcengruppe ab. Die Informationen umfassen die Beschreibung und andere Eigenschaften des Netzwerks.

### <a name="global-arguments"></a>Globale Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --debug | Ausführlichkeit der Protokollierung erhöhen, um alle Debugprotokolle anzuzeigen. |
| --help -h | Zeigen Sie diese Hilfemeldung an, und schließen Sie sie. |
| --output -o | Ausgabeformat.  Zulässige Werte\: „json“, „jsonc“, „table“, „tsv“.  Standardwert\: „json“. |
| --query | JMESPath-Abfragezeichenfolge. Weitere Informationen und Beispiele finden Sie unter „http\://jmespath.org/“. |
| --verbose | Ausführlichkeit der Protokollierung erhöhen. „--debug“ für vollständige Debugprotokolle verwenden. |

## <a name="sfctl-mesh-network-show"></a>sfctl mesh network show
Ruft die Netzwerkressource mit dem angegebenen Namen ab.

Ruft die Informationen zur Netzwerkressource mit dem angegebenen Namen ab. Die Informationen umfassen die Beschreibung und andere Eigenschaften des Netzwerks.

### <a name="arguments"></a>Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --name -n [Erforderlich] | Der Name des Netzwerks. |

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