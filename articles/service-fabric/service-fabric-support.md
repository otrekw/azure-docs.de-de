---
title: Informationen zu den Azure Service Fabric-Supportoptionen
description: Unterstützte Azure Service Fabric-Clusterversionen und Links zu Dateisupporttickets
author: pkcsf
ms.topic: troubleshooting
ms.date: 8/24/2018
ms.author: pkc
ms.openlocfilehash: 977cd79de629670cef526f072340f8897fa6446e
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "92316494"
---
# <a name="azure-service-fabric-support-options"></a>Supportoptionen für Azure Service Fabric

Wir haben eine Reihe von Supportanfrageoptionen erstellt, um die Anforderungen für die Verwaltung Ihrer Service Fabric-Cluster und Anwendungsworkloads zu erfüllen. Abhängig von der Dringlichkeit der erforderlichen Unterstützung und dem Schweregrad des Problems können Sie die für Sie geeignete Option auswählen.

## <a name="report-production-issues-or-request-paid-support-for-azure"></a>Melden von Produktionsproblemen oder Anfordern von kostenpflichtigem Support für Azure

Um Probleme mit Ihrem in Azure ausgeführten Service Fabric-Clusters zu melden, öffnen Sie [im Azure-Portal](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview) oder [Microsoft-Supportportal](https://support.microsoft.com/oas/default.aspx?prid=16146) ein Supportticket.

Weitere Informationen:
 
- [Support von Microsoft für Azure](https://azure.microsoft.com/support/plans/?b=16.44)
- [Microsoft Premier Support](https://support.microsoft.com/en-us/premier)

> [!Note]
> Auf Clustern, die auf einer Bronze-Zuverlässigkeitsebene oder in einem Cluster mit nur einem Knoten ausgeführt werden, können Sie nur Testworkloads ausführen. Wenn Sie Probleme mit einem Cluster mit Bronze-Zuverlässigkeit oder einem Cluster mit nur einem Knoten haben, wird Ihnen das Microsoft-Supportteam bei der Behebung des Problems helfen, aber keine Analyse der Grundursache durchführen. Weitere Informationen finden Sie unter [Zuverlässigkeitsmerkmale des Clusters](./service-fabric-cluster-capacity.md#reliability-characteristics-of-the-cluster).
>
> Weitere Informationen darüber, was für einen produktionsbereiten Cluster erforderlich ist, finden Sie in der Checkliste [Produktionsbereitschaft](./service-fabric-production-readiness-checklist.md).

<a id="getlivesitesupportonprem"></a>

## <a name="report-production-issues-or-request-paid-support-for-standalone-service-fabric-clusters"></a>Melden von Produktionsproblemen oder Anfordern von kostenpflichtigem Support für eigenständige Service Fabric-Cluster

Um Probleme mit Ihrem in lokalen oder anderen Clouds bereitgestellten Service Fabric-Cluster zu melden, erstellen Sie im [Microsoft-Supportportal](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview) ein Ticket für Professional Support.

Weitere Informationen:

- [Professional Support von Microsoft für lokale Bereitstellungen](https://support.microsoft.com/en-us/gp/offerprophone?wa=wsignin1.0)
- [Microsoft Premier Support](https://support.microsoft.com/en-us/premier)

## <a name="report-azure-service-fabric-issues"></a>Melden von Azure Service Fabric-Problemen

Wir haben ein GitHub-Repository zum Melden von Service Fabric-Problemen eingerichtet.  Zudem überwachen wir aktiv die folgenden Foren.

### <a name="github-repo"></a>GitHub-Repository 

Melden Sie Azure Service Fabric-Probleme in [Service Fabric GitHub](https://github.com/microsoft/service-fabric/issues). Dieses Repository dient zur Meldung und Nachverfolgung von Problemen sowie zum Senden kleiner Funktionsanforderungen, die sich auf Azure Service Fabric beziehen. **Verwenden Sie dieses Medium nicht zum Melden von Problemen mit der Livewebsite**.

### <a name="stackoverflow-and-msdn-forums"></a>StackOverflow und MSDN-Foren

Das [Service Fabric-Tag auf Stack Overflow][stackoverflow] und das [Service Fabric-Forum auf MSDN][msdn-forum] eignen sich für allgemeine Fragen zur Funktionsweise der Plattform und zur Durchführung bestimmter Aufgaben.

### <a name="azure-feedback-forum"></a>Azure-Feedbackforum

Das [Azure-Feedbackforum für Service Fabric][uservoice-forum] ist der beste Ort für die Übermittlung wichtiger Ideen für Produktfunktionen. Wir überprüfen die beliebtesten Anforderungen und berücksichtigen diese für unsere mittel- bis langfristige Planung. Wir empfehlen Ihnen, in der Community Unterstützung für Ihre Vorschläge zu gewinnen.

## <a name="service-fabric-preview-versions---unsupported-for-production-use"></a>Service Fabric-Vorschauversionen – nicht für den Einsatz in Produktionsumgebungen unterstützt

Gelegentlich werden spezielle Vorschauversionen mit erheblichen Funktionsänderungen bereitgestellt, für die wir frühes Feedback untersuchen möchten. Sie sollten Vorschauversionen nur in isolierten Testumgebungen verwenden, die keine Produktionsworkloads bereitstellen. Ihr Produktionscluster muss immer als unterstütze stabile Service Fabric-Version ausgeführt werden. Wir bieten keine kostenpflichtige Supportoption für diese Vorschauversionen.

Eine Vorschauversion beginnt immer mit einer großen und kleinen Versionsnummer von 255. Wenn Sie z. B. eine Service Fabric-Version 255.255.5703.949 vor sich haben, befindet sich diese Version in der Vorschau und darf nur in Testclustern verwendet werden. Diese Produktversionen werden auch im [Service Fabric-Teamblog](https://techcommunity.microsoft.com/t5/azure-service-fabric/bg-p/Service-Fabric) bekannt gegeben und besitzen Details zu den enthaltenen Features. Verwenden Sie eine der unter [Melden von Azure Service Fabric-Problemen](#report-azure-service-fabric-issues) aufgeführten Optionen, um Fragen zu stellen oder Feedback zu geben.

## <a name="next-steps"></a>Nächste Schritte

[Unterstützte Service Fabric-Versionen](service-fabric-versions.md)

<!--references-->
[Microsoft Q&A question page]: /answers/topics/azure-service-fabric.html
[stackoverflow]: https://stackoverflow.com/questions/tagged/azure-service-fabric
[uservoice-forum]: https://feedback.azure.com/forums/293901-service-fabric
[acom-docs]: ./index.yml
[sample-repos]: /samples/browse/?products=azure
[msdn-forum]: https://social.msdn.microsoft.com/forums/azure/en-US/home?category=windowsazureplatform