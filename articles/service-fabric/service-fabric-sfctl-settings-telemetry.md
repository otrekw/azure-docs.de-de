---
title: 'Azure Service Fabric CLI: sfctl settings telemetry'
description: Erfahren Sie mehr über sfctl, die Azure Service Fabric-Befehlszeilenschnittstelle. Enthält eine Liste mit Befehlen zum Konfigurieren der sfctl-Telemetrie.
author: jeffj6123
ms.topic: reference
ms.date: 1/16/2020
ms.author: jejarry
ms.openlocfilehash: 6af5fa944ef399756f9e890ddd77a7f5f32e2bfb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "76903016"
---
# <a name="sfctl-settings-telemetry"></a>sfctl settings telemetry
Konfiguriert Telemetrieeinstellungen, die für diese Instanz von sfctl lokal sind.

sfctl telemetry erfasst den Befehlsnamen ohne angegebene Parameter oder deren Werte, sfctl-Version, Betriebssystemtyp, Python-Version, den Erfolg oder Fehler des Befehls oder die zurückgegebene Fehlermeldung.

## <a name="commands"></a>Befehle

|Get-Help|BESCHREIBUNG|
| --- | --- |
| set-telemetry | Aktiviert oder deaktiviert die Telemetrie. |

## <a name="sfctl-settings-telemetry-set-telemetry"></a>sfctl settings telemetry set-telemetry
Aktiviert oder deaktiviert die Telemetrie.

### <a name="arguments"></a>Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --off | Deaktiviert die Telemetrie. |
| --on | Aktiviert die Telemetrie. Dies ist der Standardwert. |

### <a name="global-arguments"></a>Globale Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --debug | Ausführlichkeit der Protokollierung erhöhen, um alle Debugprotokolle anzuzeigen. |
| --help -h | Zeigen Sie diese Hilfemeldung an, und schließen Sie sie. |
| --output -o | Ausgabeformat.  Zulässige Werte\: „json“, „jsonc“, „table“, „tsv“.  Standardwert\: „json“. |
| --query | JMESPath-Abfragezeichenfolge. Weitere Informationen und Beispiele finden Sie unter „http\://jmespath.org/“. |
| --verbose | Ausführlichkeit der Protokollierung erhöhen. „--debug“ für vollständige Debugprotokolle verwenden. |

### <a name="examples"></a>Beispiele

Deaktiviert die Telemetrie.

```
sfctl settings telemetry set_telemetry --off
```

Aktiviert die Telemetrie.

```
sfctl settings telemetry set_telemetry --on
```


## <a name="next-steps"></a>Nächste Schritte
- [Einrichten](service-fabric-cli.md) der Service Fabric-Befehlszeilenschnittstelle
- Informationen zum Verwenden der Service Fabric-Befehlszeilenschnittstelle mit den [Beispielskripts](/azure/service-fabric/scripts/sfctl-upgrade-application)