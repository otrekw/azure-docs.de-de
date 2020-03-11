---
title: Preise für Azure Security Center-Dienstebenen
description: 'Azure Security Center wird in zwei Tarifen angeboten: Free und Standard. Auf dieser Seite erfahren Sie, wie Sie von Free auf Standard upgraden.'
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
ms.date: 12/13/2019
ms.author: memildin
ms.openlocfilehash: 60a88e667918533f2c507846fa75b0e036ba5262
ms.sourcegitcommit: 3c925b84b5144f3be0a9cd3256d0886df9fa9dc0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/28/2020
ms.locfileid: "77921282"
---
# <a name="upgrade-to-standard-tier-for-enhanced-security"></a>Upgrade auf den Standard-Tarif für erhöhte Sicherheit
Azure Security Center ermöglicht eine einheitliche Sicherheitsverwaltung und erweiterten Schutz vor Bedrohungen für Workloads, die in Azure, lokal und in anderen Clouds ausgeführt werden. Security Center bietet Transparenz und Kontrolle für Hybridcloud-Workloads, aktive Abwehrmaßnahmen zur Verringerung Ihrer Anfälligkeit für Bedrohungen sowie eine intelligente Erkennung, damit Sie mit der schnellen Entwicklung bei Cyberangriffen Schritt halten können.

## <a name="pricing-tiers"></a>Tarife
Security Center wird in zwei Tarifen angeboten:

- Der **Free** -Tarif ist für alle Ihre Azure-Abonnements aktiviert, sobald Sie das Azure Security Center-Dashboard zum ersten Mal im Azure-Portal aufrufen, oder wenn Sie es programmgesteuert über die API aktiviert haben. Der Tarif „Free“ umfasst eine Sicherheitsrichtlinie, eine kontinuierliche Bewertung der Sicherheit und umsetzbare Sicherheitsempfehlungen für den Schutz Ihrer Azure-Ressourcen.
- Der Tarif **Standard** erweitert die Funktionen des Free-Tarifs auf Workloads, die in privaten und anderen öffentlichen Clouds ausgeführt werden. Er bietet eine einheitliche Sicherheitsverwaltung und Schutz vor Bedrohungen für Ihre Hybridcloud-Workloads. Im Tarif „Standard“ stehen zudem Funktionen für den Schutz vor Bedrohungen zur Verfügung. Diese verwenden u. a. integrierte Verhaltensanalysen und maschinelles Lernen, um Angriffe und Zero-Day-Exploits zu erkennen, sowie Zugriffs- und Anwendungskontrollen zur Verringerung der Angriffsfläche für Netzwerkangriffe und Schadsoftware. Außerdem wird im Tarif „Standard“ die Überprüfung auf Sicherheitsrisiken für Ihre virtuellen Computer durchgeführt. Sie können den Tarif „Standard“ kostenlos testen. Security Center Standard unterstützt Azure-Ressourcen wie VMs, Skalierungsgruppen für virtuelle Computer, App Service, SQL Server-Instanzen und Speicherkonten. Wenn Sie über Azure Security Center Standard verfügen, sind Sie nicht mehr auf Support auf Basis des Ressourcentyps angewiesen. 

Die meisten der Sicherheitsbewertungen im Tarif „Free“ für virtuelle Computer erfordern, ebenso wie viele der Sicherheitswarnungen im Standard-Tarif, die Installation der Funktion „Microsoft Monitoring Agent (MMA)“. Sie können die automatische Bereitstellung im Security Center aktivieren, um den Agent automatisch für Ihre Azure-VMs bereitzustellen.

## <a name="try-standard-tier-free-for-30-days"></a>Tarif „Standard“ kostenlos 30 Tage lang testen
Der Tarif „Standard“ ist in den ersten 30 Tagen kostenlos. Wenn Sie den Dienst über diese 30 Tage hinaus nutzen, wird er automatisch in Rechnung gestellt.

Sie können ein Upgrade für ein gesamtes Azure-Abonnement auf den Tarif „Standard“ durchführen, da diese Änderung von allen Ressourcen im Abonnement übernommen wird.

So erhalten Sie den Tarif „Standard“

