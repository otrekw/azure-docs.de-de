---
title: Azure Service Fabric-CLI – sfctl mesh-App
description: Erfahren Sie mehr über sfctl, die Azure Service Fabric-Befehlszeilenschnittstelle. Enthält eine Liste der Befehle zum Verwalten von Service Fabric Mesh-Anwendungsressurcen.
author: jeffj6123
ms.topic: reference
ms.date: 1/16/2020
ms.author: jejarry
ms.openlocfilehash: 7c571136e6bef64c2024c81db85ef736f64a4e01
ms.sourcegitcommit: 67e9f4cc16f2cc6d8de99239b56cb87f3e9bff41
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/31/2020
ms.locfileid: "76906058"
---
# <a name="sfctl-mesh-app"></a>sfctl mesh app
Ruft Anwendungsressourcen ab bzw. löscht diese.

## <a name="commands"></a>Befehle

|Get-Help|Beschreibung|
| --- | --- |
| delete | Löscht die Anwendungsressource. |
| list | Listet alle Anwendungsressourcen auf. |
| show | Ruft die Anwendungsressource mit dem angegebenen Namen ab. |

## <a name="sfctl-mesh-app-delete"></a>sfctl mesh app delete
Löscht die Anwendungsressource.

Löscht die Anwendungsressource, die durch den Namen identifiziert wird.

### <a name="arguments"></a>Argumente

|Argument|Beschreibung|
| --- | --- |
| --name -n [Erforderlich] | Der Namen der Anwendung. |

### <a name="global-arguments"></a>Globale Argumente

|Argument|Beschreibung|
| --- | --- |
| --debug | Ausführlichkeit der Protokollierung erhöhen, um alle Debugprotokolle anzuzeigen. |
| --help -h | Zeigen Sie diese Hilfemeldung an, und schließen Sie sie. |
| --output -o | Ausgabeformat.  Zulässige Werte\: „json“, „jsonc“, „table“, „tsv“.  Standardwert\: „json“. |
| --query | JMESPath-Abfragezeichenfolge. Weitere Informationen und Beispiele finden Sie unter „http\://jmespath.org/“. |
| --verbose | Ausführlichkeit der Protokollierung erhöhen. „--debug“ für vollständige Debugprotokolle verwenden. |

## <a name="sfctl-mesh-app-list"></a>sfctl mesh app list
Listet alle Anwendungsressourcen auf.

Ruft die Informationen zu allen Anwendungsressourcen in einer bestimmten Ressourcengruppe ab. Die Informationen umfassen die Beschreibung und andere Eigenschaften der Anwendung.

### <a name="global-arguments"></a>Globale Argumente

|Argument|Beschreibung|
| --- | --- |
| --debug | Ausführlichkeit der Protokollierung erhöhen, um alle Debugprotokolle anzuzeigen. |
| --help -h | Zeigen Sie diese Hilfemeldung an, und schließen Sie sie. |
| --output -o | Ausgabeformat.  Zulässige Werte\: „json“, „jsonc“, „table“, „tsv“.  Standardwert\: „json“. |
| --query | JMESPath-Abfragezeichenfolge. Weitere Informationen und Beispiele finden Sie unter „http\://jmespath.org/“. |
| --verbose | Ausführlichkeit der Protokollierung erhöhen. „--debug“ für vollständige Debugprotokolle verwenden. |

## <a name="sfctl-mesh-app-show"></a>sfctl mesh app show
Ruft die Anwendungsressource mit dem angegebenen Namen ab.

Ruft die Informationen zur Anwendungsressource mit dem angegebenen Namen ab. Die Informationen umfassen die Beschreibung und andere Eigenschaften der Anwendung.

### <a name="arguments"></a>Argumente

|Argument|Beschreibung|
| --- | --- |
| --name -n [Erforderlich] | Der Namen der Anwendung. |

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