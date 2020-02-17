---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 02/10/2020
ms.author: dacoulte
ms.openlocfilehash: 8fe3dca69974f1df09ffb9ca4e1ece5a614f61f5
ms.sourcegitcommit: bdf31d87bddd04382effbc36e0c465235d7a2947
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/12/2020
ms.locfileid: "77170408"
---
|Name |BESCHREIBUNG |Richtlinien |Version |
|---|---|---|---|
|[Azure Monitor für VM-Skalierungsgruppen (VMSS) aktivieren](https://github.com/Azure/azure-policy/blob/masterbuilt-in-policies/policySetDefinitions/Monitoring/AzureMonitor_VMSS.json) |Hiermit wird Azure Monitor für die VM-Skalierungsgruppen im angegebenen Bereich (Verwaltungsgruppe, Abonnement oder Ressourcengruppe) aktiviert. Akzeptiert den Log Analytics-Arbeitsbereich als Parameter. Hinweis: Wenn „upgradePolicy“ für Ihre Skalierungsgruppe auf „Manuell“ festgelegt ist, müssen Sie die Erweiterung auf alle VMs in der Gruppe anwenden, indem Sie dafür ein Upgrade aufrufen. Führen Sie dazu in der CLI „az vmss update-instances“ aus. |6 |1.0.0-preview |
|[Azure Monitor für VMs aktivieren](https://github.com/Azure/azure-policy/blob/masterbuilt-in-policies/policySetDefinitions/Monitoring/AzureMonitor_VM.json) |Hiermit aktivieren Sie Azure Monitor für die virtuellen Computer (VMs) in dem angegebenen Bereich (Verwaltungsgruppe, Abonnement oder Ressourcengruppe). Akzeptiert den Log Analytics-Arbeitsbereich als Parameter. |6 |1.0.0-preview |
