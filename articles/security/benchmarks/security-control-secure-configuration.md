---
title: 'Azure-Sicherheitskontrolle: sichere Konfiguration'
description: 'Sicherheitskontrolle: sichere Konfiguration'
author: msmbaldwin
manager: rkarlin
ms.service: security
ms.topic: conceptual
ms.date: 12/30/2019
ms.author: mbaldwin
ms.custom: security-recommendations
ms.openlocfilehash: 03564effeee36ddb3316d48329ccab8ccfce75b9
ms.sourcegitcommit: 014e916305e0225512f040543366711e466a9495
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/14/2020
ms.locfileid: "75934291"
---
# <a name="security-control-secure-configuration"></a>Sicherheitskontrolle: Sichere Konfiguration

Erstellen, implementieren und verwalten Sie die Sicherheitskonfiguration von Azure-Ressourcen aktiv (nachverfolgen, melden und korrigieren), um zu verhindern, dass Angreifer anfällige Dienste und Einstellungen ausnutzen.

## <a name="71-establish-secure-configurations-for-all-azure-resources"></a>7.1: Einrichten sicherer Konfigurationen für alle Azure-Ressourcen

| Azure-ID | CIS-IDs | Verantwortlichkeit |
|--|--|--|
| 7.1 | 5,1 | Kunde |

Verwenden Sie Azure Policy oder Azure Security Center, um Sicherheitskonfigurationen für alle Azure-Ressourcen zu verwalten.

Konfigurieren und Verwalten von Azure Policy:

https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

## <a name="72-establish-secure-operating-system-configurations"></a>7.2: Einrichten sicherer Betriebssystemkonfigurationen

| Azure-ID | CIS-IDs | Verantwortlichkeit |
|--|--|--|
| 7.2 | 5,1 | Kunde |

Wenden Sie die Azure Security Center-Empfehlung &quot;Remediate Vulnerabilities in Security Configurations on your Virtual Machines&quot; (Beheben von Sicherheitsrisiken in Sicherheitskonfigurationen auf virtuellen Computern) an, um Sicherheitskonfigurationen für alle Computeressourcen zu verwalten.

Überwachen von Empfehlungen in Azure Security Center:

https://docs.microsoft.com/azure/security-center/security-center-recommendations

Umsetzen von Empfehlungen in Azure Security Center:

https://docs.microsoft.com/azure/security-center/security-center-remediate-recommendations

## <a name="73-maintain-secure-azure-resource-configurations"></a>7.3: Verwalten von sicheren Konfigurationen für Azure-Ressourcen

| Azure-ID | CIS-IDs | Verantwortlichkeit |
|--|--|--|
| 7.3 | 5,2 | Kunde |

Verwenden Sie die Azure-Richtlinien [Verweigern] und [Bereitstellen, falls nicht vorhanden], um sichere Einstellungen in den Azure-Ressourcen zu erzwingen.

Konfigurieren und Verwalten von Azure Policy:

https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

Grundlegendes zu Azure Policy-Auswirkungen:

https://docs.microsoft.com/azure/governance/policy/concepts/effects

## <a name="74-maintain-secure-operating-system-configurations"></a>7.4: Verwalten sicherer Betriebssystemkonfigurationen

| Azure-ID | CIS-IDs | Verantwortlichkeit |
|--|--|--|
| 7.4 | 5,2 | Shared |

Betriebssystem-Basisimages werden von Microsoft verwaltet und gewartet.

Sie können jedoch Sicherheitseinstellungen, die für Ihre Organisation erforderlich sind, mithilfe von Azure Resource Manager-Vorlagen und/oder Desired State Configuration anwenden.

Erstellen eines virtuellen Azure-Computers mithilfe einer Azure Resource Manager-Vorlage:

https://docs.microsoft.com/azure/virtual-machines/windows/ps-template

Grundlegendes zu Desired State Configuration für virtuelle Azure-Computer:

https://docs.microsoft.com/azure/virtual-machines/extensions/dsc-overview

## <a name="75-securely-store-configuration-of-azure-resources"></a>7.5: Sicheres Speichern der Konfiguration von Azure-Ressourcen

| Azure-ID | CIS-IDs | Verantwortlichkeit |
|--|--|--|
| 7,5 | 5.3 | Kunde |

Wenn Sie benutzerdefinierte Azure-Richtliniendefinitionen verwenden, nutzen Sie Azure DevOps oder Azure Repos, um Code sicher zu speichern und zu verwalten.

Speichern von Code in Azure DevOps:

https://docs.microsoft.com/azure/devops/repos/git/gitworkflow?view=azure-devops

Dokumentation zu Azure Repos:

https://docs.microsoft.com/azure/devops/repos/index?view=azure-devops

## <a name="76-securely-store-custom-operating-system-images"></a>7.6: Sicheres Speichern von benutzerdefinierten Betriebssystemimages

| Azure-ID | CIS-IDs | Verantwortlichkeit |
|--|--|--|
| 7.6 | 5.3 | Kunde |

Wenn Sie benutzerdefinierte Images verwenden, nutzen Sie RBAC, um sicherzustellen, dass nur autorisierte Benutzer auf die Images zugreifen können. Speichern Sie Containerimages in Azure Container Registry, und verwenden Sie RBAC, um sicherzustellen, dass nur autorisierte Benutzer auf die Images zugreifen können.

