---
title: Preise für Azure Security Center
description: Azure Security Center wird in zwei Modi mit und ohne Azure Defender angeboten.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 4d1364cd-7847-425a-bb3a-722cb0779f78
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/28/2020
ms.author: memildin
ms.openlocfilehash: 6d2127af2c2c9e04141551dae72f0177f495b165
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90904794"
---
# <a name="pricing-of-azure-security-center"></a>Preise für Azure Security Center
Azure Security Center ermöglicht eine einheitliche Sicherheitsverwaltung und erweiterten Schutz vor Bedrohungen für Workloads, die in Azure, lokal und in anderen Clouds ausgeführt werden. Security Center bietet Transparenz und Kontrolle für Hybridcloud-Workloads, aktive Abwehrmaßnahmen zur Verringerung Ihrer Anfälligkeit für Bedrohungen sowie eine intelligente Erkennung, damit Sie mit der schnellen Entwicklung bei Cyberangriffen Schritt halten können.


## <a name="free-option-vs-azure-defender-enabled"></a>Kostenlose Option und Azure Defender-Aktivierung

Security Center wird in zwei Modi angeboten:

- **Azure Defender AUS** (Kostenlos): Security Center ohne Azure Defender ist kostenlos für alle Ihre Azure-Abonnements aktiviert, sobald Sie das Azure Security Center-Dashboard zum ersten Mal im Azure-Portal aufrufen, oder wenn Sie es programmgesteuert über die API aktiviert haben. Die Verwendung dieses kostenlosen Tarifs umfasst eine Sicherheitsrichtlinie, eine kontinuierliche Bewertung der Sicherheit und direkt umsetzbare Sicherheitsempfehlungen für den Schutz Ihrer Azure-Ressourcen.

- **Azure Defender EIN**: Die Aktivierung von Azure Defender erweitert die Funktionen des kostenlosen Modus auf Workloads, die in privaten und anderen öffentlichen Clouds ausgeführt werden. Sie bietet eine einheitliche Sicherheitsverwaltung und Schutz vor Bedrohungen für Ihre Hybrid Cloud-Workloads. Einige der wichtigsten Features von Azure Defender:

    - **Hybridsicherheit:** Verschaffen Sie sich einen einheitlichen Überblick über die Sicherheit sämtlicher lokaler und cloudbasierter Workloads. Wenden Sie Sicherheitsrichtlinien an, und bewerten Sie kontinuierlich die Sicherheit Ihrer Hybridcloud-Workloads, um die Einhaltung von Sicherheitsstandards zu gewährleisten. Sammeln, durchsuchen und analysieren Sie Sicherheitsdaten aus zahlreichen Quellen (einschließlich Firewalls und Partnerlösungen).
    - **Bedrohungsschutzwarnungen:** Mit erweiterten Verhaltensanalysen und Microsoft Intelligent Security Graph sind Sie neuen Arten von Cyberangriffen immer einen Schritt voraus. Nutzen Sie integrierte Verhaltensanalysen und Machine Learning, um Angriffe und Zero-Day-Exploits zu erkennen. Überwachen Sie Netzwerke, Computer und Clouddienste auf neue Angriffe und auf Aktivitäten nach einer Sicherheitsverletzung. Optimieren Sie die Untersuchung mit interaktiven Tools und kontextbezogenen Informationen zu Bedrohungen.
    - **Überprüfung auf Sicherheitsrisiken für virtuelle Computer und Containerregistrierungen:** Stellen Sie problemlos einen Scanner auf allen virtuellen Computern bereit, der die branchenweit fortgeschrittenste Lösung zum Umgang mit Sicherheitsrisiken bietet. Sie können die Ergebnisse direkt in Security Center anzeigen, untersuchen und behandeln. 
    - **Zugriffs- und Anwendungskontrollen:** Blockieren Sie Schadsoftware und andere unerwünschte Anwendungen, indem Sie durch Machine Learning unterstützte Empfehlungen anwenden, die auf Ihre spezifischen Workloads abgestimmt sind, um Zulassungs- und Verweigerungslisten zu erstellen. Verringern Sie die Angriffsfläche im Netzwerk mit kontrolliertem Just-in-Time-Zugriff auf Verwaltungsports auf Azure Virtual Machines. Dadurch wird das Risiko von Brute-Force- und anderen Netzwerkangriffen drastisch reduziert.
    - **Containersicherheitsfeatures**: Profitieren Sie in Ihren Containerumgebungen von der Handhabung von Sicherheitsrisiken und des Schutzes vor Bedrohungen in Echtzeit. Wenn Sie **Azure Defender für Containerregistrierungen** aktivieren, kann es bis zu zwölf Stunden dauern, bis alle Features aktiviert sind. Die Gebühren basieren auf der Anzahl der eindeutigen Containerimages, die in Ihre verbundene Registrierung gepusht werden. Nachdem ein Image ein Mal gescannt wurde, wird es nicht mehr in Rechnung gestellt, es sei denn, es wird geändert und noch einmal gepusht. 

