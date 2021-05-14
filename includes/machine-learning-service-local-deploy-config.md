---
author: Blackmist
ms.service: machine-learning
ms.topic: include
ms.date: 04/21/2021
ms.author: larryfr
ms.openlocfilehash: a7a6d9eb7e552ee5dededc58516927d0d8e0eef6
ms.sourcegitcommit: ad921e1cde8fb973f39c31d0b3f7f3c77495600f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/25/2021
ms.locfileid: "107952156"
---
Die Einträge im Dokument `deploymentconfig.json` werden den Parametern für [LocalWebservice.deploy_configuration](/python/api/azureml-core/azureml.core.webservice.local.localwebservicedeploymentconfiguration) zugeordnet. In der folgenden Tabelle wird die Zuordnung zwischen den Entitäten im JSON-Dokument und den Parametern für die Methode beschrieben:

| JSON-Entität | Methodenparameter | Beschreibung |
| ----- | ----- | ----- |
| `computeType` | Nicht verfügbar | Das Computeziel. Für lokale Ziele ist der Wert `local` erforderlich. |
| `port` | `port` | Der lokale Port, auf dem der HTTP-Endpunkt des Diensts verfügbar gemacht werden soll. |

Dieser JSON-Code ist ein Beispiel für eine Bereitstellungskonfiguration für die Verwendung mit der CLI:

```json
{
    "computeType": "local",
    "port": 32267
}
```

Speichern Sie diese JSON-Datei unter dem Namen `deploymentconfig.json`.