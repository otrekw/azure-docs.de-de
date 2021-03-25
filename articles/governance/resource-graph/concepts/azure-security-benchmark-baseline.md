---
title: Azure-Sicherheitsbaseline für Azure Resource Graph
description: Die Azure Resource Graph-Sicherheitsbaseline enthält Schrittanleitungen und Ressourcen für die Implementierung der Sicherheitsempfehlungen aus dem Azure-Sicherheitsvergleichstest.
author: msmbaldwin
ms.service: resource-graph
ms.topic: conceptual
ms.date: 02/17/2021
ms.author: mbaldwin
ms.custom: subject-security-benchmark
ms.openlocfilehash: e5e01c8d1ac16e5e8be405660a0726796789e645
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "101738972"
---
# <a name="azure-security-baseline-for-azure-resource-graph"></a>Azure-Sicherheitsbaseline für Azure Resource Graph

Diese Sicherheitsbaseline wendet Empfehlungen des [Azure-Sicherheitsvergleichstests Version 1.0](../../../security/benchmarks/overview-v1.md) auf Azure Resource Graph an. Der Azure-Sicherheitsvergleichstest enthält Empfehlungen zum Schutz Ihrer Cloudlösungen in Azure.
Der Inhalt wird nach den **Sicherheitskontrollen** gruppiert, die durch den Azure-Sicherheitsvergleichstest und durch die entsprechenden, für Azure Resource Graph geltenden Empfehlungen definiert werden. Für Azure Resource Graph nicht relevante **Kontrollen** wurden ausgeschlossen.

 
Die gesamte Zuordnung zwischen Azure Resource Graph und Azure-Sicherheitsvergleichstest finden Sie in der [vollständigen Zuordnungsdatei der Azure Resource Graph-Sicherheitsbaseline](https://github.com/MicrosoftDocs/SecurityBenchmarks/tree/master/Azure%20Offer%20Security%20Baselines).

## <a name="identity-and-access-control"></a>Identität und Zugriffssteuerung

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Identität und Zugriffssteuerung](../../../security/benchmarks/security-control-identity-access-control.md).*

### <a name="310-regularly-review-and-reconcile-user-access"></a>3.10: Regelmäßiges Überprüfen und Abstimmen des Benutzerzugriffs

**Leitfaden**: Azure Resource Graph ermöglicht den Zugriff auf Ressourcentypen und -eigenschaften auf der Grundlage der rollenbasierten Zugriffssteuerung von Azure (Azure Role-Based Access Control, Azure RBAC). Überwachen und überprüfen Sie regelmäßig den Zugriff, der Sicherheitsprinzipalen (Benutzern, Gruppen und Dienstkonten) gewährt wurde, um sicherzustellen, dass bei Abfragen Ergebnisse für die entsprechenden Ressourcen zurückgegeben werden.

- [Berechtigungen in Azure Resource Graph](https://docs.microsoft.com/azure/governance/resource-graph/overview#permissions-in-azure-resource-graph)

- [Verwenden von Zugriffsüberprüfungen für Azure-Identitäten](../../../active-directory/governance/access-reviews-overview.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

## <a name="data-protection"></a>Datenschutz

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Schutz von Daten](../../../security/benchmarks/security-control-data-protection.md).*

### <a name="46-use-azure-rbac-to-control-access-to-resources"></a>4.6: Verwenden von Azure RBAC zum Steuern des Zugriffs auf Ressourcen 

**Leitfaden**: Steuern Sie den Zugriff auf Daten und Ressourcen mithilfe der Azure RBAC (Role-Based Access Control, rollenbasierte Zugriffssteuerung). Für die Verwendung von Azure Resource Graph ist auch entsprechender Zugriff auf die abzufragenden Ressourcen erforderlich. Dabei sollte es sich um reinen Lesezugriff handeln, und dieser sollte lediglich den erforderlichen Mitarbeitern gewährt werden.

- [Berechtigungen in Azure Resource Graph](https://docs.microsoft.com/azure/governance/resource-graph/overview#permissions-in-azure-resource-graph)

- [Konfigurieren von Azure RBAC](../../../role-based-access-control/role-assignments-rest.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

## <a name="inventory-and-asset-management"></a>Bestands- und Ressourcenverwaltung

*Weitere Informationen finden Sie unter [Azure-Sicherheitsvergleichstest: Bestands- und Ressourcenverwaltung](../../../security/benchmarks/security-control-inventory-asset-management.md).*

### <a name="61-use-automated-asset-discovery-solution"></a>6.1: Verwenden der automatisierten Asset Discovery-Lösung

**Leitfaden**: Verwenden Sie Azure Resource Graph, um alle unterstützten Ressourcen innerhalb Ihrer Abonnements, Verwaltungsgruppen und Mandanten abzufragen und zu ermitteln. Stellen Sie sicher, dass Sie über entsprechende Berechtigungen in Ihrem Mandanten verfügen und alle Azure-Abonnements und Ressourcen in Ihren Abonnements auflisten können.

- [Erstellen von Abfragen mit Azure Resource Graph](../first-query-portal.md)

- [Grundlegendes zu Azure RBAC](../../../role-based-access-control/overview.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="64-define-and-maintain-inventory-of-approved-azure-resources"></a>6.4: Definieren und Verwalten des Bestands an genehmigten Azure-Ressourcen

**Leitfaden**: Erstellen Sie einen Bestand an genehmigten Azure-Ressourcen und genehmigter Software für Computeressourcen gemäß den Anforderungen Ihrer Organisation. Verwenden Sie Azure Resource Graph zum Abfragen genehmigter Azure-Ressourcen sowie den Änderungsverlauf (Vorschauversion), um Momentaufnahmen zu überprüfen und Änderungen zu ermitteln.

- [Abfragen von Azure-Ressourcen mit Azure Resource Graph](../first-query-portal.md)

- [Abrufen von Ressourcenänderungen](../how-to/get-resource-changes.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

### <a name="65-monitor-for-unapproved-azure-resources"></a>6.5: Überwachung auf nicht genehmigte Azure-Ressourcen

**Leitfaden**: Verwenden Sie Azure Resource Graph, um Ressourcen in Ihren Abonnements, Verwaltungsgruppen und Mandanten abzufragen und zu ermitteln. Stellen Sie sicher, dass alle Azure-Ressourcen in der Umgebung genehmigt sind.

- [Abfragen von Azure-Ressourcen mit Azure Resource Graph](../first-query-portal.md)

- [Beispiele: Einfache Abfragen für Azure Resource Graph](../samples/starter.md)

**Verantwortlichkeit**: Kunde

**Azure Security Center-Überwachung:** Keine

## <a name="next-steps"></a>Nächste Schritte

- Sehen Sie sich die [Übersicht über Version 2 des Azure-Sicherheitsvergleichstests](/azure/security/benchmarks/overview) an.
- Erfahren Sie mehr über [Azure-Sicherheitsbaselines](/azure/security/benchmarks/security-baselines-overview).
