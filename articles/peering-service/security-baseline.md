---
title: Azure-Sicherheitsbaseline für Microsoft Azure Peering Service
description: Die Sicherheitsbaseline für Microsoft Azure Peering Service enthält Schrittanleitungen und Ressourcen für die Implementierung der Sicherheitsempfehlungen aus dem Azure-Sicherheitsvergleichstest.
author: msmbaldwin
ms.service: peering-service
ms.topic: conceptual
ms.date: 04/09/2021
ms.author: mbaldwin
ms.custom: subject-security-benchmark
ms.openlocfilehash: 854c669861c6aff71861481843bff97fdb7157ab
ms.sourcegitcommit: b4fbb7a6a0aa93656e8dd29979786069eca567dc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2021
ms.locfileid: "107315255"
---
# <a name="azure-security-baseline-for-microsoft-azure-peering-service"></a>Azure-Sicherheitsbaseline für Microsoft Azure Peering Service

Diese Sicherheitsbaseline wendet Empfehlungen des [Azure-Sicherheitsvergleichstests 2.0](../security/benchmarks/overview.md) auf Microsoft Azure Peering Service an. Der Azure-Sicherheitsvergleichstest enthält Empfehlungen zum Schutz Ihrer Cloudlösungen in Azure. Der Inhalt wird nach den **Sicherheitskontrollen** gruppiert, die durch den Azure-Sicherheitsvergleichstest und die entsprechenden, für Microsoft Azure Peering Service geltenden Empfehlungen definiert werden.

> [!NOTE]
> Nicht auf Microsoft Azure Peering Service anwendbare **Kontrollen** oder Kontrollen, für die Microsoft zuständig ist, wurden ausgeschlossen. Die vollständige Zuordnung zwischen Microsoft Azure Peering Service und Azure-Sicherheitsvergleichstest finden Sie in der **[vollständigen Zuordnungsdatei der Sicherheitsbaseline für Microsoft Azure Peering Service](https://github.com/MicrosoftDocs/SecurityBenchmarks/tree/master/Azure%20Offer%20Security%20Baselines)** .

## <a name="network-security"></a>Netzwerksicherheit

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Netzwerksicherheit](/azure/security/benchmarks/security-controls-v2-network-security).*

### <a name="ns-5-deploy-intrusion-detectionintrusion-prevention-systems-idsips"></a>NS-5: Bereitstellen von Angriffserkennungs-/Eindringschutzsystemen (Intrusion Detection/Intrusion Prevention Systems, IDS/IPS)

**Leitfaden**: Peering Service bietet keine nativen Dienste für die Angriffserkennung oder -verhinderung. Kunden sollten bewährte Sicherheitsmethoden anwenden, um ihre geschäftlichen Anforderungen zu erfüllen. Ein Beispiel wäre etwa das Threat Intelligence-gestützte Filtern von Azure Firewall, um Warnungen zu generieren und ein-/ausgehenden Datenverkehr für als schädlich bekannte IP-Adressen und Domänen zu blockieren. Diese IP-Adressen und Domänen stammen aus dem Threat Intelligence-Feed von Microsoft. 

Wenn eine Payloaduntersuchung erforderlich ist, können Sie Angriffserkennungs- oder Eindringschutzsysteme (IDS/IPS) von Drittanbietern mit Payloaduntersuchungsfunktionen aus dem Azure Marketplace bereitstellen.  
Alternativ können Sie eine hostbasierte EDR-Lösung (Endpoint Detection and Response, Endpunkterkennung und Reaktion) in Verbindung mit oder anstelle von netzwerkbasierten IDS-/IPS-Lösungen verwenden.  

Wenn Sie gesetzlich zur Verwendung eines Angriffserkennungs- oder Eindringschutzsystems verpflichtet sind, achten Sie darauf, dass es für die Nutzung oder Bereitstellung hochwertiger Warnungen optimiert ist. 

- [Bereitstellen von Azure Firewall](../firewall/tutorial-firewall-deploy-portal.md)

- [Azure Marketplace umfasst IDS-Funktionen von Drittanbietern](https://azuremarketplace.microsoft.com/marketplace?search=IDS)

- [ATP-EDR-Funktion von Microsoft Defender](/windows/security/threat-protection/microsoft-defender-atp/overview-endpoint-detection-response)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

## <a name="identity-management"></a>Identitätsverwaltung

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Identitätsverwaltung](/azure/security/benchmarks/security-controls-v2-identity-management).*

### <a name="im-1-standardize-azure-active-directory-as-the-central-identity-and-authentication-system"></a>IM-1: Standardisieren von Azure Active Directory als zentrales Identitäts- und Authentifizierungssystem

**Leitfaden**: Verwalten Sie einen Bestand der Benutzerkonten mit Administratorzugriff auf die Steuerungsebene (z. B. Azure-Portal) Ihrer Microsoft Azure Peering Service-Ressourcen.

Verwenden Sie die Identitäts- und Zugriffssteuerung (IAM) im Azure-Portal für Ihr Abonnement, um die rollenbasierte Zugriffssteuerung von Azure (Azure RBAC) zu konfigurieren. Die Rollen werden Benutzern, Gruppen, Dienstprinzipalen und verwalteten Identitäten in Azure Active Directory (Azure AD) zugewiesen.

