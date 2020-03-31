---
title: Azure Service Fabric-CLI – sfctl mesh deployment | Microsoft-Dokumentation
description: Erfahren Sie mehr über sfctl, die Azure Service Fabric-Befehlszeilenschnittstelle. Enthält eine Liste der Befehle zum Erstellen von Service Fabric Mesh-Ressourcen.
author: jeffj6123
ms.topic: reference
ms.date: 1/16/2020
ms.author: jejarry
ms.openlocfilehash: 108389407221779ed20e81310f084b7b5c23b8c7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "76906029"
---
# <a name="sfctl-mesh-deployment"></a>sfctl mesh deployment
Erstellt Service Fabric Mesh-Ressourcen.

## <a name="commands"></a>Befehle

|Get-Help|BESCHREIBUNG|
| --- | --- |
| create | Erstellt eine Bereitstellung von Service Fabric-Mesh-Ressourcen. |

## <a name="sfctl-mesh-deployment-create"></a>sfctl mesh deployment create
Erstellt eine Bereitstellung von Service Fabric-Mesh-Ressourcen.

### <a name="arguments"></a>Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --input-yaml-files [erforderlich] | Durch Trennzeichen getrennte relative oder absolute Dateipfade aller YAML-Dateien oder relativer oder absoluter Pfad des Verzeichnisses (rekursiv), das YAML-Dateien enthält. |
| --parameters | Ein relativer oder absoluter Pfad einer YAML-Datei oder einem JSON-Objekt, die bzw. das die Parameter enthält, die außer Kraft gesetzt werden müssen. |

### <a name="global-arguments"></a>Globale Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --debug | Ausführlichkeit der Protokollierung erhöhen, um alle Debugprotokolle anzuzeigen. |
| --help -h | Zeigen Sie diese Hilfemeldung an, und schließen Sie sie. |
| --output -o | Ausgabeformat.  Zulässige Werte\: „json“, „jsonc“, „table“, „tsv“.  Standardwert\: „json“. |
| --query | JMESPath-Abfragezeichenfolge. Weitere Informationen und Beispiele finden Sie unter „http\://jmespath.org/“. |
| --verbose | Ausführlichkeit der Protokollierung erhöhen. „--debug“ für vollständige Debugprotokolle verwenden. |

### <a name="examples"></a>Beispiele

Konsolidiert und stellt alle Ressourcen für den Cluster bereit, indem die Parameter in der YAML-Datei außer Kraft gesetzt werden.
``` 
sfctl mesh deployment create --input-yaml-files ./app.yaml,./network.yaml --parameters  
./param.yaml    
```

Konsolidiert und stellt alle Ressourcen in einem Verzeichnis für den Cluster bereit, indem die Parameter in der YAML-Datei außer Kraft gesetzt werden.

``` 
sfctl mesh deployment create --input-yaml-files ./resources --parameters ./param.yaml
```

Konsolidiert und stellt alle Ressourcen in einem Verzeichnis für den Cluster bereit, indem die Parameter außer Kraft gesetzt werden, die direkt als JSON-Objekt übergeben werden.
``` 
sfctl mesh deployment create --input-yaml-files ./resources --parameters "{ 'my_param' :    
{'value' : 'my_value'} }"   
```

## <a name="next-steps"></a>Nächste Schritte
- [Einrichten](service-fabric-cli.md) der Service Fabric-Befehlszeilenschnittstelle
- Informationen zum Verwenden der Service Fabric-Befehlszeilenschnittstelle mit den [Beispielskripts](/azure/service-fabric/scripts/sfctl-upgrade-application)
