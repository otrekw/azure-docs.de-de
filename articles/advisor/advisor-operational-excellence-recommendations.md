---
title: Verbessern des optimalen Betriebs mit Advisor
description: Verwenden Sie Azure Advisor, um den optimalen Betrieb für Ihre Azure-Abonnements zu verbessern und weiterzuentwickeln.
ms.topic: article
ms.date: 10/24/2019
ms.openlocfilehash: 2b4c4726400134e4eec3868e155da47cb8c515b5
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2020
ms.locfileid: "87057636"
---
# <a name="achieve-operational-excellence-by-using-azure-advisor"></a>Sicherstellen des optimalen Betriebs mit Azure Advisor

Empfehlungen zum optimalen Betrieb in Azure Advisor können Sie bei folgenden Aspekten unterstützen: 
- Effizienz von Prozessen und Workflows.
- Verwaltbarkeit von Ressourcen.
- Bewährte Methoden für die Bereitstellung. 

Diese Empfehlungen stehen Ihnen auf der Registerkarte **Erstklassige Betriebsprozesse** des Advisor-Dashboards zur Verfügung.

## <a name="create-azure-service-health-alerts-to-be-notified-when-azure-problems-affect-you"></a>Erstellen von Azure Service Health-Warnungen für Benachrichtigungen bei Azure-Problemen

Es wird empfohlen, Azure Service Health-Warnungen einzurichten, um über Azure-Dienstprobleme benachrichtigt zu werden. [Azure Service Health](https://azure.microsoft.com/features/service-health/) ist ein kostenloser Dienst, der personalisierte Anleitungen und Unterstützung bietet, wenn Sie von einem Azure-Dienstproblem betroffen sind. Advisor identifiziert Abonnements, für die keine Warnungen konfiguriert sind, und empfiehlt deren Konfiguration.


## <a name="design-your-storage-accounts-to-prevent-reaching-the-maximum-subscription-limit"></a>Speicherkontenentwurf zum Vermeiden der maximalen Abonnementgrenze

In einer Azure-Region können maximal 250 Speicherkonten pro Abonnement unterstützt werden. Wenn Sie dieses Limit erreichen, können Sie keine weiteren Speicherkonten mit dieser Kombination aus Abonnement und Region erstellen. Advisor überprüft Ihre Abonnements und bietet Empfehlungen, damit Sie weniger Speicherkonten für Regionen/Abonnements entwerfen, bei denen das Höchstlimit bald erreicht ist.

## <a name="ensure-you-have-access-to-azure-cloud-experts-when-you-need-it"></a>Sicherstellen, dass Sie bei Bedarf Zugang zu Azure-Cloudexperten haben

Bei unternehmenskritischen Workloads ist es wichtig, bei Bedarf Zugang zu technischem Support zu haben. Advisor identifiziert potenzielle geschäftskritische Abonnements, die keinen technischen Support im Supportplan enthalten. Es wird ein Upgrade auf eine Option empfohlen, die den technischen Support umfasst.

## <a name="delete-and-re-create-your-pool-to-remove-a-deprecated-internal-component"></a>Löschen und erneutes Erstellen Ihres Pools, um eine veraltete interne Komponente zu entfernen

Wenn Ihr Pool eine veraltete interne Komponente verwendet, löschen Sie den Pool, und erstellen Sie ihn neu, um Stabilität und Leistung zu verbessern.

## <a name="repair-invalid-log-alert-rules"></a>Reparieren ungültiger Protokollwarnungsregeln

Azure Advisor erkennt Warnungsregeln mit ungültigen Abfragen im Bedingungsabschnitt. Sie können in Azure Monitor Protokollwarnungsregeln erstellen und diese zum Ausführen von Analyseabfragen in bestimmten Intervallen verwenden. Anhand der Ergebnisse der Abfrage wird ermittelt, ob eine Warnung ausgelöst werden muss. Es kann vorkommen, dass Analytics-Abfragen aufgrund von Änderungen an referenzierten Ressourcen, Tabellen oder Befehlen im Laufe der Zeit ungültig werden. Von Advisor wird empfohlen, die Abfrage in der Warnungsregel zu korrigieren, um eine automatische Deaktivierung zu verhindern und die Überwachung Ihrer Ressourcen in Azure sicherzustellen. [Weitere Informationen zur Behandlung von Problemen mit Warnungsregeln](https://aka.ms/aa_logalerts_queryrepair).

## <a name="use-azure-policy-recommendations"></a>Verwenden von Azure Policy-Empfehlungen

Azure Policy ist ein Dienst in Azure, mit dem Sie Richtlinien erstellen, zuweisen und verwalten können. Mit diesen Richtlinien werden Regeln und Auswirkungen für Ihre Ressourcen erzwungen. Die folgenden Azure Policy-Empfehlungen sollen Sie beim Sicherstellen des optimalen Betriebs unterstützen: 

**Markierungen verwalten.** Diese Richtlinie dient zum Hinzufügen oder Ersetzen des angegebenen Tags und des zugehörigen Werts, wenn eine Ressource erstellt oder aktualisiert wird. Vorhandene Ressourcen können durch Auslösen eines Wartungstasks gewartet werden. Tags für Ressourcengruppen werden von dieser Richtlinie nicht geändert.

**Erzwingen von geografischen Konformitätsanforderungen.** Mit dieser Richtlinie können Sie die Speicherorte einschränken, die Ihre Organisation beim Bereitstellen von Ressourcen angeben kann. 

**Angeben von zulässigen VM-SKUs für Bereitstellungen.** Über diese Richtlinie können Sie einen Satz von SKUs für virtuelle Computer angeben, die Ihre Organisation bereitstellen kann.

**Erzwingen Sie *Virtuelle Computer überwachen, die keine verwalteten Datenträger verwenden*.**

**Aktivieren Sie *Tag von der Ressourcengruppe erben*.** Diese Richtlinie dient zum Hinzufügen oder Ersetzen des angegebenen Tags aus der übergeordneten Ressourcengruppe und des zugehörigen Werts, wenn eine Ressource erstellt oder aktualisiert wird. Vorhandene Ressourcen können durch Auslösen eines Wartungstasks gewartet werden.

## <a name="next-steps"></a>Nächste Schritte

Hier finden Sie weitere Informationen zu Empfehlungen des Advisor:
* [Einführung in Advisor](advisor-overview.md)
* [Erste Schritte](advisor-get-started.md)
* [Kostenempfehlungen von Advisor](advisor-cost-recommendations.md)
* [Leistungsempfehlungen von Advisor](advisor-performance-recommendations.md)
* [Zuverlässigkeitsempfehlungen von Advisor](advisor-high-availability-recommendations.md)
* [Sicherheitsempfehlungen von Advisor](advisor-security-recommendations.md)
* [Advisor-REST-API](/rest/api/advisor/)
