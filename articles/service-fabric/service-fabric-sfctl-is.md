---
title: 'Azure Service Fabric CLI: sfctl is'
description: Hier erfahren Sie mehr über sfctl, die Azure Service Fabric-Befehlszeilenschnittstelle. Enthält eine Liste der Befehle zum Verwalten der Infrastruktur.
author: jeffj6123
ms.topic: reference
ms.date: 9/17/2019
ms.author: jejarry
ms.openlocfilehash: c3c98e8da0c2a5449296a0fd108977ec0633f83d
ms.sourcegitcommit: f788bc6bc524516f186386376ca6651ce80f334d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/03/2020
ms.locfileid: "75639121"
---
# <a name="sfctl-is"></a>sfctl is
Ermöglicht es, Befehle an den Infrastrukturdienst abzufragen und zu senden.

## <a name="commands"></a>Befehle

|Get-Help|BESCHREIBUNG|
| --- | --- |
| command | Ruft einen Verwaltungsbefehl für die angegebene Infrastrukturdienstinstanz auf. |
| Abfrage | Ruft eine schreibgeschützte Abfrage für die angegebene Infrastrukturdienstinstanz auf. |

## <a name="sfctl-is-command"></a>sfctl is command
Ruft einen Verwaltungsbefehl für die angegebene Infrastrukturdienstinstanz auf.

Für Cluster, für die mindestens eine Instanz des Infrastrukturdiensts konfiguriert ist, bietet diese API eine Möglichkeit, infrastrukturspezifische Befehle an eine bestimmte Instanz des Infrastrukturdiensts zu senden. Die verfügbaren Befehle und ihre entsprechenden Antwortformate variieren je nach der Infrastruktur, auf der der Cluster ausgeführt wird. Diese API unterstützt die Service Fabric-Plattform. Sie ist nicht dazu vorgesehen, direkt aus Ihrem Code verwendet zu werden.

### <a name="arguments"></a>Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --command [erforderlich] | Der Text des Befehls, der aufgerufen werden soll. Der Inhalt des Befehls ist infrastrukturspezifisch. |
| --service-id | Die Identität des Infrastrukturdiensts. <br><br> Dies ist der vollständige Name des Infrastrukturdiensts ohne das URI-Schema „fabric\:“. Dieser Parameter ist nur für den Cluster erforderlich, in dem mehrere Instanzen des Infrastrukturdiensts ausgeführt werden. |
| --timeout -t | Standardwert\: 60. |

### <a name="global-arguments"></a>Globale Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --debug | Ausführlichkeit der Protokollierung erhöhen, um alle Debugprotokolle anzuzeigen. |
| --help -h | Zeigen Sie diese Hilfemeldung an, und schließen Sie sie. |
| --output -o | Ausgabeformat.  Zulässige Werte\: „json“, „jsonc“, „table“, „tsv“.  Standardwert\: „json“. |
| --query | JMESPath-Abfragezeichenfolge. Weitere Informationen und Beispiele finden Sie unter „http\://jmespath.org/“. |
| --verbose | Ausführlichkeit der Protokollierung erhöhen. „--debug“ für vollständige Debugprotokolle verwenden. |

## <a name="sfctl-is-query"></a>sfctl is query
Ruft eine schreibgeschützte Abfrage für die angegebene Infrastrukturdienstinstanz auf.

Für Cluster, für die mindestens eine Instanz des Infrastrukturdiensts konfiguriert ist, bietet diese API eine Möglichkeit, infrastrukturspezifische Abfragen an eine bestimmte Instanz des Infrastrukturdiensts zu senden. Die verfügbaren Befehle und ihre entsprechenden Antwortformate variieren je nach der Infrastruktur, auf der der Cluster ausgeführt wird. Diese API unterstützt die Service Fabric-Plattform. Sie ist nicht dazu vorgesehen, direkt aus Ihrem Code verwendet zu werden.

### <a name="arguments"></a>Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --command [erforderlich] | Der Text des Befehls, der aufgerufen werden soll. Der Inhalt des Befehls ist infrastrukturspezifisch. |
| --service-id | Die Identität des Infrastrukturdiensts. <br><br> Dies ist der vollständige Name des Infrastrukturdiensts ohne das URI-Schema „fabric\:“. Dieser Parameter ist nur für den Cluster erforderlich, in dem mehrere Instanzen des Infrastrukturdiensts ausgeführt werden. |
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
- Informationen zum Verwenden der Service Fabric-Befehlszeilenschnittstelle mit den [Beispielskripts](/azure/service-fabric/scripts/sfctl-upgrade-application)