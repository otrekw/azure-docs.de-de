---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 02/10/2020
ms.author: dacoulte
ms.openlocfilehash: 667d017e8febcf1abcc1cfe21ecfaa43ae75ea6d
ms.sourcegitcommit: bdf31d87bddd04382effbc36e0c465235d7a2947
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/12/2020
ms.locfileid: "77170098"
---
|Name |BESCHREIBUNG |Auswirkungen |Version |
|---|---|---|---|
|[Zulässige SKUs für virtuelle Computer](https://github.com/Azure/azure-policy/blob/masterbuilt-in-policies/policyDefinitions/Compute/VMSkusAllowed_Deny.json) |Über diese Richtlinie können Sie einen Satz von SKUs für virtuelle Computer angeben, die Ihre Organisation bereitstellen kann. |Verweigern |1.0.0 |
|[VMs überwachen, für die keine Notfallwiederherstellung konfiguriert ist](https://github.com/Azure/azure-policy/blob/masterbuilt-in-policies/policyDefinitions/Compute/RecoveryServices_DisasterRecovery_Audit.json) |Hiermit werden VMs überwacht, für die keine Notfallwiederherstellung konfiguriert ist. Weitere Informationen zur Notfallwiederherstellung finden Sie unter https://aka.ms/asr-doc. |auditIfNotExists |1.0.0 |
|[Virtuelle Computer überwachen, die keine verwalteten Datenträger verwenden](https://github.com/Azure/azure-policy/blob/masterbuilt-in-policies/policyDefinitions/Compute/VMRequireManagedDisk_Audit.json) |Diese Richtlinie überwacht virtuelle Computer, die keine verwalteten Datenträger verwenden. |Überwachung |1.0.0 |
|[Microsoft IaaSAntimalware-Standarderweiterung für Windows Server bereitstellen](https://github.com/Azure/azure-policy/blob/masterbuilt-in-policies/policyDefinitions/Compute/VMAntimalwareExtension_Deploy.json) |Mit dieser Richtlinie wird eine Microsoft IaaSAntimalware-Erweiterung mit einer Standardkonfiguration bereitgestellt, wenn eine VM nicht mit der Antischadsoftware-Erweiterung konfiguriert ist. |deployIfNotExists |1.0.0 |
|[In VM-Skalierungsgruppen sollten Diagnoseprotokolle aktiviert sein](https://github.com/Azure/azure-policy/blob/masterbuilt-in-policies/policyDefinitions/Compute/ServiceFabric_and_VMSS_AuditVMSSDiagnostics.json) |Es wird empfohlen, die Protokollierung zu aktivieren. So können Sie Aktivitäten nachvollziehen, wenn bei einem Incident oder einer Beeinträchtigung Untersuchungen durchgeführt werden müssen. |AuditIfNotExists, Disabled |1.0.0 |
|[Microsoft Antimalware für Azure muss für die automatische Aktualisierung von Schutzsignaturen konfiguriert sein](https://github.com/Azure/azure-policy/blob/masterbuilt-in-policies/policyDefinitions/Compute/VirtualMachines_AntiMalwareAutoUpdate_AuditIfNotExists.json) |Diese Richtlinie überwacht alle virtuellen Windows-Computer, die nicht für die automatische Aktualisierung von Microsoft Antimalware-Schutzsignaturen konfiguriert sind. |AuditIfNotExists, Disabled |1.0.0 |
|[Die Microsoft IaaSAntimalware-Erweiterung muss auf Windows-Servern bereitgestellt werden](https://github.com/Azure/azure-policy/blob/masterbuilt-in-policies/policyDefinitions/Compute/WindowsServers_AntiMalware_AuditIfNotExists.json) |Diese Richtlinie überwacht alle Windows Server-VMs ohne bereitgestellte Microsoft IaaSAntimalware-Erweiterung. |AuditIfNotExists, Disabled |1.0.0 |
|[Es dürfen nur genehmigte VM-Erweiterungen installiert werden](https://github.com/Azure/azure-policy/blob/masterbuilt-in-policies/policyDefinitions/Compute/VirtualMachines_ApprovedExtensions_Audit.json) |Diese Richtlinie regelt die nicht genehmigten VM-Erweiterungen. |Audit, Deny, Disabled |1.0.0 |
|[Automatische Patches für Betriebssystemimages in VM-Skalierungsgruppen erzwingen](https://github.com/Azure/azure-policy/blob/masterbuilt-in-policies/policyDefinitions/Compute/VMSSOSUpgradeHealthCheck_Deny.json) |Diese Richtlinie erzwingt die Aktivierung automatischer Patches für Betriebssystemimages in VM-Skalierungsgruppen zum ständigen Schutz virtueller Computer durch sicheres monatliches Anwenden der neuesten Sicherheitspatches. |deny |1.0.0 |
|[Nicht angefügte Datenträger müssen verschlüsselt werden](https://github.com/Azure/azure-policy/blob/masterbuilt-in-policies/policyDefinitions/Compute/UnattachedDisk_Encryption_Audit.json) |Diese Richtlinie überwacht nicht angefügte Datenträger ohne aktivierte Verschlüsselung. |Audit, Disabled |1.0.0 |
|[VMs sollten zu neuen Azure Resource Manager-Ressourcen migriert werden](https://github.com/Azure/azure-policy/blob/masterbuilt-in-policies/policyDefinitions/Compute/ClassicCompute_Audit.json) |Verwenden Sie den neuen Azure Resource Manager für Ihre VMs, um von den folgenden Sicherheitsverbesserungen zu profitieren: strengere Zugriffssteuerung (RBAC), bessere Überwachung, ARM-basierte Bereitstellung und Governance, Zugriff auf verwaltete Identitäten, Zugriff auf Schlüsseltresore für Geheimnisse, Azure AD-basierte Authentifizierung und Unterstützung für Tags und Ressourcengruppen für eine einfachere Sicherheitsverwaltung. |Audit, Deny, Disabled |1.0.0 |
