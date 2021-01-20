---
title: Azure Policy-Sicherheitsbaseline für den Vergleichstest für die Azure-Sicherheit
description: Die Azure Policy-Sicherheitsbaseline enthält Schrittanleitungen und Ressourcen für die Implementierung der Sicherheitsempfehlungen, die im Vergleichstest für die Azure-Sicherheit angegeben sind.
author: msmbaldwin
ms.service: azure-policy
ms.topic: conceptual
ms.date: 07/02/2020
ms.author: mbaldwin
ms.custom: subject-security-benchmark
ms.openlocfilehash: e8915e1c15972341befd176b412925f4e87c94f6
ms.sourcegitcommit: 2bd0a039be8126c969a795cea3b60ce8e4ce64fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/14/2021
ms.locfileid: "98201449"
---
# <a name="azure-policy-security-baseline-for-azure-security-benchmark"></a>Azure Policy-Sicherheitsbaseline für den Vergleichstest für die Azure-Sicherheit

Diese Sicherheitsbaseline wendet Empfehlungen des [Vergleichstests für die Azure-Sicherheit](../../../security/benchmarks/overview.md) auf Azure Policy an. Der Azure-Sicherheitsvergleichstest enthält Empfehlungen zum Schutz Ihrer Cloudlösungen in Azure. Der Inhalt wird nach den vom Vergleichstest für die Azure-Sicherheit definierten und den entsprechenden für Azure Policy geltenden Empfehlungen nach **Compliancebereichen** und **Sicherheitskontrollen** gruppiert. Nicht auf Azure Policy anwendbare **Steuerungen** wurden ausgeschlossen. Zum Anzeigen der vollständigen Zuordnung von Azure Policy zum Vergleichstest für die Azure-Sicherheit sehen Sie sich die [vollständige Zuordnungsdatei der Azure Policy-Sicherheitsbaseline](https://github.com/MicrosoftDocs/SecurityBenchmarks/tree/master/Azure%20Offer%20Security%20Baselines) an.

Eine Zuordnung der Steuerungen im Vergleichstest für die Azure-Sicherheit zu den integrierten Richtliniendefinitionen über die integrierte Initiative finden Sie unter [Integrationen zur Einhaltung gesetzlicher Bestimmungen: Vergleichstest für die Azure-Sicherheit](../samples/azure-security-benchmark.md).

In Azure Policy wird der Begriff _Ownership_ anstelle von _Zuständigkeit_ verwendet. Ausführliche Informationen zu _Ownership_ finden Sie unter [Azure Policy-Richtliniendefinitionen](./definition-structure.md#type) und [Gemeinsame Verantwortung in der Cloud](../../../security/fundamentals/shared-responsibility.md).


## <a name="logging-and-monitoring"></a>Protokollierung und Überwachung

*Weitere Informationen finden Sie unter [Sicherheitskontrolle: Protokollierung und Überwachung](../../../security/benchmarks/security-control-logging-monitoring.md).*

### <a name="23-enable-audit-logging-for-azure-resources"></a>2.3: Aktivieren der Überwachungsprotokollierung für Azure-Ressourcen

**Leitfaden**: Azure Policy verwendet automatisch aktivierte Aktivitätsprotokolle, um Elemente wie Ereignisquelle, Datum, Benutzer, Zeitstempel, Quelladressen, Zieladressen und andere nützliche Elemente einzuschließen.

* [Sammeln von Plattformprotokollen und -metriken mit Azure Monitor](../../../azure-monitor/platform/diagnostic-settings.md)

* [Grundlegendes zur Protokollierung und zu verschiedenen Protokolltypen in Azure](../../../azure-monitor/platform/platform-logs-overview.md)


**Azure Security Center-Überwachung**: Zurzeit nicht verfügbar

**Verantwortlichkeit**: Kunde

## <a name="identity-and-access-control"></a>Identität und Zugriffssteuerung

*Weitere Informationen finden Sie unter [Sicherheitskontrolle: Identität und Zugriffssteuerung](../../../security/benchmarks/security-control-identity-access-control.md).*

### <a name="33-use-dedicated-administrative-accounts"></a>3.3: Verwenden dedizierter Administratorkonten

**Leitfaden**: Erstellen Sie Standardbetriebsvorgänge für die Verwendung dedizierter Administratorkonten. Verwenden Sie die Identitäts- und Zugriffsverwaltung in Azure Security Center, um die Anzahl der Administratorkonten zu überwachen. 

Sie können auch eine Lösung für Just-in-Time-/Just-Enough-Zugriff aktivieren, indem Sie privilegierte Rollen von [Azure AD Privileged Identity Management](../../../active-directory/privileged-identity-management/pim-configure.md) oder [Azure Resource Manager](../../../azure-resource-manager/management/overview.md) verwenden.


**Azure Security Center-Überwachung**: Zurzeit nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="36-use-dedicated-machines-privileged-access-workstations-for-all-administrative-tasks"></a>3.6: Verwenden dedizierter Computer (Arbeitsstationen mit privilegiertem Zugriff) für alle administrativen Aufgaben

**Leitfaden**: Verwenden Sie Arbeitsstationen mit privilegiertem Zugriff (Privileged Access Workstations, PAWs) mit MFA, die für die Anmeldung bei und die Konfiguration von Azure-Ressourcen konfiguriert sind.

* [Informationen zu Arbeitsstationen mit privilegiertem Zugriff](https://4sysops.com/archives/understand-the-microsoft-privileged-access-workstation-paw-security-model/)

* [Aktivieren von MFA in Azure](../../../active-directory/authentication/howto-mfa-getstarted.md)


**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Kunde

## <a name="data-protection"></a>Schutz von Daten

*Weitere Informationen finden Sie unter [Sicherheitskontrolle: Datenschutz](../../../security/benchmarks/security-control-data-protection.md).*

### <a name="46-use-azure-rbac-to-control-access-to-resources"></a>4.6: Verwenden von Azure RBAC zum Steuern des Zugriffs auf Ressourcen

**Leitfaden**: Steuern Sie den Zugriff auf Azure Policy mithilfe der rollenbasierten Zugriffssteuerung in Azure (Azure Role-Based Access Control, Azure RBAC).

* [Azure RBAC-Berechtigungen in Azure Policy](../overview.md#azure-rbac-permissions-in-azure-policy)

* [Konfigurieren von Azure RBAC](../../../role-based-access-control/role-assignments-portal.md)


**Azure Security Center-Überwachung**: Zurzeit nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="49-log-and-alert-on-changes-to-critical-azure-resources"></a>4.9: Protokollieren und Warnen bei Änderungen an wichtigen Azure-Ressourcen

**Leitfaden**: Verwenden Sie Azure Monitor mit Aktivitätsprotokollen, um Warnungen zu erstellen, die ausgegeben werden, wenn Änderungen in Azure Policy vorgenommen werden.

* [Erstellen von Warnungen für Azure-Aktivitätsprotokollereignisse](../../../azure-monitor/platform/alerts-activity-log.md)


**Azure Security Center-Überwachung**: Zurzeit nicht verfügbar

**Verantwortlichkeit**: Kunde

## <a name="inventory-and-asset-management"></a>Bestands- und Ressourcenverwaltung

*Weitere Informationen finden Sie unter [Sicherheitskontrolle: Bestands- und Ressourcenverwaltung](../../../security/benchmarks/security-control-inventory-asset-management.md).*

### <a name="62-maintain-asset-metadata"></a>6.2: Verwalten von Ressourcenmetadaten

**Leitfaden**: Wenden Sie Tags auf Ihre Azure-Ressourcen an, die Metadaten erzeugen, um sie logisch in einer Taxonomie zu organisieren. Verwenden Sie den Azure Policy-Effekt _Modify_, um die Einhaltung und konsistente Governance von Tags zu melden und zu erzwingen.

* [Tutorial: Erstellen und Verwalten von Richtlinien](../tutorials/create-and-manage.md)

* [Tutorial: Verwalten der Tag-Governance](../tutorials/govern-tags.md)


**Azure Security Center-Überwachung**: Zurzeit nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="64-define-and-maintain-an-inventory-of-approved-azure-resources"></a>6.4: Definieren und Verwalten eines Bestands an genehmigten Azure-Ressourcen

**Leitfaden**: Erstellen Sie eine Liste genehmigter Richtliniendefinitionen und Richtlinienzuweisungen gemäß Ihren organisatorischen Anforderungen.

**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Kunde

### <a name="65-monitor-for-unapproved-azure-resources"></a>6.5: Überwachung auf nicht genehmigte Azure-Ressourcen

**Leitfaden**: Verwenden Sie Azure Policy, um Einschränkungen für den Typ der Ressourcen festzulegen, die in Ihren Abonnements erstellt werden können.

* [Konfigurieren und Verwalten von Azure Policy](../tutorials/create-and-manage.md)


**Azure Security Center-Überwachung**: Nicht verfügbar

**Verantwortlichkeit**: Kunde

## <a name="next-steps"></a>Nächste Schritte

- Lesen Sie den [Vergleichstest für die Azure-Sicherheit](../../../security/benchmarks/overview.md).
- Erfahren Sie mehr über [Azure-Sicherheitsbaselines](../../../security/benchmarks/security-baselines-overview.md).
