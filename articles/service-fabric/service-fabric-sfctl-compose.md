---
title: Azure Service Fabric-CLI – sfctl compose
description: Erfahren Sie mehr über sfctl, die Azure Service Fabric-Befehlszeilenschnittstelle. Enthält eine Liste der Befehle für Docker Compose-Anwendungen.
author: jeffj6123
ms.topic: reference
ms.date: 1/16/2020
ms.author: jejarry
ms.openlocfilehash: e84b35ff3dfa7be0c848a4d7b906c622d9b88914
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "86257340"
---
# <a name="sfctl-compose"></a>sfctl compose
Ermöglicht es, Docker Compose Anwendungen zu erstellen, zu löschen und zu verwalten.

## <a name="commands"></a>Befehle

|Get-Help|BESCHREIBUNG|
| --- | --- |
| create | Erstellt eine Service Fabric-Compose-Bereitstellung. |
| list | Ruft die Liste der Compose-Bereitstellungen ab, die in dem Service Fabric-Cluster erstellt wurden. |
| remove | Löscht eine vorhandene Service Fabric-Compose-Bereitstellung aus dem Cluster. |
| status | Ruft Informationen zu einer Service Fabric-Compose-Bereitstellung ab. |
| upgrade | Startet ein Aktualisieren einer Compose-Bereitstellung im Service Fabric-Cluster. |
| upgrade-rollback | Startet das Rollback des Upgrades einer Compose-Bereitstellung im Service Fabric-Cluster. |
| upgrade-status | Ruft Details zu dem neuesten Upgrade ab, das für diese Compose-Bereitstellung von Service Fabric durchgeführt wurde. |

## <a name="sfctl-compose-create"></a>sfctl compose create
Erstellt eine Service Fabric-Compose-Bereitstellung.

### <a name="arguments"></a>Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --deployment-name [erforderlich] | Der Name der Bereitstellung |
| --file-path [erforderlich] | Pfad zur Docker Compose-Zieldatei. |
| --encrypted-pass | Bewirkt, dass anstelle der Aufforderung zur Eingabe eines Containerregistrierungskennworts eine bereits verschlüsselte Passphrase verwendet wird. |
| --has-pass | Fordert zur Eingabe eines Kennworts für die Containerregistrierung auf. |
| --timeout -t | Standardwert\: 60. |
| --user | Der Benutzername, für den die Verbindung zur Containerregistrierung hergestellt werden soll. |

### <a name="global-arguments"></a>Globale Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --debug | Ausführlichkeit der Protokollierung erhöhen, um alle Debugprotokolle anzuzeigen. |
| --help -h | Zeigen Sie diese Hilfemeldung an, und schließen Sie sie. |
| --output -o | Ausgabeformat.  Zulässige Werte\: „json“, „jsonc“, „table“, „tsv“.  Standardwert\: „json“. |
| --query | JMESPath-Abfragezeichenfolge. Weitere Informationen und Beispiele finden Sie unter „http\://jmespath.org/“. |
| --verbose | Ausführlichkeit der Protokollierung erhöhen. „--debug“ für vollständige Debugprotokolle verwenden. |

## <a name="sfctl-compose-list"></a>sfctl compose list
Ruft die Liste der Compose-Bereitstellungen ab, die in dem Service Fabric-Cluster erstellt wurden.

Ruft den Status zu den Compose-Bereitstellungen ab, die im Service Fabric-Cluster erstellt wurden oder momentan erstellt werden. Die Antwort enthält den Namen, den Status und weitere Details über die Compose-Bereitstellungen. Wenn die Liste mit den Bereitstellungen nicht auf eine Seite passt, werden sowohl eine Seite mit Ergebnissen als auch ein Fortsetzungstoken zurückgegeben, über das zur nächsten Seite gewechselt werden kann.

