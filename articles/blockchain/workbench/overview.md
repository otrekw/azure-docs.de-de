---
title: Übersicht über die Vorschauversion von Azure Blockchain Workbench
description: Enthält eine Übersicht über die Vorschauversion von Azure Blockchain Workbench und die zugehörigen Funktionen.
ms.date: 05/22/2020
ms.topic: overview
ms.reviewer: brendal
ms.openlocfilehash: fbd6be3907dbd10b003d065dfb14031a0e378478
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2020
ms.locfileid: "87003145"
---
# <a name="what-is-azure-blockchain-workbench"></a>Was ist Azure Blockchain Workbench?

Azure Blockchain Workbench (Vorschauversion) ist eine Sammlung mit Azure-Diensten und -Funktionen zum Erstellen und Bereitstellen von Blockchain-Anwendungen, mit denen Geschäftsprozesse und Daten mit anderen Organisationen gemeinsam genutzt werden können. Mit Azure Blockchain Workbench wird das Infrastrukturgerüst für die Erstellung von Blockchain-Anwendungen bereitgestellt, sodass sich Entwickler auf die Erstellung von Geschäftslogik und Smart Contracts konzentrieren können. Außerdem wird die Erstellung von Blockchain-Anwendungen vereinfacht, indem mehrere Azure-Dienste und -Funktionen für die Automatisierung von häufigen Entwicklungsaufgaben integriert werden.

[!INCLUDE [Preview note](./includes/preview.md)]

## <a name="create-blockchain-applications"></a>Erstellen von Blockchain-Anwendungen

Mit Blockchain Workbench können Sie Blockchainanwendungen durch Konfiguration und Schreiben von intelligentem Vertragscode definieren. Sie können schnell in die Entwicklung von Blockchain-Anwendungen einsteigen und sich auf das Definieren Ihres Smart Contracts und das Schreiben von Geschäftslogik konzentrieren, anstatt das Gerüst erstellen und unterstützende Dienste einrichten zu müssen.

## <a name="manage-applications-and-users"></a>Verwalten von Anwendungen und Benutzern

Azure Blockchain Workbench stellt eine Webanwendung und REST-APIs zur Verwaltung von Blockchain-Anwendungen und -Benutzern bereit. Blockchain Workbench-Administratoren können den Anwendungszugriff verwalten und Ihre Benutzer den Anwendungsrollen zuweisen. Azure AD-Benutzer werden den Mitgliedern in der Anwendung automatisch zugeordnet.

## <a name="integrate-blockchain-with-applications"></a>Integrieren von Blockchain in Anwendungen

Sie können die Blockchain Workbench-REST-APIs und nachrichtenbasierten APIs für die Integration in vorhandene Systeme verwenden. Die APIs stellen eine Schnittstelle für das Austauschen bzw. Nutzen von mehreren Distributed Ledger-Technologien sowie Speicher- und Datenbankangeboten bereit.

Blockchain Workbench kann Nachrichten transformieren, die an die entsprechende nachrichtenbasierte API gesendet werden, um Transaktionen in einem Format zu erstellen, das von der nativen API der jeweiligen Blockchain-Komponente erwartet wird.  Workbench kann Transaktionen signieren und an die entsprechende Blockchain-Komponente weiterleiten. 

Workbench stellt Ereignisse automatisch für Service Bus und Event Grid bereit, damit Nachrichten an nachgeschaltete Consumer gesendet werden können. Entwickler können eine Integration in beide Nachrichtensysteme durchführen, um Transaktionen zu ermöglichen und die Ergebnisse anzuzeigen.

## <a name="deploy-a-blockchain-network"></a>Bereitstellen eines Blockchain-Netzwerks

Azure Blockchain Workbench vereinfacht das Setup eines Blockchain-Netzwerks für Konsortien als vorkonfigurierte Lösung mit einer Azure Resource Manager-Lösungsvorlage. Die Vorlage ermöglicht einen vereinfachten Bereitstellungsvorgang, bei dem alle Komponenten bereitgestellt werden, die für die Konsortiumausführung benötigt werden. Derzeit wird Ethereum von Blockchain Workbench unterstützt.

## <a name="use-active-directory"></a>Active Directory verwenden

Bei vorhandenen Blockchain-Protokollen werden Blockchain-Identitäten als Adresse im Netzwerk dargestellt. Azure Blockchain Workbench entfernt die Blockchain-Identität per Abstrahierung, indem sie einer Active Directory-Identität zugeordnet wird. So wird das Erstellen von Unternehmensanwendungen mit Active Directory-Identitäten vereinfacht.

## <a name="synchronize-on-chain-data-with-off-chain-storage"></a>Synchronisieren von On-Chain-Daten mit Off-Chain-Speicher

Azure Blockchain Workbench vereinfacht das Analysieren von Blockchain-Ereignissen und -Daten, indem Daten der Blockchain-Komponente automatisch mit dem Off-Chain-Speicher synchronisiert werden. Anstatt Daten direkt aus der Blockchain-Komponente zu extrahieren, können Sie Off-Chain-Datenbanksysteme abfragen, z.B. SQL Server. Für Endbenutzer, die Aufgaben zur Datenanalyse durchführen, ist kein Blockchain-Wissen erforderlich.

## <a name="support-and-feedback"></a>Support und Feedback

Neues zu Azure Blockchain: Mit dem [Azure Blockchain-Blog](https://azure.microsoft.com/blog/topics/blockchain/) bleiben Sie in Bezug auf Blockchain-Dienstangebote und Informationen vom Azure Blockchain-Technikteam immer auf dem Laufenden.

Über das [Azure-Feedbackforum für Blockchain](https://aka.ms/blockchainuservoice) können Sie Produktfeedback senden, neue Features anfordern oder über Ideen abstimmen.

### <a name="community-support"></a>Communityunterstützung

Diskutieren Sie mit Microsoft-Technikern und Azure Blockchain-Communityexperten.

* [Frageseite von Microsoft Q&A (Fragen und Antworten) für Azure Blockchain Workbench](/answers/topics/azure-blockchain-workbench.html)
* [Microsoft Tech Community](https://techcommunity.microsoft.com/t5/Blockchain/bd-p/AzureBlockchain)
* [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-blockchain-workbench)

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Azure Blockchain Workbench-Architektur](architecture.md)