Grundlegendes zu RBAC in Azure:

https://docs.microsoft.com/azure/role-based-access-control/rbac-and-directory-admin-roles

Grundlegendes zu RBAC für Container Registry:

https://docs.microsoft.com/azure/container-registry/container-registry-roles

Konfigurieren von RBAC in Azure:

https://docs.microsoft.com/azure/role-based-access-control/quickstart-assign-role-user-portal

## <a name="77-deploy-system-configuration-management-tools"></a>7.7: Bereitstellen von Verwaltungstools zur Systemkonfiguration

| Azure-ID | CIS-IDs | Verantwortlichkeit |
|--|--|--|
| 7,7 | 5.4 | Kunde |

Verwenden Sie Azure Policy, um Systemkonfigurationen zu melden, zu überwachen und zu erzwingen. Entwickeln Sie außerdem einen Prozess und eine Pipeline zum Verwalten von Richtlinienausnahmen.

Konfigurieren und Verwalten von Azure Policy:

https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

## <a name="78-deploy-system-configuration-management-tools-for-operating-systems"></a>7.8: Bereitstellen von Verwaltungstools für Systemkonfigurationen für Betriebssysteme

| Azure-ID | CIS-IDs | Verantwortlichkeit |
|--|--|--|
| 7,8 | 5.4 | Kunde |

Verwenden Sie Azure Compute-Erweiterungen wie PowerShell Desired State Configuration für Windows Compute oder Linux Chef Extension für Linux.

Installieren von Erweiterungen für virtuelle Computer in Azure:

https://docs.microsoft.com/azure/virtual-machines/extensions/overview#how-can-i-install-an-extension

## <a name="79-implement-automated-configuration-monitoring-for-azure-services"></a>7.9: Implementieren der automatisierten Konfigurationsüberwachung für Azure-Dienste

| Azure-ID | CIS-IDs | Verantwortlichkeit |
|--|--|--|
| 7.9 | 5.5 | Kunde |

Verwenden Sie Azure Security Center, um Baselineüberprüfungen der Azure-Ressourcen durchzuführen.

Umsetzen von Empfehlungen in Azure Security Center:

https://docs.microsoft.com/azure/security-center/security-center-remediate-recommendations

## <a name="710-implement-automated-configuration-monitoring-for-operating-systems"></a>7.10: Implementieren der automatisierten Konfigurationsüberwachung für Betriebssysteme

| Azure-ID | CIS-IDs | Verantwortlichkeit |
|--|--|--|
| 7.1 | 5.5 | Kunde |

Verwenden Sie Azure Security Center, um Baselineüberprüfungen für Betriebssystem- und Docker-Einstellungen für Container durchzuführen.

Grundlegendes zu Azure Security Center-Containerempfehlungen:

https://docs.microsoft.com/azure/security-center/security-center-container-recommendations

## <a name="711-manage-azure-secrets-securely"></a>7.11: Sicheres Verwalten von Azure-Geheimnissen 

| Azure-ID | CIS-IDs | Verantwortlichkeit |
|--|--|--|
| 7.11 | Version 13.1 | Kunde |

Verwenden Sie die verwaltete Dienstidentität in Verbindung mit Azure Key Vault, um die Verwaltung von Geheimnissen für Cloudanwendungen zu vereinfachen und zu schützen.

Integration mit verwalteten Azure-Identitäten:

https://docs.microsoft.com/azure/azure-app-configuration/howto-integrate-azure-managed-service-identity

Erstellen einer Key Vault-Instanz:

https://docs.microsoft.com/azure/key-vault/quick-create-portal

Bereitstellen der Key Vault-Authentifizierung mit einer verwalteten Identität:

https://docs.microsoft.com/azure/key-vault/managed-identity

## <a name="712-manage-identities-securely-and-automatically"></a>7.12: Sicheres und automatisches Verwalten von Identitäten

| Azure-ID | CIS-IDs | Verantwortlichkeit |
|--|--|--|
| 7.12 | 4,1 | Kunde |

Verwenden Sie verwaltete Identitäten, um Azure-Dienste mit einer automatisch verwalteten Identität in Azure AD bereitzustellen. Mit verwalteten Identitäten können Sie die Authentifizierung bei jedem Dienst verwenden, der die Azure AD-Authentifizierung unterstützt, einschließlich Key Vault. Hierfür müssen keine Anmeldeinformationen im Code enthalten sein.

Konfigurieren von verwalteten Identitäten:

https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/qs-configure-portal-windows-vm

## <a name="713-eliminate-unintended-credential-exposure"></a>7.13: Beheben der unbeabsichtigten Offenlegung von Anmeldeinformationen

| Azure-ID | CIS-IDs | Verantwortlichkeit |
|--|--|--|
| 7.13 | 13.3 | Kunde |

Implementieren Sie Credential Scanner, um Anmeldeinformationen im Code zu identifizieren. In Credential Scanner wird auch das Verschieben von ermittelten Anmeldeinformationen an sicherere Speicherorte (z. B. Azure Key Vault) empfohlen. 

Einrichten von Credential Scanner:

https://secdevtools.azurewebsites.net/helpcredscan.html

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie in der nächsten Sicherheitskontrolle: [Schutz vor Schadsoftware](security-control-malware-defense.md)