### <a name="arguments"></a>Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --continuation-token | Der Parameter „continuation-token“ (Fortsetzungstoken) wird dazu verwendet, den nächsten Satz von Ergebnissen abzurufen. Ein Fortsetzungstoken mit einem nicht leeren Wert wird in die Antwort der API eingefügt, wenn die Ergebnisse aus dem System nicht in eine einzige Antwort passen. Wird dieser Wert an den nächsten API-Aufruf übergeben, gibt die API den nächsten Satz von Ergebnissen zurück. Gibt es keine weiteren Ergebnisse, enthält das Fortsetzungstoken keinen Wert. Der Wert dieses Parameters darf nicht als URL codiert sein. |
| --max-results | Die maximale Anzahl von Ergebnissen, die als Teil der seitenweisen Abfragen zurückgegeben werden sollen. Dieser Parameter definiert die obere Grenze für die Anzahl von zurückgegebenen Ergebnissen. Es können weniger Ergebnisse zurückgegeben werden, als dieser maximalen Anzahl entspricht. Dies ist der Fall, wenn die Ergebnisse wegen der Größenbeschränkungen, die für Meldungen in der Konfiguration definiert sind, nicht in die jeweilige Meldung passen. Ist dieser Parameter gleich null oder nicht angegeben, enthält die seitenweise Abfrage so viele Ergebnisse, wie in die Rückgabemeldung passen. |
| --timeout -t | Der Servertimeout für die Ausführung des Vorgangs in Sekunden. Dieser Timeout gibt die Zeitdauer an, die der Client bereit ist, auf den Abschluss des angeforderten Vorgangs zu warten. Der Standardwert für diesen Parameter ist 60 Sekunden.  Standardwert\: 60. |

### <a name="global-arguments"></a>Globale Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --debug | Ausführlichkeit der Protokollierung erhöhen, um alle Debugprotokolle anzuzeigen. |
| --help -h | Zeigen Sie diese Hilfemeldung an, und schließen Sie sie. |
| --output -o | Ausgabeformat.  Zulässige Werte\: „json“, „jsonc“, „table“, „tsv“.  Standardwert\: „json“. |
| --query | JMESPath-Abfragezeichenfolge. Weitere Informationen und Beispiele finden Sie unter „http\://jmespath.org/“. |
| --verbose | Ausführlichkeit der Protokollierung erhöhen. „--debug“ für vollständige Debugprotokolle verwenden. |

## <a name="sfctl-compose-remove"></a>sfctl compose remove
Löscht eine vorhandene Service Fabric-Compose-Bereitstellung aus dem Cluster.

Löscht eine vorhandene Service Fabric-Compose-Bereitstellung.

### <a name="arguments"></a>Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --deployment-name [erforderlich] | Die Identität (ID) der Bereitstellung. |
| --timeout -t | Der Servertimeout für die Ausführung des Vorgangs in Sekunden. Dieser Timeout gibt die Zeitdauer an, die der Client bereit ist, auf den Abschluss des angeforderten Vorgangs zu warten. Der Standardwert für diesen Parameter ist 60 Sekunden.  Standardwert\: 60. |

### <a name="global-arguments"></a>Globale Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --debug | Ausführlichkeit der Protokollierung erhöhen, um alle Debugprotokolle anzuzeigen. |
| --help -h | Zeigen Sie diese Hilfemeldung an, und schließen Sie sie. |
| --output -o | Ausgabeformat.  Zulässige Werte\: „json“, „jsonc“, „table“, „tsv“.  Standardwert\: „json“. |
| --query | JMESPath-Abfragezeichenfolge. Weitere Informationen und Beispiele finden Sie unter „http\://jmespath.org/“. |
| --verbose | Ausführlichkeit der Protokollierung erhöhen. „--debug“ für vollständige Debugprotokolle verwenden. |

## <a name="sfctl-compose-status"></a>sfctl compose status
Ruft Informationen zu einer Service Fabric-Compose-Bereitstellung ab.

Gibt den Status der Compose-Bereitstellung zurück, die im Service Fabric-Cluster erstellt wurde oder gerade erstellt wird und deren Name mit dem übereinstimmt, der als Parameter angegebenen wurde. Die Antwort enthält den Namen, den Status und weitere Details über die Compose-Bereitstellung.

### <a name="arguments"></a>Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --deployment-name [erforderlich] | Die Identität (ID) der Bereitstellung. |
| --timeout -t | Der Servertimeout für die Ausführung des Vorgangs in Sekunden. Dieser Timeout gibt die Zeitdauer an, die der Client bereit ist, auf den Abschluss des angeforderten Vorgangs zu warten. Der Standardwert für diesen Parameter ist 60 Sekunden.  Standardwert\: 60. |

