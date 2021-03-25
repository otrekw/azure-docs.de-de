---
title: 'Azure Service Fabric-Befehlszeilenschnittstelle: sfctl rpm'
description: Erfahren Sie mehr über sfctl, die Azure Service Fabric-Befehlszeilenschnittstelle. Enthält eine Liste der Befehle für den Reparatur-Manager-Dienst.
author: jeffj6123
ms.topic: reference
ms.date: 1/16/2020
ms.author: jejarry
ms.openlocfilehash: 7317fd66303aaabf5232106aa7391439880bebaf
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "86260281"
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
| --state-filter | Eine bitweise OR-Operation für die folgenden Werte, die angibt, welche Taskstatus in der Ergebnisliste enthalten sein sollen. <ul><li>1 – „Created“</li><li>2 – „Claimed“</li><li>4 – „Preparing“</li><li>8 – „Approved“</li><li>16 – „Executing“</li><li>32 – „Restoring“</li><li>64 – „Completed“</li></ul>
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
- Informationen zum Verwenden der Service Fabric-Befehlszeilenschnittstelle mit den [Beispielskripts](./scripts/sfctl-upgrade-application.md)
