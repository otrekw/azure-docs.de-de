---
title: Index der Blaupausenbeispiele
description: Index der Beispiele für Compliance und Standards für die Bereitstellung von Umgebungen, Richtlinien und Cloud Adoption Framework-Grundlagen mit Azure Blueprints.
ms.date: 02/01/2021
ms.topic: sample
ms.openlocfilehash: 17bacecf8fcf7c0f1969a489561a3a3ac976872e
ms.sourcegitcommit: 2dd0932ba9925b6d8e3be34822cc389cade21b0d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/01/2021
ms.locfileid: "99225140"
---
# <a name="azure-blueprints-samples"></a>Azure Blueprints-Beispiele

In der folgenden Tabelle sind Links zu Beispielen für Azure Blueprints enthalten. Jedes Beispiel hat Produktionsqualität und kann umgehend bereitgestellt werden, um Sie bei der Erfüllung Ihrer individuellen Complianceanforderungen zu unterstützen.

## <a name="standards-based-blueprint-samples"></a>Standardbasierte Blaupausenbeispiele

| Beispiel | BESCHREIBUNG |
|---------|---------|
| [ISM PROTECTED der australischen Regierung](./ism-protected/index.md) | Enthält Leitlinien für die Einhaltung von ISM PROTECTED der australischen Regierung. |
| [Azure-Sicherheitsvergleichstest v1](./azure-security-benchmark.md) | Enthält Leitlinien für die Einhaltung des [Azure-Sicherheitsvergleichstests v1](../../../security/benchmarks/overview.md). |
| [Canada Federal PBMM](./canada-federal-pbmm/index.md) | Enthält Leitlinien für die Einhaltung von PBMM (Canada Federal Protected B, Medium Integrity, Medium Availability). |
| [CIS Microsoft Azure Foundations Benchmark](./cis-azure-1-1-0.md)| Hier finden Sie eine Reihe von Richtlinien, die Ihnen bei der Einhaltung der Empfehlungen von CIS Microsoft Azure Foundations Benchmark helfen. |
| [DoD-Auswirkungsstufe 4](./dod-impact-level-4/index.md) | Hier finden Sie eine Reihe von Richtlinien, die Ihnen bei der Einhaltung der DoD-Auswirkungsstufe 4 helfen. |
| [DoD-Auswirkungsstufe 5](./dod-impact-level-5/index.md) | Hier finden Sie eine Reihe von Richtlinien, die Ihnen bei der Einhaltung der DoD-Auswirkungsstufe 5 helfen. |
| [FedRAMP Moderate](./fedramp-m/index.md) | Hier finden Sie eine Reihe von Richtlinien, die Ihnen bei der Einhaltung von „FedRAMP Moderate“ helfen. |
| [FedRAMP High](./fedramp-h/index.md) | Hier finden Sie eine Reihe von Richtlinien, die Ihnen bei der Einhaltung von „FedRAMP High“ helfen. |
| [HIPAA HITRUST 9.2](./hipaa-hitrust-9-2.md) | Hier finden Sie eine Reihe von Richtlinien, die Ihnen bei der Einhaltung von HIPAA HITRUST helfen. |
| [IRS 1075](./irs-1075/index.md) | Enthält Leitlinien für die Einhaltung von IRS 1075.|
| [ISO 27001](./iso-27001-2013.md) | Enthält Leitlinien für die Einhaltung von ISO 27001. |
| [ISO 27001: Gemeinsame Dienste](./iso27001-shared/index.md) | Stellt eine Reihe von konformen Infrastrukturmustern und Schutzmaßnahmen für Richtlinien bereit, die für den ISO 27001-Nachweis hilfreich sind. |
| [ISO 27001: App Service-Umgebungs-/SQL-Datenbank-Workload](./iso27001-ase-sql-workload/index.md) | Stellt eine zusätzliche Infrastruktur für das Blaupausenbeispiel [ISO 27001: Gemeinsame Dienste](./iso27001-shared/index.md) bereit. |
| [Medien](./media/index.md) | Hier finden Sie eine Reihe von Richtlinien, die Ihnen bei der Erfüllung der Anforderungen der Motion Picture Association of America (MPAA) helfen. |
| [NIST SP 800-53 R4](./nist-sp-800-53-r4.md) | Enthält Leitlinien für die Einhaltung von NIST SP 800-53 R4. |
| [NIST SP 800-171 R2](./nist-sp-800-171-r2.md) | Enthält Leitlinien für die Einhaltung von NIST SP 800-171 R2. |
| [PCI-DSS v3.2.1](./pci-dss-3.2.1/index.md) | Stellt verschiedene Richtlinien bereit, die Sie bei der Einhaltung von PCI-DSS v3.2.1 unterstützen. |
| [SWIFT CSP-CSCF v2020](./swift-2020/index.md) | Unterstützt Sie bei der Einhaltung von SWIFT CSP-CSCF v2020. |
| [UK OFFICIAL und UK NHS: Governance](./ukofficial/index.md) | Stellt eine Reihe von konformen Infrastrukturmustern und Schutzmaßnahmen für Richtlinien bereit, die für den UK OFFICIAL- und UK NHS-Nachweis hilfreich sind. |
| [CAF-Basis](./caf-foundation/index.md) | Bietet eine Reihe von Kontrollen, die Sie bei der Verwaltung Ihrer Cloudressourcen im Einklang mit dem [Microsoft Cloud Adoption Framework (CAF) für Azure](/azure/architecture/cloud-adoption/governance/journeys/index) unterstützen. |
| [CAF-Migrationslandezone](./caf-migrate-landing-zone/index.md) | Bietet eine Reihe von Kontrollen, die Sie bei der Vorbereitung der Migration Ihrer ersten Workload sowie bei der Verwaltung Ihrer Cloudressourcen im Einklang mit dem [Microsoft Cloud Adoption Framework (CAF) für Azure](/azure/architecture/cloud-adoption/migrate/index) unterstützen. |