### <a name="global-arguments"></a>Globale Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --debug | Ausführlichkeit der Protokollierung erhöhen, um alle Debugprotokolle anzuzeigen. |
| --help -h | Zeigen Sie diese Hilfemeldung an, und schließen Sie sie. |
| --output -o | Ausgabeformat.  Zulässige Werte\: „json“, „jsonc“, „table“, „tsv“.  Standardwert\: „json“. |
| --query | JMESPath-Abfragezeichenfolge. Weitere Informationen und Beispiele finden Sie unter „http\://jmespath.org/“. |
| --verbose | Ausführlichkeit der Protokollierung erhöhen. „--debug“ für vollständige Debugprotokolle verwenden. |

## <a name="sfctl-compose-upgrade"></a>sfctl compose upgrade
Startet ein Aktualisieren einer Compose-Bereitstellung im Service Fabric-Cluster.

Überprüft die für das Upgrade angegebenen Parameter und startet die Durchführung des Upgrades für die Bereitstellung, sofern die Parameter gültig sind.

### <a name="arguments"></a>Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --deployment-name [erforderlich] | Der Name der Bereitstellung |
| --file-path [erforderlich] | Pfad zur Docker Compose-Zieldatei. |
| --default-svc-type-health-map | JSON-codiertes Wörterbuch, in dem die Integritätsrichtlinie beschrieben ist, mit der die Integrität von Diensten ausgewertet wird. |
| --encrypted-pass | Bewirkt, dass anstelle der Aufforderung zur Eingabe eines Containerregistrierungskennworts eine bereits verschlüsselte Passphrase verwendet wird. |
| --failure-action | Folgende Werte sind möglich\: „Invalid“, „Rollback“, „Manual“. |
| --force-restart | Während eines Upgrades wird ein erzwungener Neustart der Prozesse durchgeführt, auch wenn sich die Codeversion nicht geändert hat. <br><br> Das Upgrade ändert nur die Konfiguration oder die Daten. |
| --has-pass | Fordert zur Eingabe eines Kennworts für die Containerregistrierung auf. |
| --health-check-retry | Die Zeitspanne zwischen den Versuchen, Integritätsprüfungen durchzuführen, wenn die Anwendung oder der Cluster nicht fehlerfrei ist. |
| --health-check-stable | Die Zeitspanne, während der die Anwendung oder der Cluster fehlerfrei bleiben muss, bevor das Upgrade mit der nächsten Upgradedomäne fortgesetzt wird. <br><br> Wird zuerst als Zeichenfolge interpretiert, die für eine ISO 8601-Dauer steht. Wenn dies nicht erfolgreich ist, erfolgt die Interpretation als Zahl, die für die Gesamtzahl an Millisekunden steht. |
| --health-check-wait | Die Zeitspanne, während der nach dem Abschließen einer Upgradedomäne gewartet werden soll, bevor der Integritätsprüfungsprozess gestartet wird. |
| --replica-set-check | Die maximale Zeitspanne, während der die Verarbeitung einer Upgradedomäne blockiert und Verfügbarkeitsverlust verhindert wird, wenn es unerwartete Probleme gibt. <br><br> Wenn dieses Timeout abläuft, wird die Verarbeitung der Upgradedomäne unabhängig von Verfügbarkeitsverlustproblemen fortgesetzt. Das Timeout wird beim Start jeder Upgradedomäne zurückgesetzt. Gültige Werte liegen zwischen 0 und 42.949.672.925 (einschließlich). |
| --svc-type-health-map | JSON-codierte Liste der Objekte, in denen die Integritätsrichtlinien beschrieben sind, mit denen die Integrität verschiedener Diensttypen bewertet wird. |
| --timeout -t | Standardwert\: 60. |
| --unhealthy-app | Der maximal zulässige Prozentsatz fehlerhafter Anwendungen, bevor ein Fehler gemeldet wird. <br><br> Soll es z. B.zulässig sein, dass 10 % der Anwendungen fehlerhaft sind, muss dieser Wert gleich „10“ sein. Der Prozentsatz entspricht dem maximalen tolerierten Prozentsatz an Anwendungen, die fehlerhaft sein können, bevor der Cluster als fehlerhaft behandelt wird. Wird der Prozentsatz eingehalten, gibt es aber mindestens eine fehlerhafte Anwendung, wird die Integrität als „Warning“ ausgewertet. Dieser wird berechnet, indem die Anzahl von fehlerhaften Anwendungen durch die Gesamtanzahl von Anwendungsinstanzen im Cluster dividiert wird. |
| --upgrade-domain-timeout | Die Zeitspanne, während der jede Upgradedomäne abgeschlossen werden muss, bevor die Fehleraktion (FailureAction) ausgeführt wird. <br><br> Wird zuerst als Zeichenfolge interpretiert, die für eine ISO 8601-Dauer steht. Wenn dies nicht erfolgreich ist, erfolgt die Interpretation als Zahl, die für die Gesamtzahl an Millisekunden steht. |
| --upgrade-kind | Standardwert\: „Rolling“. |
| --upgrade-mode | Folgende Werte sind möglich\: „Invalid“, „UnmonitoredAuto“, „UnmonitoredManual“, „Monitored“.  Standardwert\: „UnmonitoredAuto“. |
| --upgrade-timeout | Die Zeitspanne, während der das gesamte Upgrade abgeschlossen werden muss, bevor die Fehleraktion (FailureAction) ausgeführt wird. <br><br> Wird zuerst als Zeichenfolge interpretiert, die für eine ISO 8601-Dauer steht. Wenn dies nicht erfolgreich ist, erfolgt die Interpretation als Zahl, die für die Gesamtzahl an Millisekunden steht. |
| --user | Der Benutzername, für den die Verbindung zur Containerregistrierung hergestellt werden soll. |
| --warning-as-error | Gibt an, ob Warnungen mit demselben Schweregrad wie Fehler berücksichtigt werden. |