## <a name="try-azure-defender-free-for-30-days"></a>Kostenloses Testen von Azure Defender (30 Tage)

Azure Defender ist in den ersten 30 Tagen kostenlos. Nach Ablauf der 30 Tage wird Ihnen die Nutzung automatisch in Rechnung gestellt, sofern Sie sich dafür entscheiden, den Dienst weiterhin zu nutzen.

## <a name="enable-azure-defender"></a>Aktivieren von Azure Defender

Sie können ein gesamtes Azure-Abonnement mit Azure Defender schützen, und der Schutz wird von allen Ressourcen innerhalb des Abonnements geerbt.

So aktivieren Sie Azure Defender:

1. Wählen Sie im Hauptmenü von Security Center die Option **Preise und Einstellungen** aus.
1. Wählen Sie das Abonnement aus, das Sie aktualisieren möchten.
1. Wählen Sie zum Aktualisieren **Azure Defender ein** aus.
1. Klicken Sie auf **Speichern**.

Nachfolgend sehen Sie die Preisseite für ein Beispielabonnement. Sie werden feststellen, dass jeder Plan in Azure Defender separat abgerechnet wird und einzeln aktiviert oder deaktiviert werden kann.

:::image type="content" source="./media/security-center-pricing/pricing-tier-page.png" alt-text="Preisseite für Security Center im Portal":::

> [!NOTE]
> Wenn Sie alle Security Center-Features einschließlich des Bedrohungsschutzes aktivieren möchten, müssen Sie Azure Defender für das Abonnement mit den entsprechenden Workloads aktivieren. Durch die Aktivierung auf der Arbeitsbereichsebene werden Just-In-Time-VM-Zugriff, adaptive Anwendungssteuerungen und Netzwerkerkennungen für Azure-Ressourcen nicht aktiviert. 
>
> Sie können **Azure Defender für Storage-Konten** entweder auf Abonnement- oder auf Ressourcenebene aktivieren.
> Sie können **Azure Defender für SQL** entweder auf Abonnement- oder auf Ressourcenebene aktivieren.
> Sie können den Schutz vor Bedrohungen für **Azure Database for MariaDB/MySQL/PostgreSQL** nur auf der Ressourcenebene aktivieren.


## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel wurden die Preise für Security Center vorgestellt. Verwandte Informationen finden Sie hier:

- [Optimieren der Kosten für Azure-Workloads](https://azure.microsoft.com/blog/how-to-optimize-your-azure-workload-costs/)
- [Preisdetails in der von Ihnen gewählten Währung für Ihre Region](https://azure.microsoft.com/pricing/details/security-center/)
- Möglicherweise möchten Sie Ihre Kosten verwalten und den Umfang der für eine Lösung gesammelten Daten begrenzen, indem Sie sie auf einen bestimmten Satz von Agents beschränken. Mit der [Zielgruppenadressierung für Lösungen](../operations-management-suite/operations-management-suite-solution-targeting.md) können Sie einen Bereich auf die Lösung anwenden und eine Teilmenge von Computern im Arbeitsbereich als Ziel angeben. Bei Verwendung der Zielgruppenadressierung für Lösungen führt Security Center den Arbeitsbereich als Arbeitsbereich ohne Lösung auf.