- [Hinzufügen oder Entfernen von Azure-Rollenzuweisungen über das Azure-Portal](../role-based-access-control/role-assignments-portal.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="im-2-manage-application-identities-securely-and-automatically"></a>IM-2: Sicheres und automatisches Verwalten von Anwendungsidentitäten

**Leitfaden**: Verwaltete Identitäten werden für Peering Service nicht unterstützt. Verwenden Sie für Dienste, die wie Peering Service keine verwalteten Identitäten unterstützen, Azure Active Directory (Azure AD), um stattdessen einen Dienstprinzipal mit eingeschränkten Berechtigungen auf Ressourcenebene erstellen. 

- [Von Azure verwaltete Identitäten](../active-directory/managed-identities-azure-resources/overview.md)

- [Dienste, die verwaltete Identitäten für Azure-Ressourcen unterstützen](../active-directory/managed-identities-azure-resources/services-support-managed-identities.md)

- [Azure-Dienstprinzipal](/powershell/azure/create-azure-service-principal-azureps)

- [Erstellen eines Dienstprinzipals mit Zertifikaten](../active-directory/develop/howto-authenticate-service-principal-powershell.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="im-3-use-azure-ad-single-sign-on-sso-for-application-access"></a>IM-3: Verwenden von Azure AD Single Sign-on (SSO) für den Anwendungszugriff

**Leitfaden**: Microsoft Azure Peering Service verwendet Azure Active Directory (Azure AD), um die Identitäts- und Zugriffsverwaltung für Azure-Ressourcen bereitzustellen. Dies umfasst sowohl Unternehmensidentitäten wie Mitarbeiter als auch externe Identitäten wie Partner, Anbieter und Zulieferer. 

Verwenden Sie einmaliges Anmelden, um den Zugriff auf die Daten und Ressourcen Ihres Unternehmens lokal und in der Cloud zu verwalten und zu sichern. Verbinden Sie all Ihre Benutzer, Anwendungen und Geräte mit Azure AD, um einen nahtlosen, sicheren Zugriff sowie mehr Transparenz und Kontrolle zu ermöglichen.

- [Grundlegendes zu Anwendungs-SSO mit Azure AD](../active-directory/manage-apps/what-is-single-sign-on.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="im-4-use-strong-authentication-controls-for-all-azure-active-directory-based-access"></a>IM-4: Verwenden stärkerer Authentifizierungssteuerungen für den gesamten Azure Active Directory-basierten Zugriff

**Leitfaden**: Aktivieren Sie die mehrstufige Authentifizierung mit Azure Active Directory (Azure AD), und befolgen Sie die Empfehlungen für die Identitäts- und Zugriffsverwaltung aus Azure Security Center.

- [Planen einer Bereitstellung von Azure AD Multi-Factor Authentication](../active-directory/authentication/howto-mfa-getstarted.md)

- [Überwachen von Identität und Zugriff in Azure Security Center](../security-center/security-center-identity-access.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="im-5-monitor-and-alert-on-account-anomalies"></a>IM-5: Überwachen und Warnen bei Kontoanomalien

**Leitfaden**: Verwenden Sie Privileged Identity Management (PIM) mit Azure Active Directory (Azure AD) für die Generierung von Protokollen und Warnungen bei verdächtigen oder sicherheitsrelevanten Aktivitäten in der Umgebung. Verwenden Sie zusätzlich Azure AD-Risikoerkennungen, um Warnungen und Berichte zu riskantem Benutzerverhalten anzuzeigen.

- [Bereitstellen von Azure AD Privileged Identity Management (PIM)](../active-directory/privileged-identity-management/pim-deployment-plan.md)

- [Azure Active Directory-Risikoerkennungen](../active-directory/identity-protection/overview-identity-protection.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="im-6-restrict-azure-resource-access-based-on-conditions"></a>IM-6: Einschränken des Zugriffs auf Azure-Ressourcen basierend auf Bedingungen

**Leitfaden**: Verwenden Sie bedingten Zugriff mit Azure Active Directory (Azure AD) für eine präzisere Zugriffssteuerung auf der Grundlage benutzerdefinierter Bedingungen. So können Sie beispielsweise festlegen, dass bei Benutzeranmeldungen aus bestimmten IP-Bereichen die mehrstufige Authentifizierung verwendet werden muss. Eine differenzierte Authentifizierungssitzungsverwaltung kann auch über bedingte Azure AD-Zugriffsrichtlinien für verschiedene Anwendungsfälle erreicht werden. 

- [Übersicht über den bedingten Zugriff in Azure](../active-directory/conditional-access/overview.md)

- [Allgemeine Richtlinien für bedingten Zugriff](../active-directory/conditional-access/concept-conditional-access-policy-common.md)

- [Konfigurieren der Verwaltung von Authentifizierungssitzungen mit bedingtem Zugriff](../active-directory/conditional-access/howto-conditional-access-session-lifetime.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="im-7-eliminate-unintended-credential-exposure"></a>IM-7: Beheben der unbeabsichtigten Offenlegung von Anmeldeinformationen

**Leitfaden**: Implementieren Sie Credential Scanner innerhalb von Azure DevOps, um im Code verwendete Anmeldeinformationen zu identifizieren. In Credential Scanner wird auch das Verschieben von ermittelten Anmeldeinformationen an sicherere Speicherorte (z. B. Azure Key Vault) empfohlen.

Für GitHub können Sie das native Feature zur geheimen Überprüfung verwenden, um Anmeldeinformationen oder eine andere Form von Geheimnissen im Code zu identifizieren.

- [Einrichten von Credential Scanner](https://secdevtools.azurewebsites.net/helpcredscan.html)

- [Geheime Überprüfung in GitHub](https://docs.github.com/github/administering-a-repository/about-secret-scanning)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

## <a name="privileged-access"></a>Privilegierter Zugriff

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Privilegierter Zugriff](/azure/security/benchmarks/security-controls-v2-privileged-access).*

### <a name="pa-1-protect-and-limit-highly-privileged-users"></a>PA-1: Schützen und Einschränken stark privilegierter Benutzer

**Leitfaden**: Schränken Sie die Anzahl der Benutzerkonten mit hohen Privilegien ein, und schützen Sie diese Konten auf höherer Ebene. 

Die wichtigsten integrierten Rollen in Azure Active Directory (Azure AD) sind „Globaler Administrator“ und „Administrator für privilegierte Rollen“, da Benutzer, die diesen beiden Rollen zugewiesen sind, Administratorrollen delegieren können. 

Mit diesen Berechtigungen können Benutzer jede Ressource in Ihrer Azure-Umgebung direkt oder indirekt lesen und ändern:

- Globaler Administrator/Unternehmensadministrator: Benutzer mit dieser Rolle haben Zugriff auf alle administrativen Funktionen in Azure AD sowie auf Dienste, die Azure AD-Identitäten verwenden.

- Administrator für privilegierte Rollen: Benutzer mit dieser Rolle können Rollenzuweisungen in Azure AD und Azure AD Privileged Identity Management (PIM) verwalten. Überdies ermöglicht diese Rolle Verwaltung aller Aspekte von PIM und administrativer Einheiten.

Möglicherweise gibt es noch andere kritische Rollen, die gesteuert werden müssen, wenn Sie benutzerdefinierte Rollen mit bestimmten zugewiesenen privilegierten Berechtigungen verwenden. Wenden Sie ähnliche Kontrollen auf das Administratorkonto kritischer Unternehmensressourcen an.  

Ermöglichen Sie mit Azure AD Privileged Identity Management (PIM) privilegierten JIT-Zugriff (Just-In-Time) auf Azure-Ressourcen und Azure AD. JIT erteilt nur dann temporäre Berechtigungen zur Ausführung privilegierter Aufgaben, wenn die Benutzer sie benötigen. PIM kann auch Sicherheitswarnungen erzeugen, wenn es in Ihrer Azure AD-Organisation verdächtige oder unsichere Aktivitäten gibt.

- [Administratorrollenberechtigungen in Azure AD](/azure/active-directory/users-groups-roles/directory-assign-admin-roles)

- [Verwenden von Sicherheitswarnungen von Azure Privileged Identity Management](../active-directory/privileged-identity-management/pim-how-to-configure-security-alerts.md)

- [Schützen des privilegierten Zugriffs für hybride und Cloudbereitstellungen in Azure AD](/azure/active-directory/users-groups-roles/directory-admin-roles-secure)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="pa-2-restrict-administrative-access-to-business-critical-systems"></a>PA-2: Einschränken des administrativen Zugriffs auf unternehmenskritische Systeme

**Leitfaden:** Isolieren Sie den Zugriff auf unternehmenskritische Systeme, indem Sie Konten einschränken, denen privilegierter Zugriff auf die Abonnements und Verwaltungsgruppen gewährt wurde, zu denen sie gehören. 

Schränken Sie den Zugriff auf die Verwaltungs-, Identitäts- und Sicherheitssysteme ein, die über Administratorzugriff auf Ihre unternehmenskritischen Ressourcen verfügen. Beispiele wären etwa Active Directory-Domänencontroller (DCs), Sicherheitstools und Systemverwaltungstools mit Agents, die auf unternehmenskritischen Systemen installiert sind. Angreifer, die diese Verwaltungs- und Sicherheitssysteme kompromittieren, können diese sofort ausnutzen, um geschäftskritische Ressourcen zu gefährden. 

Alle Zugriffssteuerungstypen sollten an der Segmentierungsstrategie des Unternehmens ausgerichtet werden, um eine konsistente Zugriffssteuerung sicherzustellen. 

Vergewissern Sie sich, dass Sie separate privilegierte Konten zuweisen, die sich von den Standardbenutzerkonten unterscheiden, die für E-Mail-, Browser- und Produktivitätsaufgaben verwendet werden.

- [Azure-Komponenten und -Referenzmodell](/security/compass/microsoft-security-compass-introduction#azure-components-and-reference-model-2151)

- [Zugriff auf die Verwaltungsgruppe](https://docs.microsoft.com/azure/governance/management-groups/overview#management-group-access)

- [Azure-Abonnementadministratoren](../cost-management-billing/manage/add-change-subscription-administrator.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="pa-3-review-and-reconcile-user-access-regularly"></a>PA-3: Regelmäßiges Überprüfen und Abstimmen des Benutzerzugriffs

**Leitfaden**: Azure Active Directory (AAD) enthält Protokolle zum Ermitteln veralteter Konten. Verwenden Sie zusätzlich Zugriffsüberprüfungen für Azure-Identitäten, um Gruppenmitgliedschaften, den Zugriff auf Unternehmensanwendungen und Rollenzuweisungen effizient zu verwalten. Der Benutzerzugriff kann regelmäßig überprüft werden, um sicherzustellen, dass nur die richtigen Benutzer weiterhin Zugriff haben.

- [Speicherorte von Aktivitätsberichten im Azure-Portal](../active-directory/reports-monitoring/howto-find-activity-reports.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="pa-4-set-up-emergency-access-in-azure-ad"></a>PA-4: Einrichten des Notfallzugriffs in Azure AD

**Leitfaden**: Richten Sie ein Notfallzugriffskonto für den Fall ein, dass die regulären Administratorkonten nicht verfügbar sind, um zu verhindern, dass Sie versehentlich aus Ihrer Azure AD-Organisation (Azure Active Directory) ausgesperrt werden. Notfallzugriffskonten können höhere Berechtigungen enthalten und dürfen keinen bestimmten Personen zugewiesen werden. Beschränken Sie Notfallzugriffskonten auf Notfallszenarien.

Stellen Sie sicher, dass die Anmeldeinformationen (z. B. Kennwort, Zertifikat oder Smartcard) für Notfallzugriffskonten an einem sicheren Ort aufbewahrt werden und nur Personen bekannt sind, die für deren Verwendung im Notfall autorisiert sind.

- [Verwalten von Konten für den Notfallzugriff in Azure AD](/azure/active-directory/users-groups-roles/directory-emergency-access)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="pa-5-automate-entitlement-management"></a>PA-5: Automatisieren der Berechtigungsverwaltung 

**Leitfaden**: Verwenden Sie die Berechtigungsverwaltungsfeatures von Azure Active Directory (Azure AD), um die Zugriffsanforderungsworkflows einschließlich Zugriffszuweisungen, Überprüfungen und Gültigkeitsdauer zu automatisieren. Die duale oder die mehrstufige Genehmigung wird ebenfalls unterstützt.

- [Was sind Azure AD-Zugriffsüberprüfungen?](../active-directory/governance/access-reviews-overview.md)

- [Was ist die Azure AD-Berechtigungsverwaltung?](../active-directory/governance/entitlement-management-overview.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="pa-6-use-privileged-access-workstations"></a>PA-6: Verwenden von Privileged Access Workstations

**Leitfaden**: Verwenden Sie eine Privileged Access Workstation (PAW) mit mehrstufiger Authentifizierung über Azure Active Directory (Azure AD), um sich bei Ihren Azure Sentinel-bezogenen Ressourcen anzumelden und diese zu konfigurieren.

- [Privileged Access Workstations](/windows-server/identity/securing-privileged-access/privileged-access-workstations)

- [Planen einer cloudbasierten Azure AD Multi-Factor Authentication-Bereitstellung](../active-directory/authentication/howto-mfa-getstarted.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="pa-7-follow-just-enough-administration-least-privilege-principle"></a>PA-7: Befolgen Sie die Prinzipien der Just Enough Administration (Prinzip der geringsten Rechte) 

**Leitfaden**: Mit der rollenbasierten Zugriffssteuerung in Azure (Azure RBAC) kann der Zugriff auf Azure-Ressourcen über Rollenzuweisungen verwaltet werden. Diese Rollen können Benutzern, Gruppendienstprinzipalen und verwalteten Identitäten zugewiesen werden. Für bestimmte Ressourcen sind vordefinierte integrierte Rollen verfügbar. Diese Rollen können über Tools wie die Azure CLI, Azure PowerShell und das Azure-Portal inventarisiert oder abgefragt werden.

Die Berechtigungen, die Ressourcen über Azure RBAC zugewiesen werden, sollten immer auf die nötigsten Anforderungen der Rollen beschränkt sein. Eingeschränkte Berechtigungen ergänzen den JIT-Ansatz (Just-in-Time) von Azure Active Directory (Azure AD) Privileged Identity Management (PIM) und sollten regelmäßig überprüft werden.

Verwenden Sie integrierte Rollen, um Berechtigungen zuzuweisen und benutzerdefinierte Rollen nur bei Bedarf zu erstellen.

- [Was ist die rollenbasierte Zugriffssteuerung in Azure (Azure Role-Based Access Control, Azure RBAC)?](../role-based-access-control/overview.md)

- [Konfigurieren von Azure RBAC](../role-based-access-control/role-assignments-portal.md)

- [Was sind Azure AD-Zugriffsüberprüfungen?](../active-directory/governance/access-reviews-overview.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

## <a name="asset-management"></a>Ressourcenverwaltung

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Ressourcenverwaltung](/azure/security/benchmarks/security-controls-v2-asset-management).*

### <a name="am-1-ensure-security-team-has-visibility-into-risks-for-assets"></a>AM-1: Sicherstellen, dass das Sicherheitsteam Risiken für Ressourcen einsehen kann

**Leitfaden**: Stellen Sie sicher, dass Sicherheitsteams die Berechtigungen „Sicherheitsleseberechtigter“ in Ihrem Azure-Mandanten und Ihren Abonnements erhalten, damit sie mit Azure Security Center Sicherheitsrisiken überwachen können. 

Abhängig davon, wie die Verantwortungsbereiche des Sicherheitsteams strukturiert sind, kann die Überwachung auf Sicherheitsrisiken unter die Verantwortung eines zentralen Sicherheitsteams oder eines lokalen Teams fallen. Aus diesem Grund müssen Sicherheitserkenntnisse und -risiken immer innerhalb einer Organisation zentral aggregiert werden. 

Die Berechtigungen der Rolle „Sicherheitsleseberechtigter“ können weit gefasst auf einen gesamten Mandanten (Stammverwaltungsgruppe) angewandt oder auf Verwaltungsgruppen oder bestimmte Abonnements beschränkt werden. 

Möglicherweise sind zusätzliche Berechtigungen erforderlich, um Einblick in Workloads und Dienste zu erhalten. 

- [Übersicht über die Rolle „Sicherheitsleseberechtigter“](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#security-reader)

- [Übersicht über Azure-Verwaltungsgruppen](../governance/management-groups/overview.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="am-2-ensure-security-team-has-access-to-asset-inventory-and-metadata"></a>AM-2: Sicherstellen, dass das Sicherheitsteam Zugriff auf den Ressourcenbestand und die Metadaten hat

**Leitfaden**: Stellen Sie sicher, dass Sicherheitsteams Zugriff auf einen kontinuierlich aktualisierten Bestand der Ressourcen in Azure haben. Sicherheitsteams benötigen diesen Bestand häufig, um die potenziellen Angriffsflächen ihrer Organisation im Hinblick auf neue Risiken zu evaluieren, sowie als Quelle für die kontinuierliche Verbesserung der Sicherheit. 

Das Azure Security Center-Bestandsfeature und Azure Resource Graph können alle Ressourcen in Ihren Abonnements abfragen und ermitteln, einschließlich Azure-Diensten, Anwendungen und Netzwerkressourcen.  

Organisieren Sie Ressourcen logisch gemäß der Taxonomie Ihrer Organisation mithilfe von Tags und anderen Metadaten in Azure (Name, Beschreibung und Kategorie).  

- [Schnellstart: Ausführen Ihrer ersten Resource Graph-Abfrage mithilfe des Azure Resource Graph-Explorers](../governance/resource-graph/first-query-portal.md)

- [Verwalten des Ressourcenbestands in Azure Security Center](../security-center/asset-inventory.md)

- [Weitere Informationen zum Taggen von Ressourcen finden Sie im „Leitfaden zur Entscheidungsfindung für Ressourcenbenennung und -markierung“.](https://docs.microsoft.com/azure/cloud-adoption-framework/decision-guides/resource-tagging/?toc=/azure/azure-resource-manager/management/toc.json)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="am-3-use-only-approved-azure-services"></a>AM-3: Ausschließliche Verwendung genehmigter Azure-Dienste

**Leitfaden**: Microsoft Azure Peering Service unterstützt auf Azure Resource Manager basierende Bereitstellungen und die Erzwingung von Konfigurationen mithilfe von Azure Policy im Namespace „Microsoft.Peering/peerings“. Verwenden Sie Azure Policy, um die Dienste, die von Benutzern in Ihrer Umgebung bereitgestellt werden, zu überwachen und einzuschränken. Verwenden Sie Azure Resource Graph, um Ressourcen in ihren Abonnements abzufragen und zu ermitteln. Sie können auch mit Azure Monitor Regeln erstellen, mit denen Warnungen ausgelöst werden, wenn ein nicht genehmigter Dienst erkannt wird.

- [Konfigurieren und Verwalten von Azure Policy](../governance/policy/tutorials/create-and-manage.md)

- [Ablehnen eines bestimmten Ressourcentyps mit Azure Policy](https://docs.microsoft.com/azure/governance/policy/samples/built-in-policies#general)

- [Schnellstart: Ausführen Ihrer ersten Resource Graph-Abfrage mithilfe des Azure Resource Graph-Explorers](../governance/resource-graph/first-query-portal.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="am-5-limit-users-ability-to-interact-with-azure-resource-manager"></a>AM-5: Einschränken der Möglichkeiten von Benutzern zur Interaktion mit Azure Resource Manager

**Leitfaden**: Konfigurieren Sie bedingten Azure-Zugriff, um die Möglichkeiten der Benutzer zur Interaktion mit Azure Resource Manager einzuschränken, indem Sie „Zugriff blockieren“ für die App zur „Verwaltung von Microsoft Azure“ konfigurieren.

- [Verwalten des Zugriffs auf die Azure-Verwaltung mit bedingtem Zugriff](../role-based-access-control/conditional-access-azure-management.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

## <a name="logging-and-threat-detection"></a>Protokollierung und Bedrohungserkennung

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Protokollierung und Bedrohungserkennung](/azure/security/benchmarks/security-controls-v2-logging-threat-protection).*

### <a name="lt-1-enable-threat-detection-for-azure-resources"></a>LT-1: Aktivieren der Bedrohungserkennung für Azure-Ressourcen

**Leitfaden**: Überwachen Sie verschiedene Arten von Azure-Ressourcen auf potenzielle Bedrohungen und Anomalien. Das Ziel sollten möglichst hochwertige Warnungen sein. Damit verringern Sie die Anzahl falsch positiver Ergebnisse, die später von Analysten aussortiert werden müssen. Die Quellen von Warnungen können Protokolldaten, Agents oder andere Daten darstellen.

Verwenden Sie die integrierte Funktion zur Bedrohungserkennung in Azure Security Center, die auf der Überwachung der Telemetrie von Azure-Diensten und der Analyse von Dienstprotokollen basiert. Die Daten werden mit dem Log Analytics-Agent gesammelt. Der Agent liest verschiedene sicherheitsrelevante Konfigurationen und Ereignisprotokolle im System und kopiert die Daten zur Analyse in den Arbeitsbereich. 

Zusätzlich können Sie mithilfe von Azure Sentinel Analyseregeln erstellen, anhand derer in der gesamten Umgebung nach Bedrohungen gesucht wird, die bestimmten Kriterien entsprechen. Bei Übereinstimmung mit den Kriterien werden durch die Regeln Incidents generiert, die Sie einzeln untersuchen können. Azure Sentinel kann auch Threat Intelligence von Drittanbietern importieren, um dessen Funktion zur Bedrohungserkennung zu verbessern. 

- [Bedrohungsschutz in Azure Security Center](/azure/security-center/threat-protection)

- [Sicherheitswarnungen von Azure Security Center (Referenzhandbuch)](../security-center/alerts-reference.md)

- [Erstellen benutzerdefinierter Analyseregeln zum Erkennen von Bedrohungen](../sentinel/tutorial-detect-threats-custom.md)

- [Cyber Threat Intelligence mit Azure Sentinel](/azure/architecture/example-scenario/data/sentinel-threat-intelligence)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="lt-2-enable-threat-detection-for-azure-identity-and-access-management"></a>LT-2: Aktivieren der Bedrohungserkennung für die Identitäts- und Zugriffsverwaltung in Azure

**Leitfaden**: Azure Active Directory (Azure AD) stellt Benutzerprotokolle bereit, die in der Azure AD-Berichterstellung angezeigt oder in Azure Monitor integriert werden können. Diese Protokolle können auch in Azure Sentinel oder in andere SIEM-Lösungen oder Überwachungstools integriert werden, um Anwendungsfälle mit komplexerer Überwachung und Analyse zu ermöglichen:

- Anmeldungen: Der Bericht zu Anmeldungen enthält Informationen zur Nutzung von verwalteten Anwendungen sowie zu Benutzeranmeldeaktivitäten.

- Überwachungsprotokolle: Ermöglichen die Nachverfolgung sämtlicher Änderungen, die von verschiedenen Features in Azure AD vorgenommen wurden. Hierzu zählen unter anderem Änderungen an Ressourcen in Azure AD – beispielsweise das Hinzufügen oder Entfernen von Benutzern, Apps, Gruppen, Rollen und Richtlinien.

- Riskante Anmeldungen: Eine riskante Anmeldung ist ein Indikator für einen Anmeldeversuch einer Person, die nicht der rechtmäßige Besitzer eines Benutzerkontos ist.

- Benutzer mit Risikomarkierung: Ein Risikobenutzer ist ein Indikator für ein ggf. kompromittiertes Benutzerkonto.

Auch Azure Security Center kann bestimmte verdächtige Aktivitäten melden, beispielsweise eine übermäßige Anzahl nicht erfolgreicher Authentifizierungsversuche durch veraltete Konten in einem Abonnement. Neben der grundlegenden Überwachung der Sicherheitshygiene kann das Threat Protection-Modul in Security Center auch ausführliche Sicherheitswarnungen von einzelnen Azure-Computeressourcen (z. B. virtuelle Computer, Container, App Service), Datenressourcen (z. B. SQL-Datenbank und Speicher) und Azure-Dienstebenen sammeln. So erhalten Sie Einblick in Kontoanomalien innerhalb einzelner Ressourcen.

- [Berichte zu Überwachungsaktivitäten in Azure AD](../active-directory/reports-monitoring/concept-audit-logs.md)

- [Aktivieren von Azure Identity Protection](../active-directory/identity-protection/overview-identity-protection.md)

- [Bedrohungsschutz in Azure Security Center](/azure/security-center/threat-protection)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="lt-3-enable-logging-for-azure-network-activities"></a>LT-3: Aktivieren der Protokollierung für Azure-Netzwerkaktivitäten

**Leitfaden**: Konfigurieren Sie Verbindungstelemetrie, um Erkenntnisse hinsichtlich der Konnektivität zwischen Ihrem Verbindungsstandort und dem

Microsoft-Netzwerk über Microsoft Azure Peering Service zu erhalten.

- [Tutorial: Messen von Peering Service-Verbindungstelemetrie](measure-connection-telemetry.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="lt-4-enable-logging-for-azure-resources"></a>LT-4: Aktivieren der Protokollierung für Azure-Ressourcen

**Leitfaden**: Aktivieren Sie Azure-Aktivitätsprotokolle, und senden Sie die Protokolle zur Archivierung an einen Log Analytics-Arbeitsbereich, eine Azure Event Hub-Instanz oder ein Azure-Speicherkonto. 

Aktivitätsprotokolle geben Einblick in die Vorgänge, die für Ihre Azure ExpressRoute-Ressourcen auf Steuerungsebene erfolgen. Mit den Daten aus Azure-Aktivitätsprotokollen können Sie die Antworten auf die Fragen „Was“, „Wer“ und „Wann“ für alle Schreibvorgänge (PUT, POST, DELETE) ermitteln, die auf Steuerungsebene für Ihre Ressourcen durchgeführt werden.

- [Azure-Aktivitätsprotokoll](/azure/azure-monitor/platform/activity-log)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="lt-5-centralize-security-log-management-and-analysis"></a>LT-5: Zentralisieren der Verwaltung und Analyse von Sicherheitsprotokollen

**Leitfaden**: Zentralisieren Sie die Speicherung und Analyse von Protokollen, um eine Korrelation zu ermöglichen. Stellen Sie sicher, dass jeder Protokollquelle ein Datenbesitzer, eine Zugriffsanleitung, ein Speicherort, die für die Verarbeitung und den Zugriff verwendeten Tools sowie Anforderungen zur Datenaufbewahrung zugewiesen werden.

Stellen Sie sicher, dass die Azure-Aktivitätsprotokolle in die zentrale Protokollierung integriert werden. Erfassen von Protokollen über Azure Monitor zum Aggregieren von Sicherheitsdaten, die von Endpunktgeräten, Netzwerkressourcen und anderen Sicherheitssystemen generiert werden. Verwenden Sie in Azure Monitor Log Analytics-Arbeitsbereiche, um Analysen abzufragen und auszuführen, und verwenden Sie Azure Storage-Konten für langfristige Speicherung und Archivierung.

Zusätzlich können Sie auch Daten in Azure Sentinel oder der SIEM-Lösung eines Drittanbieters aktivieren und integrieren.

Viele Unternehmen verwenden Azure Sentinel für „heiße“ Daten, die häufig verwendet werden, und Azure Storage für seltener verwendete „kalte“ Daten.

- [Sammeln von Plattformprotokollen und -metriken mit Azure Monitor](../azure-monitor/essentials/diagnostic-settings.md)

- [Durchführen des Onboardings für Azure Sentinel](../sentinel/quickstart-onboard.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="lt-6-configure-log-storage-retention"></a>LT-6: Konfigurieren der Aufbewahrungsdauer von Protokollen im Speicher

**Leitfaden**: Legen Sie in Azure Monitor den Protokollaufbewahrungszeitraum für Log Analytics-Arbeitsbereiche, die Ihren Azure-Ressourcen zugeordnet sind, gemäß den Compliancevorschriften Ihrer Organisation fest.

- [Ändern des Datenaufbewahrungszeitraums](/azure/azure-monitor/platform/manage-cost-storage#change-the-data-retention-period)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

## <a name="incident-response"></a>Reaktion auf Vorfälle

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Reaktion auf Vorfälle](/azure/security/benchmarks/security-controls-v2-incident-response).*

### <a name="ir-1-preparation--update-incident-response-process-for-azure"></a>IR-1: Vorbereitung – Aktualisieren des Prozesses zur Reaktion auf Vorfälle für Azure

**Leitfaden**: Stellen Sie sicher, dass Ihre Organisation über Prozesse verfügt, um auf Sicherheitsvorfälle zu reagieren, dass diese Prozesse für Azure aktualisiert wurden und dass sie regelmäßig trainiert werden, um die Bereitschaft zu gewährleisten.

- [Implement security across the enterprise environment](/azure/cloud-adoption-framework/security/security-top-10#4-process-update-incident-response-ir-processes-for-cloud) (Implementieren von Sicherheit in der gesamten Unternehmensumgebung)

- [Referenzleitfaden für die Reaktion auf Vorfälle](/microsoft-365/downloads/IR-Reference-Guide.pdf)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="ir-2-preparation--setup-incident-notification"></a>IR-2: Vorbereitung – Einrichten von Ereignisbenachrichtigungen

**Leitfaden**: Richten Sie Kontaktinformationen für Sicherheitsvorfälle im Azure Security Center ein. Microsoft wendet sich unter den angegebenen Kontaktdaten an Sie, wenn das Microsoft Security Response Center (MSRC) feststellt, dass Personen unrechtmäßig oder unbefugt auf Ihre Daten zugegriffen haben. Sie haben auch die Möglichkeit, die Warnungen und Benachrichtigungen bei Vorfällen in verschiedenen Azure-Diensten entsprechend Ihrer Anforderungen bezüglich der Reaktion auf Vorfälle anzupassen. 

- [Festlegen der Kontaktinformationen in Azure Security Center](../security-center/security-center-provide-security-contact-details.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="ir-3-detection-and-analysis--create-incidents-based-on-high-quality-alerts"></a>IR-3: Erkennung und Analyse – Erstellen von Vorfällen basierend auf Warnungen mit hoher Qualität 

**Leitfaden**: Stellen Sie sicher, dass Sie über einen Prozess zum Erstellen hochwertiger Warnungen und zum Messen der Qualität von Warnungen verfügen. Auf diese Weise können Sie Lehren aus vergangenen Vorfällen ziehen und Warnungen für Analysten priorisieren, damit diese keine Zeit mit falsch-positiven Ergebnissen verschwenden. 

Qualitativ hochwertige Warnungen können auf der Grundlage von Erfahrungen aus früheren Vorfällen, validierten Communityquellen und Tools zur Generierung und Bereinigung von Warnmeldungen durch Verschmelzung und Korrelation verschiedener Signalquellen erstellt werden. 

Azure Security Center bietet qualitativ hochwertige Warnungen für viele Azure-Ressourcen. Sie können den ASC-Datenconnector verwenden, um die Warnungen an Azure Sentinel zu streamen. Mit Azure Sentinel können Sie erweiterte Warnregeln erstellen, um Vorfälle automatisch für eine Untersuchung zu generieren. 

Exportieren Sie Ihre Azure Security Center-Warnungen und -Empfehlungen über das Exportfeature, um Risiken für Azure-Ressourcen zu ermitteln. Exportieren Sie Warnungen und Empfehlungen entweder manuell oder fortlaufend, kontinuierlich.

- [Konfigurieren des Exports](../security-center/continuous-export.md)

- [Streamen von Warnungen in Azure Sentinel](../sentinel/connect-azure-security-center.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="ir-4-detection-and-analysis--investigate-an-incident"></a>IR-4: Erkennung und Analyse – Untersuchen eines Vorfalls

**Leitfaden**: Stellen Sie sicher, dass Analysten bei der Untersuchung potenzieller Vorfälle verschiedene Datenquellen abfragen und verwenden können, um eine vollständige Übersicht über den Vorfall zu erhalten. Es empfiehlt sich, verschiedene Protokolle zu sammeln, um die Aktivitäten eines potenziellen Angreifers über das gesamte Kill Chain-Spektrum hinweg nachzuverfolgen und Schwachpunkte zu vermeiden.  Stellen Sie außerdem sicher, dass Erkenntnisse und Erfahrungen für andere Analysten und als zukünftige Referenzen erfasst werden.  

Zu den zu untersuchenden Datenquellen gehören die zentralisierten Protokollierungsquellen, die bereits von den zum Umfang gehörige Diensten und ausgeführten Systemen gesammelt werden, aber auch andere:

- Netzwerkdaten: Verwenden Sie die Datenflussprotokolle von Netzwerksicherheitsgruppen sowie Azure Network Watcher und Azure Monitor, um Netzwerkflussprotokolle und andere Analyseinformationen zu erfassen. 

- Momentaufnahmen von laufenden Systemen: 

    - Verwenden Sie die Momentaufnahmenfunktion des virtuellen Azure-Computers, um eine Momentaufnahme der Festplatte des laufenden Systems zu erstellen. 

    - Verwenden Sie die native Speicherabbildfunktion des Betriebssystems, um eine Momentaufnahme des Arbeitsspeichers des laufenden Systems zu erstellen.

    - Verwenden Sie das Momentaufnahmenfeature der Azure-Dienste oder die Funktion Ihrer Software, um Momentaufnahmen der laufenden Systeme zu erstellen.

Azure Sentinel bietet umfangreiche Datenanalysen über praktisch jede Protokollquelle sowie ein Fallverwaltungsportal zum Verwalten des gesamten Lebenszyklus von Vorfällen. Intelligenceinformationen während einer Untersuchung können zu Verfolgungs- und Berichtszwecken mit einem Vorfall verknüpft werden. 

- [Momentaufnahme des Datenträgers eines Windows-Computers](../virtual-machines/windows/snapshot-copy-managed-disk.md)

- [Momentaufnahme des Datenträgers eines Linux-Computers](../virtual-machines/linux/snapshot-copy-managed-disk.md)

- [Microsoft Azure-Supportdiagnoseinformationen und Speicherabbilderfassung](https://azure.microsoft.com/support/legal/support-diagnostic-information-collection/) 

- Lesen Sie [Untersuchen von Incidents mit Azure Sentinel](../sentinel/tutorial-investigate-cases.md).

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="ir-5-detection-and-analysis--prioritize-incidents"></a>IR-5: Erkennung und Analyse – Priorisieren von Vorfällen

**Leitfaden**: Stellen Sie den Analysten Kontext bereit, auf welche Vorfälle sie sich zuerst konzentrieren sollten, je nach Schweregrad und Ressourcensensitivität. 

Azure Security Center weist jeder Warnung einen Schweregrad zu, damit Sie priorisieren können, welche Warnungen zuerst untersucht werden sollen. Der Schweregrad basiert darauf, wie zuversichtlich Security Center in Bezug auf den Befund oder die Analyse ist, der oder die zum Auslösen der Warnung verwendet wird, sowie auf dem Zuverlässigkeitsgrad, dass hinter der Aktivität, die zu der Warnung führte, eine böswillige Absicht stand.

Markieren Sie Ressourcen außerdem mithilfe von Tags, und erstellen Sie ein Benennungssystem, um Azure-Ressourcen eindeutig zu identifizieren und zu kategorisieren – insbesondere solche, von denen vertrauliche Daten verarbeitet werden.  Die Priorisierung der Behebung von Warnungen basierend auf der Wichtigkeit der Azure-Ressourcen und der Umgebung, in der der Vorfall aufgetreten ist, liegt in Ihrer Verantwortung.

- [Sicherheitswarnungen in Azure Security Center](../security-center/security-center-alerts-overview.md)

- [Verwenden von Tags zum Organisieren von Azure-Ressourcen](/azure/azure-resource-manager/resource-group-using-tags)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="ir-6-containment-eradication-and-recovery--automate-the-incident-handling"></a>IR-6: Eindämmung, Ausmerzung und Wiederherstellung – Automatisieren der Behandlung von Vorfällen

**Leitfaden**: Automatisieren Sie manuelle, sich wiederholende Aufgaben, um die Antwortzeit zu beschleunigen und die Belastung der Analysten zu verringern. Manuelle Aufgaben dauern länger, verlangsamen jeden Vorfall und reduzieren die Anzahl der Vorfälle, die ein Analyst bewältigen kann. Manuelle Aufgaben erhöhen auch die Ermüdung der Analytiker. Dadurch erhöht sich das Risiko menschlicher Fehler, die zu Verzögerungen führen, und es verschlechtert sich die Fähigkeit der Analytiker, sich effektiv auf komplexe Aufgaben zu konzentrieren. Verwenden Sie die Workflowautomatisierungsfeatures in Azure Security Center und Azure Sentinel, um automatisch Aktionen auszulösen oder ein Playbook auszuführen, um auf eingehende Sicherheitswarnungen zu reagieren. Das Playbook führt Aktionen aus, wie das Versenden von Benachrichtigungen, das Deaktivieren von Konten und das Isolieren problematischer Netzwerke. 

- [Konfigurieren der Workflowautomatisierung in Security Center](../security-center/workflow-automation.md)

- [Einrichten automatisierter Reaktionen auf Bedrohungen in Azure Security Center](https://docs.microsoft.com/azure/security-center/tutorial-security-incident#triage-security-alerts)

- Machen Sie sich mit dem [Einrichten automatisierter Reaktionen auf Bedrohungen in Azure Sentinel](../sentinel/tutorial-respond-threats-playbook.md) vertraut.

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

## <a name="posture-and-vulnerability-management"></a>Status- und Sicherheitsrisikoverwaltung

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Status- und Sicherheitsrisikoverwaltung](/azure/security/benchmarks/security-controls-v2-vulnerability-management).*

### <a name="pv-1-establish-secure-configurations-for-azure-services"></a>PV-1: Einrichten sicherer Konfigurationen für Azure-Dienste 

**Leitfaden**: Definieren Sie Sicherheitsleitlinien für Infrastruktur- und DevOps-Teams, indem Sie die sichere Konfiguration der verwendeten Azure-Dienste erleichtern. 

Beginnen Sie bei der Sicherheitskonfiguration von Azure-Diensten mit den Dienstbaselines in Azure Security Benchmark, und passen Sie sie nach Bedarf für Ihre Organisation an. 

Verwenden Sie Azure Security Center, um Azure Policy für die Überwachung und Durchsetzung von Konfigurationen Ihrer Azure-Ressourcen zu konfigurieren. 

Sie können Azure Blueprints verwenden, um die Bereitstellung und Konfiguration von Diensten und Anwendungsumgebungen wie Azure Resource Manager-Vorlagen, Azure RBAC-Steuerelementen und Richtlinien in einer einzigen Blaupausendefinition zu automatisieren.

- [Abbildung der Leitlinienimplementierung in der Landing Zone auf Unternehmensebene](/azure/cloud-adoption-framework/ready/enterprise-scale/architecture#landing-zone-expanded-definition)

- [Arbeiten mit Sicherheitsrichtlinien in Azure Security Center](../security-center/tutorial-security-policy.md)

- [Erstellen und Verwalten von Richtlinien zur Konformitätserzwingung](../governance/policy/tutorials/create-and-manage.md)

- [Azure Blueprint](../governance/blueprints/overview.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="pv-2-sustain-secure-configurations-for-azure-services"></a>PV-2: Aufrechterhalten sicherer Konfigurationen für Azure-Dienste

**Leitfaden**: Nicht zutreffend. Diese Empfehlung ist für Computeressourcen vorgesehen.

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="pv-8-conduct-regular-attack-simulation"></a>PV-8: Regelmäßiges Durchführen einer Angriffssimulation

**Leitfaden**: Führen Sie nach Bedarf Penetrationstests oder Red Team-Aktivitäten für Ihre Azure-Ressourcen durch, und stellen Sie sicher, dass alle kritischen Sicherheitsergebnisse behandelt werden.
Befolgen Sie die Einsatzregeln für Penetrationstests für die Microsoft Cloud, um sicherzustellen, dass die Penetrationstests nicht gegen Microsoft-Richtlinien verstoßen. Nutzen Sie die Microsoft-Strategie und Durchführung von Red Team- und Livewebsite-Penetrationstests für von Microsoft verwaltete Cloudinfrastruktur, Dienste und Anwendungen.

- [Penetrationstests in Azure](../security/fundamentals/pen-testing.md)

- [Penetrationstests – Rules of Engagement](https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=1) 

- [Microsoft Cloud Red Teaming](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e)

**Verantwortlichkeit**: Shared

**Azure Security Center-Überwachung:** Keine

## <a name="governance-and-strategy"></a>Governance und Strategie

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Governance und Strategie](/azure/security/benchmarks/security-controls-v2-governance-strategy).*

### <a name="gs-1-define-asset-management-and-data-protection-strategy"></a>GS-1: Definieren der Strategie für Ressourcenverwaltung und Datenschutz 

**Leitfaden**: Stellen Sie sicher, dass Sie eine klare Strategie für die kontinuierliche Überwachung und den Schutz von Systemen und Daten dokumentieren und kommunizieren. Priorisieren Sie die Ermittlung, die Bewertung, den Schutz und die Überwachung unternehmenskritischer Daten und Systeme. 

Diese Strategie sollte dokumentierte Anleitungen, Richtlinien und Standards für die folgenden Elemente umfassen: 

-   Datenklassifizierungsstandard in Übereinstimmung mit den Geschäftsrisiken

-   Sicherheitsorganisationseinblick in Risiken und Ressourcenbestand 

-   Genehmigung durch die Sicherheitsorganisation für die Nutzung der Azure-Dienste 

-   Sicherheit von Ressourcen durch ihren gesamten Lebenszyklus

-   Erforderliche Zugriffssteuerungsstrategie in Übereinstimmung mit der Organisationsdatenklassifizierung

-   Verwendung von nativen Azure- und Drittanbieterfunktionen für den Datenschutz

-   Datenverschlüsselungsanforderungen für Anwendungsfälle während der Übertragung und für ruhende Anwendungsfälle

-   Geeignete Verschlüsselungsstandards

Weitere Informationen finden Sie in den folgenden Referenzen:
- [Empfehlung für eine Azure-Sicherheitsarchitektur: Speicher, Daten, Verschlüsselung](https://docs.microsoft.com/azure/architecture/framework/security/storage-data-encryption?toc=/security/compass/toc.json&amp;bc=/security/compass/breadcrumb/toc.json)

- [Grundlegende Azure-Sicherheitsinformationen: Sicherheit, Verschlüsselung und Speicherung von Azure-Daten](../security/fundamentals/encryption-overview.md)

- [Cloud Adoption Framework: Bewährte Methoden für Datensicherheit und Datenverschlüsselung in Azure](https://docs.microsoft.com/azure/security/fundamentals/data-encryption-best-practices?toc=/azure/cloud-adoption-framework/toc.json&amp;bc=/azure/cloud-adoption-framework/_bread/toc.json)

- [Azure-Sicherheitsvergleichstest: Ressorcenverwaltung](/azure/security/benchmarks/security-benchmark-v2-asset-management)

- [Azure-Sicherheitsvergleichstest: Datenschutz](/azure/security/benchmarks/security-benchmark-v2-data-protection)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="gs-2-define-enterprise-segmentation-strategy"></a>GS-2: Definieren der Strategie für Unternehmenssegmentierung 

**Leitfaden**: Erstellen Sie eine unternehmensweite Strategie zum Segmentieren des Zugriffs auf Ressourcen durch eine Kombination aus Identität, Netzwerk, Anwendung, Abonnement, Verwaltungsgruppe und anderen Steuerelementen.

Wägen Sie die Notwendigkeit einer Sicherheitstrennung sorgfältig gegen die Notwendigkeit ab, den täglichen Betrieb der Systeme zu ermöglichen, die miteinander kommunizieren und auf Daten zugreifen müssen.

Stellen Sie sicher, dass die Segmentierungsstrategie einheitlich für alle Steuerelementtypen implementiert wird, einschließlich Netzwerksicherheit, Identitäts- und Zugriffsmodelle, Berechtigungs-/Zugriffsmodelle für Anwendungen sowie Steuerelemente für Benutzerprozesse.

- [Leitfaden für die Segmentierungsstrategie in Azure (Video)](/security/compass/microsoft-security-compass-introduction#azure-components-and-reference-model-2151)

- [Leitfaden für die Segmentierungsstrategie in Azure (Dokument)](/security/compass/governance#enterprise-segmentation-strategy)

- [Ausrichten der Netzwerksegmentierung an der Segmentierungsstrategie des Unternehmens](/security/compass/network-security-containment#align-network-segmentation-with-enterprise-segmentation-strategy)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="gs-3-define-security-posture-management-strategy"></a>GS-3: Definieren der Strategie für die Sicherheitsstatusverwaltung

**Leitfaden**: Führen Sie kontinuierliche Messungen durch, und Mindern Sie die Risiken für Ihre individuellen Ressourcen und die Umgebung, in der diese gehostet werden. Priorisieren Sie Ressourcen mit hohem Wert und hoch exponierte Angriffsflächen, wie z. B. veröffentlichte Anwendungen, Eingangs- und Ausgangspunkte für Netzwerke, Benutzer- und Administratorendpunkte usw.

- [Azure-Sicherheitsvergleichstest: Verwaltung von Status und Sicherheitsrisiken](/azure/security/benchmarks/security-benchmark-v2-posture-vulnerability-management)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="gs-4-align-organization-roles-responsibilities-and-accountabilities"></a>GS-4: Ausrichten von Organisationsrollen, Zuständigkeiten und Verantwortlichkeiten

**Leitfaden**: Stellen Sie sicher, dass Sie eine klare Strategie für Rollen und Zuständigkeiten in ihrer Sicherheitsorganisation dokumentieren und kommunizieren. Priorisieren Sie durch Zuweisung klarer Verantwortlichkeiten für Sicherheitsentscheidungen, Schulung aller Beteiligten für das Modell der gemeinsamen Verantwortung und Schulung von technischen Teams hinsichtlich der Technologie zum Sichern der Cloud.

- [Azure Security Best Practice 1 – People: Educate Teams on Cloud Security Journey](/azure/cloud-adoption-framework/security/security-top-10#1-people-educate-teams-about-the-cloud-security-journey) (Bewährte Methoden für die Sicherheit von Azure 1 – Personen: Schulen von Teams auf dem Weg zur Cloudsicherheit)

- [Azure Security Best Practice 2 – People: Educate Teams on Cloud Security Technology](/azure/cloud-adoption-framework/security/security-top-10#2-people-educate-teams-on-cloud-security-technology) (Bewährte Methoden für die Sicherheit von Azure 1 – Personen: Schulen der Teams in Cloudsicherheitstechnologie)

- [Azure Security Best Practice 3 – Process: Assign Accountability for Cloud Security Decisions](/azure/cloud-adoption-framework/security/security-top-10#4-process-update-incident-response-ir-processes-for-cloud) (Bewährte Methoden für die Sicherheit von Azure 1 – Prozess: Zuweisen von Verantwortlichkeit für Cloudsicherheitsentscheidungen)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="gs-5-define-network-security-strategy"></a>GS-5: Definieren einer Netzwerksicherheitsstrategie

**Leitfaden**: Richten Sie einen Ansatz für die Azure-Netzwerksicherheit im Rahmen der Gesamtstrategie Ihrer Organisation für die Sicherheitszugriffssteuerung ein.  

Diese Strategie sollte dokumentierte Anleitungen, Richtlinien und Standards für die folgenden Elemente umfassen: 

-   Zentralisieren der Verantwortlichkeit für Netzwerkverwaltung und -sicherheit

-   Segmentierungsmodell für virtuelle Netzwerke, das auf die Segmentierungsstrategie des Unternehmens abgestimmt ist

-   Wiederherstellungsstrategie in verschiedenen Bedrohungs- und Angriffsszenarien

-   Strategie für Internet-Randpunkte, -Eingangspunkte und -Ausgangspunkte

-   Strategie für Konnektivität zwischen Hybrid Cloud und lokalen Systemen

-   Aktuelle Netzwerksicherheitsartefakte (z. B. Netzwerkdiagramme, Referenznetzwerkarchitektur)

Weitere Informationen finden Sie in den folgenden Referenzen:
- [Azure Security Best Practice 11 – Architecture. Single unified security strategy](/azure/cloud-adoption-framework/security/security-top-10#11-architecture-establish-a-single-unified-security-strategy) (Bewährte Methoden für die Sicherheit von Azure 11 – Architektur: Zentralisierte einheitliche Sicherheitsstrategie)

- [Azure-Sicherheitsvergleichstest: Netzwerksicherheit](/azure/security/benchmarks/security-benchmark-v2-network-security)

- [Die Netzwerksicherheit in Azure in der Übersicht](../security/fundamentals/network-overview.md)

- [Strategie für die Unternehmensnetzwerkarchitektur](/azure/cloud-adoption-framework/ready/enterprise-scale/architecture)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="gs-6-define-identity-and-privileged-access-strategy"></a>GS-6: Definieren der Strategie für Identität und privilegierten Zugriff

**Leitfaden**: Richten Sie einen Ansatz für Azure-Identität und privilegierten Zugriff im Rahmen der Gesamtstrategie Ihrer Organisation für die Sicherheitszugriffssteuerung ein.  

Diese Strategie sollte dokumentierte Anleitungen, Richtlinien und Standards für die folgenden Elemente umfassen: 

-   Zentralisiertes Identitäts- und Authentifizierungssystem mit Interkonnektivität zu anderen internen und externen Identitätssystemen

-   Starke Authentifizierungsmethoden in verschiedenen Anwendungsfällen und Bedingungen

-   Schutz stark privilegierter Benutzer

-   Überwachung und Behandlung von Anomalien bei Benutzeraktivitäten  

-   Benutzeridentität und Zugriffsüberprüfung und Abstimmungsprozess

Weitere Informationen finden Sie in den folgenden Referenzen:

- [Azure-Sicherheitsvergleichstest: Identitätsverwaltung](/azure/security/benchmarks/security-benchmark-v2-identity-management)

- [Azure-Sicherheitsvergleichstest: Privilegierter Zugriff](/azure/security/benchmarks/security-benchmark-v2-privileged-access)

- [Azure Security Best Practice 11 – Architecture. Single unified security strategy](/azure/cloud-adoption-framework/security/security-top-10#11-architecture-establish-a-single-unified-security-strategy) (Bewährte Methoden für die Sicherheit von Azure 11 – Architektur: Zentralisierte einheitliche Sicherheitsstrategie)

- [Übersicht über die Sicherheit der Azure-Identitätsverwaltung](../security/fundamentals/identity-management-overview.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="gs-7-define-logging-and-threat-response-strategy"></a>GS-7: Definieren einer Strategie für Protokollierung und Reaktion auf Bedrohungen

**Leitfaden**: Richten Sie eine Strategie für Protokollierung und Reaktion auf Bedrohungen zur schnellen Erkennung und Behebung von Bedrohungen bei gleichzeitiger Erfüllung von Complianceanforderungen ein. Priorisieren Sie die Bereitstellung wertvoller Warnungen und nahtloser Funktionen für Analysten, damit sie sich auf Bedrohungen konzentrieren können, anstatt mit der Integration und der Ausführung manueller Schritte. 

Diese Strategie sollte dokumentierte Anleitungen, Richtlinien und Standards für die folgenden Elemente umfassen: 

-   Rolle und Verantwortlichkeiten der Organisation für Sicherheitsvorgänge (SecOps) 

-   Ein klar definierter Prozess zur Reaktion auf Vorfälle gemäß NIST oder einem anderen Branchenframework 

-   Erfassung und Aufbewahrung von Protokollen zur Unterstützung von Bedrohungserkennung, Reaktion auf Vorfälle und Complianceanforderungen

-   Zentralisierte Sichtbarkeit von und Korrelationsinformationen zu Bedrohungen unter Verwendung von SIEM, nativen Azure-Funktionen und anderen Quellen 

-   Kommunikations- und Benachrichtigungsplan mit Ihren Kunden, Lieferanten und öffentlichen Interessengruppen

-   Verwendung von nativen Azure-Plattformen und Plattformen von Drittanbietern für die Behandlung von Vorfällen, wie z. B. Protokollierung und Bedrohungserkennung, Forensik sowie Reduzierung und Bekämpfung von Angriffen

-   Prozesse für den Umgang mit Vorfällen und Aktivitäten nach Vorfällen, wie z. B. Erkenntnisse und Beweissicherung

Weitere Informationen finden Sie in den folgenden Referenzen:

- [Azure-Sicherheitsvergleichstest: Protokollierung und Bedrohungserkennung](/azure/security/benchmarks/security-benchmark-v2-logging-threat-detection)

- [Azure-Sicherheitsvergleichstest: Reaktion auf Vorfälle](/azure/security/benchmarks/security-benchmark-v2-incident-response)

- [Azure Security Best Practice 4 – Process. Update Incident Response Processes for Cloud](/azure/cloud-adoption-framework/security/security-top-10#4-process-update-incident-response-ir-processes-for-cloud) (Bewährte Methoden für die Sicherheit von Azure 4 – Prozess: Aktualisieren der Prozesse zur Reaktion auf Vorfälle für die Cloud)

- [Azure Adoption Framework, Entscheidungshilfe für Protokollierung und Berichterstattung](/azure/cloud-adoption-framework/decision-guides/logging-and-reporting/)

- [Azure auf Unternehmensebene, Verwaltung und Überwachung](/azure/cloud-adoption-framework/ready/enterprise-scale/management-and-monitoring)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

## <a name="next-steps"></a>Nächste Schritte

- Sehen Sie sich die [Übersicht über Version 2 des Azure-Sicherheitsvergleichstests](/azure/security/benchmarks/overview) an.
- Erfahren Sie mehr über [Azure-Sicherheitsbaselines](/azure/security/benchmarks/security-baselines-overview).
