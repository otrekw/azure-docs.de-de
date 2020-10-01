---
title: 'Azure-Sicherheitsvergleichstest V2: Privilegierter Zugriff'
description: 'Azure-Sicherheitsvergleichstest V2: Privilegierter Zugriff'
author: msmbaldwin
ms.service: security
ms.topic: conceptual
ms.date: 09/13/2020
ms.author: mbaldwin
ms.custom: security-benchmark
ms.openlocfilehash: 8727cbd07fad1960f4bdb33742729b6bda3a369e
ms.sourcegitcommit: 94c750edd4d755d6ecee50ac977328098a277479
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/13/2020
ms.locfileid: "90059076"
---
# <a name="security-control-privileged-access"></a>Sicherheitskontrolle: Privilegierter Zugriff

Privilegierter Zugriff gilt für Kontrollelemente, um den privilegierten Zugriff auf Ihren Azure-Mandanten und Ihre Azure-Ressourcen zu schützen. Dies umfasst eine Reihe von Kontrollelementen, um Ihr Verwaltungsmodell, Ihre Verwaltungskonten und Workstations mit privilegiertem Zugriff vor absichtlichen und unbeabsichtigten Risiken zu schützen.

## <a name="pa-1-protect-and-limit-the-global-administrators"></a>PA-1: Schützen und Beschränken globaler Administratoren

| Azure-ID | CIS Controls v7.1 ID(s) | NIST SP800-53 r4 ID(s) |
|--|--|--|--|
| PA-1 | 4.3, 4.8 | AC-2 |

Benutzer, denen die Rolle „Globaler Administrator“ zugewiesen ist, können alle administrativen Einstellungen in Ihrer Azure AD-Organisation lesen und ändern. Beschränken Sie die Anzahl Ihrer globalen Azure-Administratorkonten auf maximal zwei pro Abonnement. Die wichtigsten integrierten Rollen in Azure AD sind „Globaler Administrator“ und „Administrator für privilegierte Rollen“, da Benutzer, die diesen beiden Rollen zugeordnet sind, Administratorrollen delegieren können:
- Globaler Administrator/Unternehmensadministrator: Benutzer mit dieser Rolle haben Zugriff auf alle administrativen Funktionen in Azure AD sowie auf Dienste, die Azure AD-Identitäten verwenden.

- Administrator für privilegierte Rollen: Benutzer mit dieser Rolle können Rollenzuweisungen in Azure AD und Azure AD Privileged Identity Management (PIM) verwalten. Überdies ermöglicht diese Rolle Verwaltung aller Aspekte von PIM und administrativer Einheiten.

Hinweis: Möglicherweise verfügen Sie über weitere kritische Rollen, die geregelt werden müssen, wenn Sie benutzerdefinierte Rollen mit bestimmten zugewiesenen privilegierten Berechtigungen verwenden. Möglicherweise möchten Sie ähnliche Kontrollelemente auch auf das Administratorkonto für kritische Unternehmensressourcen anwenden.  

Mit Azure AD Privileged Identity Management (PIM) können Sie privilegierten Just-in-Time-Zugriff (JIT) auf Azure-Ressourcen und Azure AD ermöglichen. JIT erteilt nur dann temporäre Berechtigungen zur Ausführung privilegierter Aufgaben, wenn die Benutzer sie benötigen. PIM kann auch Sicherheitswarnungen erzeugen, wenn es in Ihrer Azure AD-Organisation verdächtige oder unsichere Aktivitäten gibt.

Hinweis: Einige Azure-Dienste wie Azure SQL unterstützen neben der Azure AD-Authentifizierung auch die lokale Benutzerauthentifizierung. Diese Art der lokalen Benutzerauthentifizierung wird nicht über Azure AD verwaltet. Sie müssen diese Benutzer separat verwalten.

- [Administratorrollenberechtigungen in Azure AD](../../active-directory/users-groups-roles/directory-assign-admin-roles.md)

- [Benutzerdefinierte Azure-Rollen](../../role-based-access-control/custom-roles.md)

- [Verwenden von Sicherheitswarnungen von Azure Privileged Identity Management](../../active-directory/privileged-identity-management/pim-how-to-configure-security-alerts.md)

