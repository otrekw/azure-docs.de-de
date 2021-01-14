---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 01/08/2021
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: dd9743f98959f5590198b752c0c2c0037b3d2b28
ms.sourcegitcommit: 8dd8d2caeb38236f79fe5bfc6909cb1a8b609f4a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/08/2021
ms.locfileid: "98046402"
---
|Name |BESCHREIBUNG |Richtlinien |Version |
|---|---|---|---|
|[Computer mit unsicheren Einstellungen zur Kennwortsicherheit überwachen](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policySetDefinitions/Guest%20Configuration/GuestConfiguration_WindowsPasswordSettingsAINE.json) |Diese Initiative sorgt für die Bereitstellung der erforderlichen Richtlinienkomponenten und überwacht Computer mit unsicheren Einstellungen für die Kennwortsicherheit. Weitere Informationen zu Richtlinien für die Gastkonfiguration finden Sie unter [https://aka.ms/gcpol](https://aka.ms/gcpol). |9 |1.0.0 |
|[Voraussetzungen zum Aktivieren der Gastkonfigurationsrichtlinien auf VMs bereitstellen](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policySetDefinitions/Guest%20Configuration/GuestConfiguration_Prerequisites.json) |Mit dieser Initiative wird eine systemseitig zugewiesene verwaltete Identität hinzugefügt und die plattformgerechte Erweiterung für die Gastkonfiguration auf VMs bereitgestellt, die zur Überwachung durch Gastkonfigurationsrichtlinien berechtigt sind. Dies ist eine Voraussetzung für alle Gastkonfigurationsrichtlinien und erfordert eine Zuweisung zum Richtlinienzuweisungsbereich, bevor eine Gastkonfigurationsrichtlinie verwendet werden kann. Weitere Informationen zur Gastkonfiguration finden Sie unter [https://aka.ms/gcpol](https://aka.ms/gcpol). |4 |1.0.0-preview |
|[Windows-Computer müssen die Anforderungen der Azure-Sicherheitsbaseline erfüllen](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policySetDefinitions/Guest%20Configuration/GuestConfiguration_AzureBaseline.json) |Diese Initiative überwacht Windows-Computer mit Einstellungen, die nicht die Azure-Sicherheitsbaseline erfüllen. Ausführliche Informationen finden Sie unter [https://aka.ms/gcpol](https://aka.ms/gcpol). |29 |2.0.0-preview |
