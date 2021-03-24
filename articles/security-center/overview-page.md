---
title: 'Azure Security Center: Hauptdashboard oder Übersichtsseite'
description: Enthält eine Beschreibung der Features auf der Übersichtsseite von Security Center.
author: memildin
ms.author: memildin
ms.date: 03/04/2021
ms.topic: overview
ms.service: security-center
manager: rkarlin
ms.openlocfilehash: 768d69b626a870910d6734e1a1ddc29871f96afb
ms.sourcegitcommit: 4b7a53cca4197db8166874831b9f93f716e38e30
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "102099776"
---
# <a name="azure-security-centers-overview-page"></a>Azure Security Center-Übersichtsseite

Wenn Sie Azure Security Center öffnen, wird als Erstes die Übersichtsseite angezeigt. 

Dieses interaktive Dashboard ermöglicht eine einheitliche Übersicht über den Sicherheitsstatus Ihrer Hybrid Cloud-Workloads. Darüber hinaus werden darauf Sicherheitswarnungen, Informationen zur Abdeckung und andere Infos angezeigt.

Sie können ein beliebiges Element auf der Seite auswählen, um ausführlichere Informationen zu erhalten.

:::image type="content" source="media/overview-page/overview.png" alt-text="Security Center-Übersichtsseite":::

## <a name="features-of-the-overview-page"></a>Features der Übersichtsseite

:::image type="content" source="media/overview-page/top-bar-of-overview.png" alt-text="Leiste am oberen Bildschirmrand auf der Security Center-Übersichtsseite":::

Die **obere Menüleiste** enthält Folgendes:
- **Abonnements**: Sie können die Liste mit den Abonnements anzeigen und filtern, indem Sie diese Schaltfläche auswählen. Die Anzeige wird angepasst, um den Sicherheitsstatus der ausgewählten Abonnements widerzuspiegeln.
- **Neuigkeiten**: Öffnet die [Versionshinweise](release-notes.md), damit Sie sich über den neuesten Stand der neuen Features, Fehlerbehebungen und veralteten Funktionen informieren können.
- **Allgemeine Zahlenwerte** für die verbundenen Cloudkonten, um den Kontext zu den Informationen in den darunter angeordneten Kacheln anzuzeigen. Darüber hinaus wird die Anzahl der bewerteten Ressourcen, aktiven Empfehlungen und Sicherheitswarnungen gezeigt. Wählen Sie die Anzahl der bewerteten Ressourcen aus, um auf [Ressourcenbestand](asset-inventory.md) zuzugreifen. Informieren Sie sich darüber, wie Sie Ihre [AWS-Konten](quickstart-onboard-aws.md) und [GCP-Projekte](quickstart-onboard-gcp.md) verbinden.


In der Mitte der Seite befinden sich **vier zentrale Kacheln**, die jeweils mit einem dedizierten Dashboard mit weiteren Details verlinkt sind:
- **Sicherheitsbewertung**: Security Center führt eine ständige Bewertung Ihrer Ressourcen, Abonnements und Organisation in Bezug auf Sicherheitsprobleme durch. Anschließend werden alle Ergebnisse in einer einzigen Bewertung zusammengefasst, sodass Sie auf einen Blick Ihre aktuelle Sicherheitssituation erkennen können: je höher die Bewertung, desto geringer das ermittelte Risiko. [Weitere Informationen](secure-score-security-controls.md)
- **Konformität**: Security Center stellt basierend auf fortlaufenden Bewertungen Ihrer Azure-Umgebung Informationen zu Ihrem Konformitätsstatus (Compliance) bereit. Mit Security Center werden Risikofaktoren in Ihrer Hybrid Cloud-Umgebung anhand der bewährten Methoden für die Sicherheit analysiert. Diese Bewertungen sind Konformitätskontrollen zugeordnet, die aus dem unterstützten Standardsatz stammen. [Weitere Informationen](security-center-compliance-dashboard.md).
- **Azure Defender**: Die in Security Center integrierte Plattform für den Schutz von Cloudworkloads, die einen erweiterten, intelligenten Schutz Ihrer Azure- und Hybridworkloads ermöglicht. Auf der Kachel wird die Abdeckung für Ihre verbundenen Ressourcen (für die derzeit ausgewählten Abonnements) und die letzten Warnungen angezeigt, die je nach Schweregrad mit einer bestimmten Farbe gekennzeichnet sind. [Weitere Informationen](azure-defender.md)
- **Firewall Manager**: Diese Kachel zeigt den Status Ihrer Hubs und Netzwerke in [Azure Firewall Manager](../firewall-manager/overview.md). 


Der Bereich **Insights** enthält angepasste Elemente für Ihre Umgebung, die beispielsweise folgende Informationen liefern:
- Ihre Ressourcen mit den meisten Angriffen
- Ihre Sicherheitskontrollen mit dem höchsten Potenzial für die Erhöhung Ihrer Sicherheitsbewertung
- Aktive Empfehlungen mit den meisten betroffenen Ressourcen
- Aktuelle Blogbeiträge von Azure Security Center-Experten

## <a name="next-steps"></a>Nächste Schritte

Auf dieser Seite wurde die Übersichtsseite von Security Center beschrieben. Verwandte Informationen

- [Untersuchen und Verwalten Ihrer Ressourcen mit dem Ressourcenbestand und Verwaltungstools](asset-inventory.md)
- [Secure Score in Azure Security Center](secure-score-security-controls.md)