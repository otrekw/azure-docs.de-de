---
title: 'Azure Service Fabric-Befehlszeilenschnittstelle: sfctl rpm'
description: Erfahren Sie mehr über sfctl, die Azure Service Fabric-Befehlszeilenschnittstelle. Enthält eine Liste der Befehle für den Reparatur-Manager-Dienst.
author: jeffj6123
ms.topic: reference
ms.date: 9/17/2019
ms.author: jejarry
ms.openlocfilehash: 674970276046034d13801db7c1bb4ab5175385fb
ms.sourcegitcommit: f788bc6bc524516f186386376ca6651ce80f334d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/03/2020
ms.locfileid: "75639087"
---
# <a name="sfctl-rpm"></a>sfctl rpm
Ermöglicht es, Befehle an den Reparatur-Manager-Dienst abzufragen und zu senden.

## <a name="commands"></a>Befehle

|Get-Help|BESCHREIBUNG|
| --- | --- |
| approve-force | Erzwingt die Genehmigung das angegebenen Reparaturtasks. |
| delete | Löscht einen abgeschlossenen Reparaturtask. |
| list | Ruft eine Liste der Reparaturtasks ab, die dem angegebenen Filter entsprechen. |

## <a name="sfctl-rpm-approve-force"></a>sfctl rpm approve-force
Erzwingt die Genehmigung das angegebenen Reparaturtasks.

Diese API unterstützt die Service Fabric-Plattform. Sie ist nicht dazu vorgesehen, direkt aus Ihrem Code verwendet zu werden.

### <a name="arguments"></a>Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --task-id [erforderlich] | Die ID des Reparaturtasks. |
| --version | Die aktuelle Versionsnummer des Reparaturtasks. Ist der Wert ungleich null, ist die Anforderung nur dann erfolgreich, wenn dieser Wert mit der tatsächlichen aktuellen Version des Reparaturtasks übereinstimmt. Ist der Wert gleich null, wird keine Versionsüberprüfung ausgeführt. |

### <a name="global-arguments"></a>Globale Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --debug | Ausführlichkeit der Protokollierung erhöhen, um alle Debugprotokolle anzuzeigen. |
| --help -h | Zeigen Sie diese Hilfemeldung an, und schließen Sie sie. |
| --output -o | Ausgabeformat.  Zulässige Werte\: „json“, „jsonc“, „table“, „tsv“.  Standardwert\: „json“. |
| --query | JMESPath-Abfragezeichenfolge. Weitere Informationen und Beispiele finden Sie unter „http\://jmespath.org/“. |
| --verbose | Ausführlichkeit der Protokollierung erhöhen. „--debug“ für vollständige Debugprotokolle verwenden. |

## <a name="sfctl-rpm-delete"></a>sfctl rpm delete
Löscht einen abgeschlossenen Reparaturtask.

Diese API unterstützt die Service Fabric-Plattform. Sie ist nicht dazu vorgesehen, direkt aus Ihrem Code verwendet zu werden.

### <a name="arguments"></a>Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --task-id [erforderlich] | Die ID des abgeschlossenen Reparaturtasks, der gelöscht werden soll. |
| --version | Die aktuelle Versionsnummer des Reparaturtasks. Ist der Wert ungleich null, ist die Anforderung nur dann erfolgreich, wenn dieser Wert mit der tatsächlichen aktuellen Version des Reparaturtasks übereinstimmt. Ist der Wert gleich null, wird keine Versionsüberprüfung ausgeführt. |

### <a name="global-arguments"></a>Globale Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --debug | Ausführlichkeit der Protokollierung erhöhen, um alle Debugprotokolle anzuzeigen. |
| --help -h | Zeigen Sie diese Hilfemeldung an, und schließen Sie sie. |
| --output -o | Ausgabeformat.  Zulässige Werte\: „json“, „jsonc“, „table“, „tsv“.  Standardwert\: „json“. |
| --query | JMESPath-Abfragezeichenfolge. Weitere Informationen und Beispiele finden Sie unter „http\://jmespath.org/“. |
| --verbose | Ausführlichkeit der Protokollierung erhöhen. „--debug“ für vollständige Debugprotokolle verwenden. |

## <a name="sfctl-rpm-list"></a>sfctl rpm list
Ruft eine Liste der Reparaturtasks ab, die dem angegebenen Filter entsprechen.

Diese API unterstützt die Service Fabric-Plattform. Sie ist nicht dazu vorgesehen, direkt aus Ihrem Code verwendet zu werden.

### <a name="arguments"></a>Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --executor-filter | Der Name des Reparaturexecutors, dessen beanspruchte Tasks in der Liste enthalten sein sollen. |
| --state-filter | Eine bitweise OR-Operation für die folgenden Werte, die angibt, welche Taskstatus in der Ergebnisliste enthalten sein sollen. <br> 1 – „Created“ <br>2 – „Claimed“  <br>4 – „Preparing“  <br>8 – „Approved“  <br>16 – „Executing“  <br>32 – „Restoring“  <br>64 – „Completed“ |
| --task-id-filter | Das Reparaturtask-ID-Präfix, das abgeglichen werden soll. |

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