- [Abrufen einer Verzeichnisrolle in Azure AD mit PowerShell](https://docs.microsoft.com/powershell/module/azuread/get-azureaddirectoryrole?view=azureadps-2.0)

- [Verwenden von Azure Security Center zum Überwachen von Identität und Zugriff](../../security-center/security-center-identity-access.md)

**Verantwortlichkeit**: Kunde

**Sicherheitsbeteiligte der Kunden**:

- [Identität und Schlüssel](/azure/cloud-adoption-framework/organize/cloud-security-identity-keys)

- [Anwendungssicherheit und DevSecOps](/azure/cloud-adoption-framework/organize/cloud-security-application-security-devsecops)

## <a name="pa-2-review-and-reconcile-user-access-regularly"></a>PA-2: Regelmäßiges Überprüfen und Abstimmen des Benutzerzugriffs

| Azure-ID | CIS Controls v7.1 ID(s) | NIST SP800-53 r4 ID(s) |
|--|--|--|--|
| PA-2 | 4.1, 16.9, 16.10 | AC-2 |

Überprüfen Sie Benutzerkonten und Zugriffsberechtigungen regelmäßig, um sicherzustellen, dass die Benutzerkonten und ihre Zugriffsrechte gültig sind. 

Verwenden Sie Azure AD-Identitäts- und -Zugriffsüberprüfungen, um Gruppenmitgliedschaften, den Zugriff auf Unternehmensanwendungen sowie Rollenzuweisungen zu verwalten. Azure AD-Berichte können Protokolle zum Ermitteln von veralteten Konten bereitstellen. Sie können Azure AD Privileged Identity Management auch verwenden, um einen Workflow für den Zugriffsüberprüfungsbericht zu erstellen und so den Überprüfungsprozess zu vereinfachen.

Für administrative Benutzer auf der Ebene des Azure-Diensts und des Azure-Workloads sollte häufigere Benutzer- und des Zugriffsüberprüfungen durchgeführt werden. Sie können auch die Sicherheitswarnungen von Azure Privileged Identity Management verwenden, um zu erkennen, wenn Administratorkonten erstellt werden, und um veraltete oder falsch konfigurierte Administratorkonten zu ermitteln. 

Hinweis: Einige Azure-Dienste wie Azure SQL unterstützen lokale Benutzer, die nicht über Azure AD verwaltet werden. Sie müssen diese Benutzer separat verwalten.

- [Abrufen einer Verzeichnisrolle in Azure AD mit PowerShell](https://docs.microsoft.com/powershell/module/azuread/get-azureaddirectoryrole?view=azureadps-2.0)

- [Abrufen von Mitgliedern einer Verzeichnisrolle in Azure AD mit PowerShell](https://docs.microsoft.com/powershell/module/azuread/get-azureaddirectoryrolemember?view=azureadps-2.0)

- [Verwenden von Azure Security Center zum Überwachen von Identität und Zugriff](../../security-center/security-center-identity-access.md)

- [Verwenden von Sicherheitswarnungen von Azure Privileged Identity Management](../../active-directory/privileged-identity-management/pim-how-to-configure-security-alerts.md)

- [Zuordnen von administrativen Zuständigkeiten für Teams](/azure/cloud-adoption-framework/organize/raci-alignment) 

- [Grundlegendes zur Azure AD-Berichterstellung](/azure/active-directory/reports-monitoring/)

- [Was sind Azure AD-Zugriffsüberprüfungen?](../../active-directory/governance/access-reviews-overview.md)

- [Privileged Identity Management: Überprüfen des Zugriffs auf Azure AD-Rollen](../../active-directory/privileged-identity-management/pim-how-to-start-security-review.md)

- [Azure Security Center: Überwachen von Identität und Zugriff](../../security-center/security-center-identity-access.md)

**Verantwortlichkeit**: Kunde

**Sicherheitsbeteiligte der Kunden**:

- [Identität und Schlüssel](/azure/cloud-adoption-framework/organize/cloud-security-identity-keys)

- [Anwendungssicherheit und DevSecOps](/azure/cloud-adoption-framework/organize/cloud-security-application-security-devsecops)

- [Sicherheitscomplianceverwaltung](/azure/cloud-adoption-framework/organize/cloud-security-compliance-management)

## <a name="pa-3-set-up-an-emergency-access-account-in-azure-ad"></a>PA-3: Einrichten eines Kontos für den Notfallzugriff in Azure AD

| Azure-ID | CIS Controls v7.1 ID(s) | NIST SP800-53 r4 ID(s) |
|--|--|--|--|
| PA-3 | 12,3 | AC-2 |

Um zu verhindern, dass Sie versehentlich aus Ihrer Azure AD-Organisation ausgesperrt werden, richten Sie ein Konto für den Notfallzugriff ein, für den Fall, dass normale Verwaltungskonten nicht verwendet werden können. Konten für den Notfallzugriff verfügen normalerweise über umfangreiche Berechtigungen und sollten keinen Einzelpersonen zugewiesen werden. Konten für den Notfallzugriff sind auf Notfallsituationen oder Szenarien beschränkt, in denen normale Administratorkonten nicht verwendet werden können.

Sie sollten sicherstellen, dass die Anmeldeinformationen (z. B. Kennwort, Zertifikat oder Smartcard) für Konten für den Notfallzugriff sicher aufbewahrt werden und nur den Personen bekannt sind, die für deren Verwendung im Notfall autorisiert sind.

- [Verwalten von Konten für den Notfallzugriff in Azure AD](../../active-directory/users-groups-roles/directory-emergency-access.md)

**Verantwortlichkeit**: Kunde

**Sicherheitsbeteiligte der Kunden**:

- [Identität und Schlüssel](/azure/cloud-adoption-framework/organize/cloud-security-identity-keys)

- [Anwendungssicherheit und DevSecOps](/azure/cloud-adoption-framework/organize/cloud-security-application-security-devsecops)

- [Sicherheitscomplianceverwaltung](/azure/cloud-adoption-framework/organize/cloud-security-compliance-management)

- [Security Operations Center (SOC)](/azure/cloud-adoption-framework/organize/cloud-security-operations-center)

## <a name="pa-4-automate-azure-identity-and-access-request-workflow"></a>PA-4: Automatisieren des Workflows für Azure-Identitäts- und Zugriffsanforderungen

| Azure-ID | CIS Controls v7.1 ID(s) | NIST SP800-53 r4 ID(s) |
|--|--|--|--|
| PA-4 | – | AC-2, AC-5, PM-10 |

Verwenden Sie die Berechtigungsverwaltungsfeatures von Azure AD, um die Azure-Zugriffsanforderungsworkflows zu automatisieren, einschließlich Zugriffszuweisungen, Überprüfungen und Ablauf. Die duale oder die mehrstufige Genehmigung wird ebenfalls unterstützt.  

- [Was ist die Azure AD-Berechtigungsverwaltung?](../../active-directory/governance/entitlement-management-overview.md)

- [Einrichten des Zugriffsanforderungs- und -genehmigungsprozesses](../../active-directory/governance/entitlement-management-access-package-request-policy.md)

**Verantwortlichkeit**: Kunde

**Sicherheitsbeteiligte der Kunden**:

- [Identität und Schlüssel](/azure/cloud-adoption-framework/organize/cloud-security-identity-keys)

- [Anwendungssicherheit und DevSecOps](/azure/cloud-adoption-framework/organize/cloud-security-application-security-devsecops)

- [Sicherheitscomplianceverwaltung](/azure/cloud-adoption-framework/organize/cloud-security-compliance-management)

## <a name="pa-5-use-highly-secured-machines-for-administrative-tasks"></a>PA-5: Verwenden stark gesicherter Computer für alle administrativen Aufgaben

| Azure-ID | CIS Controls v7.1 ID(s) | NIST SP800-53 r4 ID(s) |
|--|--|--|--|
| PA-5 | 4.6, 11.6, 12.12 | AC-2, SC-7 |

Gesicherte, isolierte Arbeitsstationen sind von entscheidender Bedeutung für die Sicherheit sensibler Rollen wie Administratoren, Entwickler und Operatoren kritischer Dienste. Verwenden Sie stark gesicherte Benutzerworkstations und/oder Azure Bastion für administrative Aufgaben:
- Verwenden Sie Azure Active Directory, Microsoft Defender Advanced Threat Protection (ATP) und/oder Microsoft Intune, um eine sichere und verwaltete Benutzerworkstation für administrative Aufgaben einzurichten. Die gesicherten Workstations können zentral verwaltet werden, um eine gesicherte Konfiguration einschließlich starker Authentifizierung, Software- und Hardwarebaselines, eingeschränktem logischen und Netzwerkzugang durchzusetzen. 

- Verwenden Sie das Azure Bastion-Feature für einen sicheren Pfad für den Zugriff auf Ihre virtuellen Computer über RDP oder SSH. Azure Bastion ist ein vollständig verwalteter PaaS-Dienst, der pro virtuellem Netzwerk bereitgestellt werden kann, ohne ein neues virtuelles Netzwerk zu erstellen. 

- [Informationen zu sicheren, von Azure verwalteten Arbeitsstationen](../../active-directory/devices/concept-azure-managed-workstation.md)

- [Bereitstellen einer sicheren, von Azure verwalteten Arbeitsstation](../../active-directory/devices/howto-azure-managed-workstation.md)

- [Verwenden des Azure Bastion-Hosts](../../bastion/bastion-create-host-portal.md)

**Verantwortlichkeit**: Kunde

**Sicherheitsbeteiligte der Kunden**:

- [Anwendungssicherheit und DevSecOps](/azure/cloud-adoption-framework/organize/cloud-security-application-security-devsecops)

- [Security Operations Center (SOC)](/azure/cloud-adoption-framework/organize/cloud-security-operations-center)

- [Identität und Schlüssel](/azure/cloud-adoption-framework/organize/cloud-security-identity-keys)

## <a name="pa-6-assign-privileges-to-resources-using-azure-rbac"></a>PA-6: Zuweisen von Berechtigungen zu Ressourcen mithilfe von Azure RBAC

| Azure-ID | CIS Controls v7.1 ID(s) | NIST SP800-53 r4 ID(s) |
|--|--|--|--|
| PA-6 | 14.6 | AC-2, AC-3 |

Mit der rollenbasierten Zugriffssteuerung in Azure (Role-Based Access Control, RBAC) können die Berechtigungen für den Zugriff auf Azure-Ressourcen über Rollenzuweisungen verwaltet werden. Diese Rollen können Benutzern, Gruppen, Dienstprinzipalen und verwalteten Identitäten zugewiesen werden. Für bestimmte Ressourcen sind vordefinierte integrierte Rollen verfügbar. Diese Rollen können über Tools wie die Azure CLI, Azure PowerShell oder das Azure-Portal inventarisiert oder abgefragt werden. 

- [Was ist die rollenbasierte Zugriffssteuerung in Azure (Azure Role-Based Access Control, Azure RBAC)?](../../role-based-access-control/overview.md)

- [Konfigurieren von RBAC in Azure](../../role-based-access-control/role-assignments-portal.md)

**Verantwortlichkeit**: Kunde

**Sicherheitsbeteiligte der Kunden**:

- [Anwendungssicherheit und DevSecOps](/azure/cloud-adoption-framework/organize/cloud-security-application-security-devsecops)

- [Sicherheitscomplianceverwaltung](/azure/cloud-adoption-framework/organize/cloud-security-compliance-management)

- [Statusverwaltung](/azure/cloud-adoption-framework/organize/cloud-security-posture-management)

- [Identität und Schlüssel](/azure/cloud-adoption-framework/organize/cloud-security-identity-keys)

## <a name="pa-7-choose-approval-process-for-microsoft-support"></a>PA-7: Auswählen des Genehmigungsprozesses für Microsoft-Support

| Azure-ID | CIS Controls v7.1 ID(s) | NIST SP800-53 r4 ID(s) |
|--|--|--|--|
| PA-7 | 16 | AC-2, AC-3, AC-4 |

In Supportszenarien, bei denen Microsoft auf Kundendaten zugreifen muss, bietet Kunden-Lockbox Ihnen die Möglichkeit, einzelne Anforderungen nach Zugriff auf Kundendaten explizit zu prüfen und dann zu genehmigen oder abzulehnen.

- [Grundlegendes zu Kunden-Lockbox](../fundamentals/customer-lockbox-overview.md)

**Verantwortlichkeit**: Kunde

**Sicherheitsbeteiligte der Kunden**:

- [Anwendungssicherheit und DevSecOps](/azure/cloud-adoption-framework/organize/cloud-security-application-security-devsecops)

- [Sicherheitscomplianceverwaltung](/azure/cloud-adoption-framework/organize/cloud-security-compliance-management)

- [Identität und Schlüssel](/azure/cloud-adoption-framework/organize/cloud-security-identity-keys)

