---
title: Azure Service Fabric-CLI – sfctl mesh secret
description: Erfahren Sie mehr über sfctl, die Azure Service Fabric-Befehlszeilenschnittstelle. Enthält eine Liste der Befehle zum Abrufen und Löschen von Service Fabric Mesh-Geheimnisressourcen.
author: jeffj6123
ms.topic: reference
ms.date: 1/16/2020
ms.author: jejarry
ms.openlocfilehash: fab388ff223eb95020e2ba0945c76532bc54f224
ms.sourcegitcommit: 67e9f4cc16f2cc6d8de99239b56cb87f3e9bff41
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/31/2020
ms.locfileid: "76905984"
---
# <a name="sfctl-mesh-secret"></a>sfctl mesh secret
Ruft die Mesh-Geheimnisressourcen ab bzw. löscht diese.

## <a name="commands"></a>Befehle

|Get-Help|Beschreibung|
| --- | --- |
| delete | Löscht die Geheimnisressource. |
| list | Listet alle Geheimnisressourcen auf. |
| show | Ruft die Geheimnisressource mit dem angegebenen Namen ab. |

## <a name="sfctl-mesh-secret-delete"></a>sfctl mesh secret delete
Löscht die Geheimnisressource.

Löscht die angegebene Geheimnisressource und alle ihre benannten Werte.

### <a name="arguments"></a>Argumente

|Argument|Beschreibung|
| --- | --- |
| --name -n [Erforderlich] | Der Name der Geheimnisressource. |

### <a name="global-arguments"></a>Globale Argumente

|Argument|Beschreibung|
| --- | --- |
| --debug | Ausführlichkeit der Protokollierung erhöhen, um alle Debugprotokolle anzuzeigen. |
| --help -h | Zeigen Sie diese Hilfemeldung an, und schließen Sie sie. |
| --output -o | Ausgabeformat.  Zulässige Werte\: „json“, „jsonc“, „table“, „tsv“.  Standardwert\: „json“. |
| --query | JMESPath-Abfragezeichenfolge. Weitere Informationen und Beispiele finden Sie unter „http\://jmespath.org/“. |
| --verbose | Ausführlichkeit der Protokollierung erhöhen. „--debug“ für vollständige Debugprotokolle verwenden. |

## <a name="sfctl-mesh-secret-list"></a>sfctl mesh secret list
Listet alle Geheimnisressourcen auf.

Ruft die Informationen zu allen Geheimnisressourcen in einer bestimmten Ressourcengruppe ab. Die Informationen umfassen die Beschreibung und andere Eigenschaften des Geheimnisses.

### <a name="global-arguments"></a>Globale Argumente

|Argument|Beschreibung|
| --- | --- |
| --debug | Ausführlichkeit der Protokollierung erhöhen, um alle Debugprotokolle anzuzeigen. |
| --help -h | Zeigen Sie diese Hilfemeldung an, und schließen Sie sie. |
| --output -o | Ausgabeformat.  Zulässige Werte\: „json“, „jsonc“, „table“, „tsv“.  Standardwert\: „json“. |
| --query | JMESPath-Abfragezeichenfolge. Weitere Informationen und Beispiele finden Sie unter „http\://jmespath.org/“. |
| --verbose | Ausführlichkeit der Protokollierung erhöhen. „--debug“ für vollständige Debugprotokolle verwenden. |

## <a name="sfctl-mesh-secret-show"></a>sfctl mesh secret show
Ruft die Geheimnisressource mit dem angegebenen Namen ab.

Ruft die Informationen zur Geheimnisressource mit dem angegebenen Namen ab. Die Informationen umfassen die Beschreibung und andere Eigenschaften des Geheimnisses.

### <a name="arguments"></a>Argumente

|Argument|Beschreibung|
| --- | --- |
| --name -n [Erforderlich] | Der Name der Geheimnisressource. |

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