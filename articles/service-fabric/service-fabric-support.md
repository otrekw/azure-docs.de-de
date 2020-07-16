---
title: Informationen zu den Azure Service Fabric-Supportoptionen
description: Unterstützte Azure Service Fabric-Clusterversionen und Links zu Dateisupporttickets
author: pkcsf
ms.topic: troubleshooting
ms.date: 8/24/2018
ms.author: pkc
ms.openlocfilehash: ae49a59c2629d9f9461d298ada555d314c0c9f22
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2020
ms.locfileid: "86256967"
---
# <a name="azure-service-fabric-support-options"></a>Supportoptionen für Azure Service Fabric

Um die entsprechende Unterstützung für die Service Fabric-Cluster bereitzustellen, in denen Sie Ihre Anwendungsworkloads ausführen, haben wir verschiedene Optionen für Sie eingerichtet. Sie können je nach benötigter Supportstufe und Schweregrad des Problems die passenden Optionen auswählen. 

## <a name="report-production-issues-or-request-paid-support-for-azure"></a>Melden von Produktionsproblemen oder Anfordern von kostenpflichtigem Support für Azure

Um Probleme mit Ihrem in Azure bereitgestellten Service Fabric-Cluster zu melden, öffnen Sie [im Azure-Portal](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview) oder [Microsoft-Support-Portal](https://support.microsoft.com/oas/default.aspx?prid=16146) ein Supportticket.

Weitere Informationen:
 
- [Support von Microsoft für Azure](https://azure.microsoft.com/support/plans/?b=16.44)
- [Microsoft Premier Support](https://support.microsoft.com/en-us/premier)

> [!Note]
> Auf Clustern, die auf einer Bronze-Zuverlässigkeitsebene oder in einem Cluster mit nur einem Knoten ausgeführt werden, können Sie nur Testworkloads ausführen. Wenn Sie Probleme mit einem Cluster mit Bronze-Zuverlässigkeit oder einem Cluster mit nur einem Knoten haben, wird Ihnen das Microsoft-Supportteam bei der Behebung des Problems helfen, aber keine Analyse der Grundursache durchführen. Weitere Informationen finden Sie unter [Die Zuverlässigkeitsmerkmale des Clusters](./service-fabric-cluster-capacity.md#reliability-characteristics-of-the-cluster).
>
> Weitere Informationen darüber, was für einen produktionsbereiten Cluster erforderlich ist, finden Sie in der Checkliste [Produktionsbereitschaft](./service-fabric-production-readiness-checklist.md).

<a id="getlivesitesupportonprem"></a>

## <a name="report-production-issues-or-request-paid-support-for-standalone-service-fabric-clusters"></a>Melden von Produktionsproblemen oder Anfordern von kostenpflichtigem Support für eigenständige Service Fabric-Cluster

Um Probleme mit Ihrem in lokalen oder anderen Clouds bereitgestellten Service Fabric-Cluster zu melden, erstellen Sie im [Microsoft-Support-Portal](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview) ein Ticket für Professional Support.

Weitere Informationen:

- [Professional Support von Microsoft für lokale Bereitstellungen](https://support.microsoft.com/en-us/gp/offerprophone?wa=wsignin1.0)
- [Microsoft Premier Support](https://support.microsoft.com/en-us/premier)

## <a name="report-azure-service-fabric-issues"></a>Melden von Azure Service Fabric-Problemen

Wir haben ein GitHub-Repository zum Melden von Service Fabric-Problemen eingerichtet.  Zudem überwachen wir aktiv die folgenden Foren.

### <a name="github-repo"></a>GitHub-Repository 

Melden Sie Azure Service Fabric-Probleme im [Git-Repository für Service Fabric-Probleme](https://github.com/Azure/service-fabric-issues). Dieses Repository dient zur Meldung und Nachverfolgung von Problemen mit Azure Service Fabric sowie zum Senden kleiner Funktionsanforderungen. **Verwenden Sie das Repository nicht zum Melden von Problemen mit der Livewebsite**.

### <a name="stackoverflow-and-msdn-forums"></a>StackOverflow und MSDN-Foren

Das [Service Fabric-Tag auf StackOverflow][stackoverflow] und das [Service Fabric-Forum auf MSDN][msdn-forum] eignen sich für Fragen zur Funktionsweise der Plattform und zur Durchführung bestimmter Aufgaben.

### <a name="azure-feedback-forum"></a>Azure-Feedbackforum

Das [Azure-Feedbackforum für Service Fabric][uservoice-forum] ist die beste Anlaufstelle, wenn Sie umfangreiche Funktionsvorschläge für das Produkt einreichen möchten, da die häufigsten Anforderungen im Rahmen unserer mittel- und langfristigen Planung geprüft werden. Wir empfehlen Ihnen, in der Community Unterstützung für Ihre Vorschläge zu gewinnen.

## <a name="service-fabric-preview-versions---unsupported-for-production-use"></a>Service Fabric-Vorschauversionen – nicht für den Einsatz in Produktionsumgebungen unterstützt

Von Zeit zu Zeit veröffentlichen wir Versionen, die wichtige Features besitzen, die als Vorschauversionen veröffentlicht werden und für die wir gerne Feedback möchten. Diese Vorschauversionen sollten nur zu Testzwecken verwendet werden. Ihr Produktionscluster muss immer als unterstütze stabile Service Fabric-Version ausgeführt werden. Eine Vorschauversion beginnt immer mit einer großen und kleinen Versionsnummer von 255. Wenn Sie z.B. eine Service Fabric-Version 255.255.5703.949 vor sich haben, befindet sich diese Produktversion in er Vorschau und darf nur in Testclustern verwendet werden. Diese Produktversionen werden auch im [Service Fabric-Teamblog](https://techcommunity.microsoft.com/t5/azure-service-fabric/bg-p/Service-Fabric) bekannt gegeben und besitzen Details zu den enthaltenen Features.
Es existiert keine kostenpflichtige Supportoption für diese Vorschauversionen. Verwenden Sie eine der unter [Melden von Azure Service Fabric-Problemen](#report-azure-service-fabric-issues) aufgeführten Optionen, um Fragen zu stellen oder Feedback zu geben.

## <a name="next-steps"></a>Nächste Schritte

[Unterstützte Service Fabric-Versionen](service-fabric-versions.md)

<!--references-->
[Microsoft Q&A question page]: /answers/topics/azure-service-fabric.html
[stackoverflow]: https://stackoverflow.com/questions/tagged/azure-service-fabric
[uservoice-forum]: https://feedback.azure.com/forums/293901-service-fabric
[acom-docs]: https://aka.ms/servicefabricdocs
[sample-repos]: https://aka.ms/servicefabricsamples
[msdn-forum]: https://social.msdn.microsoft.com/forums/azure/en-US/home?category=windowsazureplatform
