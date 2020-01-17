---
title: Azure Service Fabric-CLI – sfctl mesh deployment | Microsoft-Dokumentation
description: Erfahren Sie mehr über sfctl, die Azure Service Fabric-Befehlszeilenschnittstelle. Enthält eine Liste der Befehle zum Erstellen von Service Fabric Mesh-Ressourcen.
author: jeffj6123
ms.topic: reference
ms.date: 9/17/2019
ms.author: jejarry
ms.openlocfilehash: 6512cce075906fc8708a39fa0747ec27be33e961
ms.sourcegitcommit: f788bc6bc524516f186386376ca6651ce80f334d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/03/2020
ms.locfileid: "75645360"
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
| --input-yaml-files [erforderlich] | Durch Trennzeichen getrennte relative/absolute Dateipfade aller YAML-Dateien oder relativer/absoluter Pfad des Verzeichnisses (rekursiv) mit YAML-Dateien. |
| --parameters | Ein relativer/absoluter Pfad zur YAML-Datei oder ein JSON-Objekt, das die Parameter enthält, die außer Kraft gesetzt werden müssen. |

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