---
title: Azure-Sicherheitsbaseline für Azure Policy
description: Die Azure Policy-Sicherheitsbaseline enthält Schrittanleitungen und Ressourcen für die Implementierung der Sicherheitsempfehlungen, die im Vergleichstest für die Azure-Sicherheit angegeben sind.
author: msmbaldwin
ms.service: azure-policy
ms.topic: conceptual
ms.date: 02/17/2021
ms.author: mbaldwin
ms.custom: subject-security-benchmark
ms.openlocfilehash: 6e8bb4cf715c6cb8d0729399c1985376de18687b
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "105561286"
---
# <a name="azure-security-baseline-for-azure-policy"></a>Azure-Sicherheitsbaseline für Azure Policy

Diese Sicherheitsbaseline wendet Empfehlungen des [Vergleichstests für die Azure-Sicherheit](../../../security/benchmarks/overview.md) auf Azure Policy an. Der Azure-Sicherheitsvergleichstest enthält Empfehlungen zum Schutz Ihrer Cloudlösungen in Azure. Der Inhalt wird nach den vom Vergleichstest für die Azure-Sicherheit definierten und den entsprechenden für Azure Policy geltenden Empfehlungen nach **Compliancebereichen** und **Sicherheitskontrollen** gruppiert. Nicht auf Azure Policy anwendbare **Steuerungen** wurden ausgeschlossen. Zum Anzeigen der vollständigen Zuordnung von Azure Policy zum Vergleichstest für die Azure-Sicherheit sehen Sie sich die [vollständige Zuordnungsdatei der Azure Policy-Sicherheitsbaseline](https://github.com/MicrosoftDocs/SecurityBenchmarks/tree/master/Azure%20Offer%20Security%20Baselines) an.

Eine Zuordnung der Steuerungen im Vergleichstest für die Azure-Sicherheit zu den integrierten Richtliniendefinitionen über die integrierte Initiative finden Sie unter [Integrationen zur Einhaltung gesetzlicher Bestimmungen: Vergleichstest für die Azure-Sicherheit](../samples/azure-security-benchmark.md).

In Azure Policy wird der Begriff _Ownership_ anstelle von _Zuständigkeit_ verwendet. Ausführliche Informationen zu _Ownership_ finden Sie unter [Azure Policy-Richtliniendefinitionen](./definition-structure.md#type) und [Gemeinsame Verantwortung in der Cloud](../../../security/fundamentals/shared-responsibility.md).

## <a name="logging-and-monitoring"></a>Protokollierung und Überwachung

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Protokollierung und Überwachung](../../../security/benchmarks/security-control-logging-monitoring.md).*

### <a name="23-enable-audit-logging-for-azure-resources"></a>2.3: Aktivieren der Überwachungsprotokollierung für Azure-Ressourcen

**Leitfaden**: Azure Policy verwendet automatisch aktivierte Aktivitätsprotokolle, um Elemente wie Ereignisquelle, Datum, Benutzer, Zeitstempel, Quelladressen, Zieladressen und andere nützliche Elemente einzuschließen.

- [Sammeln von Plattformprotokollen und -metriken mit Azure Monitor](../../../azure-monitor/essentials/diagnostic-settings.md)

- [Grundlegendes zur Protokollierung und zu verschiedenen Protokolltypen in Azure](../../../azure-monitor/essentials/platform-logs-overview.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

## <a name="identity-and-access-control"></a>Identität und Zugriffssteuerung

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Identität und Zugriffssteuerung](../../../security/benchmarks/security-control-identity-access-control.md).*

### <a name="33-use-dedicated-administrative-accounts"></a>3.3: Verwenden dedizierter Administratorkonten

**Leitfaden**: Erstellen Sie Standardbetriebsvorgänge für die Verwendung dedizierter Administratorkonten. Verwenden Sie die Identitäts- und Zugriffsverwaltung in Azure Security Center, um die Anzahl der Administratorkonten zu überwachen. Sie können auch eine Lösung für Just-in-Time-/Just-Enough-Zugriff aktivieren, indem Sie privilegierte Rollen von [Azure Active Directory Privileged Identity Management](../../../active-directory/privileged-identity-management/pim-configure.md) oder [Azure Resource Manager](../../../azure-resource-manager/management/overview.md) verwenden.

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung**: Der [Azure-Sicherheitsvergleichstest](/azure/governance/policy/samples/azure-security-benchmark) ist die Standardrichtlinieninitiative für Security Center und die Grundlage für die [Empfehlungen von Security Center](/azure/security-center/security-center-recommendations). Die Azure Policy-Definitionen für diese Kontrolle werden automatisch durch Security Center aktiviert. Warnungen für diese Kontrolle erfordern möglicherweise einen [Azure Defender](/azure/security-center/azure-defender)-Plan für die entsprechenden Dienste.

**Integrierte Azure Policy-Definitionen – Microsoft.GuestConfiguration**:

[!INCLUDE [Resource Policy for Microsoft.GuestConfiguration 3.3](../../../../includes/policy/standards/asb/rp-controls/microsoft.guestconfiguration-3-3.md)]

### <a name="36-use-secure-azure-managed-workstations-for-administrative-tasks"></a>3.6: Verwenden von sicheren, von Azure verwalteten Arbeitsstationen für Verwaltungsaufgaben

**Leitfaden**: Verwenden Sie Privileged Access Workstations (PAWs) mit mehrstufiger Authentifizierung, die für die Anmeldung bei Azure-Ressourcen und deren Konfiguration konfiguriert sind.

- [Informationen zu Arbeitsstationen mit privilegiertem Zugriff](https://4sysops.com/archives/understand-the-microsoft-privileged-access-workstation-paw-security-model/)

- [Planen einer Bereitstellung von Azure AD Multi-Factor Authentication](../../../active-directory/authentication/howto-mfa-getstarted.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

## <a name="data-protection"></a>Datenschutz

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Schutz von Daten](../../../security/benchmarks/security-control-data-protection.md).*

### <a name="46-use-azure-rbac-to-control-access-to-resources"></a>4.6: Verwenden von Azure RBAC zum Steuern des Zugriffs auf Ressourcen 

**Leitfaden**: Steuern Sie den Zugriff auf Azure Policy mithilfe der rollenbasierten Zugriffssteuerung in Azure (Azure Role-Based Access Control, Azure RBAC).

- [Azure RBAC-Berechtigungen in Azure Policy](../overview.md#azure-rbac-permissions-in-azure-policy)

- [Konfigurieren von Azure RBAC](../../../role-based-access-control/role-assignments-portal.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="49-log-and-alert-on-changes-to-critical-azure-resources"></a>4.9: Protokollieren und Warnen bei Änderungen an wichtigen Azure-Ressourcen

**Leitfaden**: Verwenden Sie Azure Monitor mit Aktivitätsprotokollen, um Warnungen zu erstellen, die ausgegeben werden, wenn Änderungen in Azure Policy vorgenommen werden.

- [Erstellen von Warnungen für Azure-Aktivitätsprotokollereignisse](../../../azure-monitor/alerts/alerts-activity-log.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

## <a name="inventory-and-asset-management"></a>Bestands- und Ressourcenverwaltung

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Bestands- und Ressourcenverwaltung](../../../security/benchmarks/security-control-inventory-asset-management.md).*

### <a name="62-maintain-asset-metadata"></a>6.2: Verwalten von Ressourcenmetadaten

**Leitfaden**: Wenden Sie Tags auf Ihre Azure-Ressourcen an, die Metadaten erzeugen, um sie logisch in einer Taxonomie zu organisieren. Verwenden Sie den Azure Policy-Effekt _Modify_, um die Einhaltung und konsistente Governance von Tags zu melden und zu erzwingen.

- [Tutorial: Erstellen und Verwalten von Richtlinien](../tutorials/create-and-manage.md)

- [Tutorial: Verwalten der Tag-Governance](../tutorials/govern-tags.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="64-define-and-maintain-inventory-of-approved-azure-resources"></a>6.4: Definieren und Verwalten des Bestands an genehmigten Azure-Ressourcen

**Leitfaden**: Erstellen Sie eine Liste genehmigter Richtliniendefinitionen und Richtlinienzuweisungen gemäß Ihren organisatorischen Anforderungen.

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="65-monitor-for-unapproved-azure-resources"></a>6.5: Überwachung auf nicht genehmigte Azure-Ressourcen

**Leitfaden**: Verwenden Sie Azure Policy, um Einschränkungen für den Typ der Ressourcen festzulegen, die in Ihren Abonnements erstellt werden können.

- [Konfigurieren und Verwalten von Azure Policy](../tutorials/create-and-manage.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

## <a name="next-steps"></a>Nächste Schritte

- Sehen Sie sich die [Übersicht über Version 2 des Azure-Sicherheitsvergleichstests](../../../security/benchmarks/overview.md) an.
- Erfahren Sie mehr über [Azure-Sicherheitsbaselines](../../../security/benchmarks/security-baselines-overview.md).