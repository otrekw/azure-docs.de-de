---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 04/14/2021
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: 88d459140be43306f5f75dd96a7800b45b28d74b
ms.sourcegitcommit: 3b5cb7fb84a427aee5b15fb96b89ec213a6536c2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/14/2021
ms.locfileid: "107504869"
---
|Name |BESCHREIBUNG |Richtlinien |Version |
|---|---|---|---|
|[Computer mit unsicheren Einstellungen zur Kennwortsicherheit überwachen](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policySetDefinitions/Guest%20Configuration/GuestConfiguration_WindowsPasswordSettingsAINE.json) |Diese Initiative sorgt für die Bereitstellung der erforderlichen Richtlinienkomponenten und überwacht Computer mit unsicheren Einstellungen für die Kennwortsicherheit. Weitere Informationen zu Richtlinien für die Gastkonfiguration finden Sie unter [https://aka.ms/gcpol](https://aka.ms/gcpol). |9 |1.0.0 |
|[\[Vorschau:\] Voraussetzungen zum Aktivieren der Gastkonfigurationsrichtlinien auf VMs bereitstellen](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policySetDefinitions/Guest%20Configuration/GuestConfiguration_Prerequisites.json) |Mit dieser Initiative wird eine systemseitig zugewiesene verwaltete Identität hinzugefügt und die plattformgerechte Erweiterung für die Gastkonfiguration auf VMs bereitgestellt, die zur Überwachung durch Gastkonfigurationsrichtlinien berechtigt sind. Dies ist eine Voraussetzung für alle Gastkonfigurationsrichtlinien und erfordert eine Zuweisung zum Richtlinienzuweisungsbereich, bevor eine Gastkonfigurationsrichtlinie verwendet werden kann. Weitere Informationen zur Gastkonfiguration finden Sie unter [https://aka.ms/gcpol](https://aka.ms/gcpol). |4 |1.0.0-preview |
|[\[Vorschau:\] Windows-Computer müssen die Anforderungen für die Azure-Sicherheitsbaseline erfüllen](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policySetDefinitions/Guest%20Configuration/GuestConfiguration_AzureBaseline.json) |Diese Initiative überwacht Windows-Computer mit Einstellungen, die nicht die Azure-Sicherheitsbaseline erfüllen. Ausführliche Informationen finden Sie unter [https://aka.ms/gcpol](https://aka.ms/gcpol). |29 |2.0.0-preview |