1. Klicken Sie im Hauptmenü von **Security Center** auf **Preise & Einstellungen**.
2. Wählen Sie das Abonnement aus, das Sie auf den Tarif „Standard“ umstellen möchten.
3. Wählen Sie **Tarif**aus.
4. Wählen Sie **Standard** aus, um das Upgrade durchzuführen.
5. Klicken Sie auf **Speichern**.

[![Security Center-Tarife](media/security-center-pricing/pricing-tier-page.png)](media/security-center-pricing/pricing-tier-page.png#lightbox)

> [!NOTE]
> Wenn Sie alle Security Center-Features aktivieren möchten, müssen Sie den Tarif „Standard“ dem Abonnement mit den entsprechenden virtuellen Computern zuweisen. Durch das Konfigurieren von Preisen für einen Arbeitsbereich werden Just-in-Time-VM-Zugriff, adaptive Anwendungssteuerungen und Netzwerkerkennungen für Azure-Ressourcen nicht aktiviert.
>

## <a name="why-upgrade-to-standard"></a>Gründe für ein Upgrade auf den Tarif „Standard“
Security Center bietet mehr Sicherheit und besseren Schutz vor Bedrohungen für Ihre Hybridcloud-Workloads:

- **Hybridsicherheit:** Verschaffen Sie sich einen einheitlichen Überblick über die Sicherheit sämtlicher lokaler und cloudbasierter Workloads. Wenden Sie Sicherheitsrichtlinien an, und bewerten Sie kontinuierlich die Sicherheit Ihrer Hybridcloud-Workloads, um die Einhaltung von Sicherheitsstandards zu gewährleisten. Sammeln, durchsuchen und analysieren Sie Sicherheitsdaten aus zahlreichen Quellen (einschließlich Firewalls und Partnerlösungen).
- **Sicherheitswarnungen**: Verwenden Sie erweiterte Analysefunktionen und Microsoft Intelligent Security Graph, um neuen Arten von Cyberangriffen immer einen Schritt voraus zu sein. Nutzen Sie integrierte Verhaltensanalysen und Machine Learning, um Angriffe und Zero-Day-Exploits zu erkennen. Überwachen Sie Netzwerke, Computer und Clouddienste auf neue Angriffe und auf Aktivitäten nach einer Sicherheitsverletzung. Optimieren Sie die Untersuchung mit interaktiven Tools und kontextbezogenen Informationen zu Bedrohungen.
- **Überprüfung auf Sicherheitsrisiken für virtuelle Computer**: Stellen Sie problemlos einen Scanner auf allen virtuellen Computern bereit, der die branchenweit fortgeschrittenste Lösung zum Umgang mit Sicherheitsrisiken bietet. Sie können die Ergebnisse direkt in Security Center anzeigen, untersuchen und behandeln. 
- **Zugriffs- und Anwendungskontrollen**: Blockieren Sie Schadsoftware und andere unerwünschte Anwendungen mithilfe von Whitelist-Empfehlungen, die auf Ihre spezifischen Workloads abgestimmt sind und durch Machine Learning unterstützt werden. Verringern Sie die Angriffsfläche im Netzwerk mit kontrolliertem Just-in-Time-Zugriff auf Verwaltungsports auf Azure Virtual Machines. Dadurch wird das Risiko von Brute-Force- und anderen Netzwerkangriffen drastisch reduziert.
- **Containersicherheitsfeatures**: Profitieren Sie in Ihren Containerumgebungen von der Handhabung von Sicherheitsrisiken und des Schutzes vor Bedrohungen in Echtzeit. Wenn Sie die Ressource „Containerregistrierungen“ aktivieren, kann es bis zu 12 Stunden dauern, bis alle Features aktiviert sind.


## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel wurden die Preise für Security Center vorgestellt. Weitere Informationen zur erweiterten Sicherheit und Bedrohungserkennung des Standard-Tarifs finden Sie unter den folgenden Links:

- [Bedrohungsschutz in Azure Security Center](threat-protection.md)
- [JIT-VM-Zugriffssteuerung](security-center-just-in-time.md)
- [Übersicht über die Containersicherheit](container-security.md)
- [Preisdetails in der von Ihnen gewählten Währung für Ihre Region](https://azure.microsoft.com/pricing/details/security-center/)