## <a name="samples-strategy"></a>Strategie für Beispiele

:::image type="complex" source="../media/blueprint-samples-strategy.png" alt-text="Diagramm, in dem dargestellt ist, wo die Blaupausenbeispiele hinsichtlich Architekturkomplexität und Complianceanforderungen passen" border="false":::
   Beschreibt ein Koordinatensystem, in dem die Architekturkomplexität auf der x-Achse und die Complianceanforderungen auf der y-Achse dargestellt ist.  Wenn sich die Architekturkomplexität erhöht und die Complianceanforderungen steigen, verwenden Sie die standardmäßigen Blaupausenbeispiele im Portal, die durch Region E markiert sind. Kunden, die die ersten Schritte mit Azure ausführen, nutzen die durch Region A und B bezeichneten, auf Cloud Adoption Framework (CAF) basierenden Basis- und Zielzonenblaupausen. Der verbleibende Platz wird benutzerdefinierten Blaupausen zugeordnet, die von Kunden erstellt werden, die Partner für die Regionen C, D und F sind. :::image-end:::

Bei der CAF-Basisblaupause und der CAF-Blaupause für die Migrationslandezone wird davon ausgegangen, dass der Kunde ein vorhandenes, sauberes Einzelabonnement für die Migration lokaler Ressourcen und Workloads zu Azure vorbereitet.
(Regionen A und B in der Abbildung)  

Es besteht die Möglichkeit, die Beispielblaupausen zu durchlaufen und bei den Anpassungen, die ein Kunde anwendet, nach Mustern zu suchen. Darüber hinaus besteht die Möglichkeit, branchenspezifische Blaupausen (etwa für Finanzdienstleistungen und E-Commerce; oberer Bereich der Region B) proaktiv zu berücksichtigen. Analog dazu planen wir die Erstellung von Blaupausen für komplexe Architekturaspekte wie mehrere Abonnements, Hochverfügbarkeit, regionsübergreifende Ressourcen und Kunden, die Kontrollen für bereits vorhandene Abonnements und Ressourcen implementieren (Regionen C und D).

Es gibt Beispielblaupausen für Kundenszenarien mit hohen Complianceanforderungen und hochkomplexer Architektur (Region E in der Abbildung). Region F in der Abbildung ist für Kunden und Partner vorgesehen, die die Beispielblaupausen anwenden und an ihre individuellen Anforderungen anpassen.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über den [Lebenszyklus von Blaupausen](../concepts/lifecycle.md).
- Machen Sie sich mit der Verwendung [statischer und dynamischer Parameter](../concepts/parameters.md) vertraut.
- Erfahren Sie, wie Sie die [Abfolge von Blaupausen](../concepts/sequencing-order.md) anpassen können.
- Erfahren Sie, wie Sie [Ressourcen in Blaupausen sperren](../concepts/resource-locking.md) können.
- Lernen Sie, wie Sie [vorhandene Zuweisungen aktualisieren](../how-to/update-existing-assignments.md).
- Beheben Sie Probleme bei der Blaupausenzuweisung mithilfe des [allgemeinen Leitfadens zur Problembehandlung](../troubleshoot/general.md).