### <a name="global-arguments"></a>Globale Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --debug | Ausführlichkeit der Protokollierung erhöhen, um alle Debugprotokolle anzuzeigen. |
| --help -h | Zeigen Sie diese Hilfemeldung an, und schließen Sie sie. |
| --output -o | Ausgabeformat.  Zulässige Werte\: „json“, „jsonc“, „table“, „tsv“.  Standardwert\: „json“. |
| --query | JMESPath-Abfragezeichenfolge. Weitere Informationen und Beispiele finden Sie unter „http\://jmespath.org/“. |
| --verbose | Ausführlichkeit der Protokollierung erhöhen. „--debug“ für vollständige Debugprotokolle verwenden. |

## <a name="sfctl-compose-upgrade-rollback"></a>sfctl compose upgrade-rollback
Startet das Rollback des Upgrades einer Compose-Bereitstellung im Service Fabric-Cluster.

Rollback des Upgrades einer Service Fabric Compose-Bereitstellung.

### <a name="arguments"></a>Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --deployment-name [erforderlich] | Die Identität (ID) der Bereitstellung. |
| --timeout -t | Der Servertimeout für die Ausführung des Vorgangs in Sekunden. Dieser Timeout gibt die Zeitdauer an, die der Client bereit ist, auf den Abschluss des angeforderten Vorgangs zu warten. Der Standardwert für diesen Parameter ist 60 Sekunden.  Standardwert\: 60. |

### <a name="global-arguments"></a>Globale Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --debug | Ausführlichkeit der Protokollierung erhöhen, um alle Debugprotokolle anzuzeigen. |
| --help -h | Zeigen Sie diese Hilfemeldung an, und schließen Sie sie. |
| --output -o | Ausgabeformat.  Zulässige Werte\: „json“, „jsonc“, „table“, „tsv“.  Standardwert\: „json“. |
| --query | JMESPath-Abfragezeichenfolge. Weitere Informationen und Beispiele finden Sie unter „http\://jmespath.org/“. |
| --verbose | Ausführlichkeit der Protokollierung erhöhen. „--debug“ für vollständige Debugprotokolle verwenden. |

## <a name="sfctl-compose-upgrade-status"></a>sfctl compose upgrade-status
Ruft Details zu dem neuesten Upgrade ab, das für diese Compose-Bereitstellung von Service Fabric durchgeführt wurde.

Gibt die Informationen über den Status des Upgrades für Compose-Bereitstellungen zusammen mit Details zurück, die beim Debuggen von Anwendungsfehlern unterstützen.

### <a name="arguments"></a>Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --deployment-name [erforderlich] | Die Identität (ID) der Bereitstellung. |
| --timeout -t | Der Servertimeout für die Ausführung des Vorgangs in Sekunden. Dieser Timeout gibt die Zeitdauer an, die der Client bereit ist, auf den Abschluss des angeforderten Vorgangs zu warten. Der Standardwert für diesen Parameter ist 60 Sekunden.  Standardwert\: 60. |

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
