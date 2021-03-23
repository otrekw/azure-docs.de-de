---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 03/10/2021
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: 3859b73e61a20f5950531d1680a3015fcb53d3a2
ms.sourcegitcommit: d135e9a267fe26fbb5be98d2b5fd4327d355fe97
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2021
ms.locfileid: "102613750"
---
|Name |BESCHREIBUNG |Richtlinien |Version |
|---|---|---|---|
|[\[Vorschau:\] Bereitstellen: Erforderliche Komponenten zum Aktivieren von Azure Monitor- und Azure Security-Agents für VMs konfigurieren](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policySetDefinitions/Monitoring/AzureMonitoring_Prerequisites.json) |Konfigurieren Sie Computer so, dass der Azure Monitor- und der Azure Security-Agent automatisch installiert werden. Security Center sammelt Ereignisse von den Agents und verwendet diese, um Sicherheitswarnungen und maßgeschneiderte Härtungsempfehlungen bereitzustellen. Erstellen Sie eine Ressourcengruppe und einen Log Analytics-Arbeitsbereich in derselben Region, in der sich auch der Computer zum Speichern der Überwachungsdatensätze befindet. Diese Richtlinie gilt nur für virtuelle Computer in einigen Regionen. |5 |1.0.0-preview |
|[Aktivieren von Azure Monitor für VM-Skalierungsgruppen](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policySetDefinitions/Monitoring/AzureMonitor_VMSS.json) |Hiermit aktivieren Sie Azure Monitor für die VM-Skalierungsgruppen im angegebenen Bereich (Verwaltungsgruppe, Abonnement oder Ressourcengruppe). Akzeptiert den Log Analytics-Arbeitsbereich als Parameter. Hinweis: Wenn „upgradePolicy“ für Ihre Skalierungsgruppe auf „Manuell“ festgelegt ist, müssen Sie die Erweiterung auf alle VMs in der Gruppe anwenden, indem Sie dafür ein Upgrade aufrufen. Führen Sie dazu in der CLI „az vmss update-instances“ aus. |6 |1.0.1 |
|[Azure Monitor für VMs aktivieren](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policySetDefinitions/Monitoring/AzureMonitor_VM.json) |Hiermit aktivieren Sie Azure Monitor für die virtuellen Computer (VMs) in dem angegebenen Bereich (Verwaltungsgruppe, Abonnement oder Ressourcengruppe). Akzeptiert den Log Analytics-Arbeitsbereich als Parameter. |10 |2.0.0 |
