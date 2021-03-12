---
author: Blackmist
ms.service: machine-learning
ms.topic: include
ms.date: 03/16/2020
ms.author: larryfr
ms.openlocfilehash: 4983c1e1e7f235fa7a5b748a0ce5b1c79176c849
ms.sourcegitcommit: 15d27661c1c03bf84d3974a675c7bd11a0e086e6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2021
ms.locfileid: "102510679"
---
Die Einträge im Dokument `deploymentconfig.json` werden den Parametern für [AciWebservice.deploy_configuration](/python/api/azureml-core/azureml.core.webservice.aci.aciservicedeploymentconfiguration) zugeordnet. In der folgenden Tabelle wird die Zuordnung zwischen den Entitäten im JSON-Dokument und den Parametern für die Methode beschrieben:

| JSON-Entität | Methodenparameter | Beschreibung |
| ----- | ----- | ----- |
| `computeType` | Nicht verfügbar | Das Computeziel. Für ACI muss der Wert `ACI` sein. |
| `containerResourceRequirements` | Nicht verfügbar | Der Container für die CPU- und Arbeitsspeicherentitäten. |
| &emsp;&emsp;`cpu` | `cpu_cores` | Die Anzahl der zuzuordnenden CPU-Kerne. Standardwert: `0.1` |
| &emsp;&emsp;`memoryInGB` | `memory_gb` | Der Arbeitsspeicherumfang (in GB), der für diesen Webdienst zugeordnet werden soll. Standardwert: `0.5`. |
| `location` | `location` | Die Azure-Region, in der dieser Webdienst bereitgestellt werden soll. Wenn keine Region angegeben ist, wird der Standort des Arbeitsbereichs verwendet. Weitere Informationen zu verfügbaren Regionen finden Sie hier: [ACI-Regionen](https://azure.microsoft.com/global-infrastructure/services/?regions=all&products=container-instances). |
| `authEnabled` | `auth_enabled` | Gibt an, ob die Authentifizierung für diesen Webdienst aktiviert werden soll. Der Standardwert ist „FALSE“. |
| `sslEnabled` | `ssl_enabled` | Gibt an, ob für diesen Webdienst SSL aktiviert werden soll. Der Standardwert lautet „False“. |
| `appInsightsEnabled` | `enable_app_insights` | Gibt an, ob für diesen Webdienst AppInsights aktiviert werden soll. Der Standardwert ist „FALSE“. |
| `sslCertificate` | `ssl_cert_pem_file` | Die benötigte CERT-Datei, wenn SSL aktiviert ist |
| `sslKey` | `ssl_key_pem_file` | Die benötigte KEY-Datei, wenn SSL aktiviert ist |
| `cname` | `ssl_cname` | Der CNAME, wenn SSL aktiviert ist |
| `dnsNameLabel` | `dns_name_label` | Die DNS-Namensbezeichnung für den Bewertungsendpunkt. Wenn keine eindeutige DNS-Namensbezeichnung angegeben ist, wird eine für den Bewertungsendpunkt generiert. |

Der folgende JSON-Code ist ein Beispiel für eine Bereitstellungskonfiguration für die Verwendung mit der CLI:

```json
{
    "computeType": "aci",
    "containerResourceRequirements":
    {
        "cpu": 0.5,
        "memoryInGB": 1.0
    },
    "authEnabled": true,
    "sslEnabled": false,
    "appInsightsEnabled": false
}
```