---
title: Funktionsweise von Routing mit Azure Dev Spaces
services: azure-dev-spaces
ms.date: 03/24/2020
ms.topic: conceptual
description: Beschreibt die Prozesse, auf denen Azure Dev Spaces basiert, und die Funktionsweise von Routing
keywords: Azure Dev Spaces, Dev Spaces, Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, Container
ms.openlocfilehash: 8677284d61a0a08a3262c26d6307399922dab4be
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/20/2020
ms.locfileid: "86512468"
---
# <a name="how-routing-works-with-azure-dev-spaces"></a>Funktionsweise von Routing mit Azure Dev Spaces

Azure Dev Spaces bietet Ihnen mehrere Möglichkeiten, Kubernetes-Anwendungen schnell zu durchlaufen und zu debuggen und mit Ihrem Team in einem Azure Kubernetes Service-Cluster (AKS) zusammenzuarbeiten. Wenn das Projekt in einem Entwicklungsbereich ausgeführt wird, bietet Azure Dev Spaces zusätzliche Netzwerk- und Routingfunktionen für Ihr Projekt.

Dieser Artikel beschreibt, wie Routing mit Dev Spaces funktioniert.

## <a name="how-routing-works"></a>Funktionsweise des Routings (Weiterleitung)

Ein Entwicklungsbereich (Dev Spac) wird auf AKS erstellt, und für ihn werden die gleichen [Netzwerkkonzepte](../aks/concepts-network.md) verwendet. Azure Dev Spaces hat außerdem einen zentralisierten *ingressmanager*-Dienst und stellt dem AKS-Cluster seinen eigenen Eingangscontroller bereit. Der *ingressmanager*-Dienst überwacht AKS-Cluster mit Entwicklungsbereichen und erweitert den Azure Dev Spaces-Eingangscontroller im Cluster durch Eingangsobjekte zum Routing an Anwendungs-Pods. Der devspaces-proxy-Container in jedem Pod fügt einem Entwicklungsbereich auf Basis der URL einen `azds-route-as`-HTTP-Header für HTTP-Datenverkehr hinzu. Zum Beispiel würde eine Anforderung an die URL *http://azureuser.s.default.serviceA.fedcba09...azds.io* zu einem HTTP-Header mit `azds-route-as: azureuser` führen. Der devspaces-proxy-Container fügt keinen `azds-route-as`-Header hinzu, wenn bereits einer vorhanden ist.

Wird eine HTTP-Anforderung von außerhalb des Clusters an einen Dienst gesendet, gelangt die Anforderung an den Eingangscontroller. Der Eingangscontroller leitet die Anforderung anhand seiner Eingangsobjekte und -regeln direkt an den entsprechenden Pod weiter. Der devspaces-proxy-Container im Pod empfängt die Anforderung, fügt den `azds-route-as`-Header anhand der URL hinzu und leitet die Anforderung dann an den Anwendungscontainer weiter.

Wird eine HTTP-Anforderung von einem anderen Dienst im Cluster an einen Dienst gesendet, durchläuft die Anforderung zunächst den devspaces-proxy-Container des aufrufenden Diensts. Der devspaces-proxy-Container analysiert die HTTP-Anforderung und überprüft den `azds-route-as`-Header. Ausgehend vom Header sucht der devspaces-proxy-Container nach der IP-Adresse des Diensts, der dem Headerwert zugeordnet ist. Wird eine IP-Adresse gefunden, leitet der devspaces-proxy-Container die Anforderung an diese IP-Adresse um. Wird keine IP-Adresse gefunden, leitet der devspaces-proxy-Container die Anforderung an den übergeordneten Anwendungscontainer weiter.

Angenommen, die Anwendungen *serviceA* und *serviceB* werden in einem übergeordneten Entwicklungsbereich namens *default* bereitgestellt. *serviceA* setzt auf *serviceB* auf und sendet HTTP-Aufrufe an ihn. Ein Azure-Benutzer erstellt auf Basis des *default*-Bereichs einen untergeordneten Entwicklungsbereich namens *azureuser*. Außerdem stellt der Azure-Benutzer seine eigene Version von *serviceA* in seinem untergeordneten Bereich bereit. Wenn eine Anforderung an *http://azureuser.s.default.serviceA.fedcba09...azds.io* gesendet wird, passiert Folgendes:

![Azure Dev Spaces-Routing](media/how-dev-spaces-works/routing.svg)

1. Der Eingangscontroller (Ingress-Controller) sucht nach der IP-Adresse für den Pod, der der URL, die *serviceA.azureuser* lautet, zugeordnet ist.
1. Der Eingangscontroller findet die IP-Adresse für den Pod im Entwicklungsbereich des Azure-Benutzers und leitet die Anforderung an den *serviceA.azureuser*-Pod weiter.
1. Der devspaces-proxy-Container im *serviceA.azureuser*-Pod empfängt die Anforderung und fügt `azds-route-as: azureuser` als HTTP-Header hinzu.
1. Der devspaces-proxy-Container im *serviceA.azureuser*-Pod leitet die Anforderung an den *serviceA*-Anwendungscontainer im *serviceA.azureuser*-Pod weiter.
1. Die *serviceA*-Anwendung im *serviceA.azureuser*-Pod sendet einen Aufruf an *serviceB*. Die *serviceA*-Anwendung enthält auch Code zum Beibehalten des vorhandenen `azds-route-as`-Headers, der in diesem Fall `azds-route-as: azureuser` lautet.
1. Der devspaces-proxy-Container im *serviceA.azureuser*-Pod empfängt die Anforderung und sucht nach der IP-Adresse von *serviceB* anhand des Werts des `azds-route-as`-Headers.
1. Der devspaces-proxy-Container im *serviceA.azureuser*-Pod findet keine IP-Adresse für *serviceB.azureuser*.
1. Der devspaces-proxy-Container im *serviceA.azureuser*-Pod sucht nach der IP-Adresse für *serviceB* im übergeordneten Bereich, der *serviceB.default* entspricht.
1. Der devspaces-proxy-Container im *serviceA.azureuser*-Pod findet die IP-Adresse für *serviceB.default* und leitet die Anforderung an den *serviceB.default*-Pod weiter.
1. Der devspaces-proxy-Container im *serviceB.default*-Pod empfängt die Anforderung und leitet diese an den *serviceB*-Anwendungscontainer im *serviceB.default*-Pod weiter.
1. Die *serviceB*-Anwendung im *serviceB.default*-Pod gibt eine Antwort an den *serviceA.azureuser*-Pod zurück.
1. Der devspaces-proxy-Container im *serviceA.azureuser*-Pod empfängt die Antwort und leitet sie an den *serviceA*-Anwendungscontainer im *serviceA.azureuser*-Pod weiter.
1. Die *serviceA*-Anwendung empfängt die Antwort und gibt dann ihre eigene Antwort zurück.
1. Der devspaces-proxy-Container im *serviceA.azureuser*-Pod empfängt die Antwort vom *serviceA*-Anwendungscontainer und leitet sie an den ursprünglichen Aufrufer außerhalb des Clusters weiter.

Jeglicher sonstiger TCP-Datenverkehr, der kein HTTP-Datenverkehr ist, durchläuft den Eingangscontroller und den devspaces-proxy-Container ungeändert.

## <a name="sharing-a-dev-space"></a>Freigeben eines Entwicklungsbereichs (Dev Space)

Wenn Sie in einem Team arbeiten, können Sie [einen Entwicklungsbereich für das gesamte Team freigeben](how-to/share-dev-spaces.md) und abgeleitete Entwicklungsbereiche erstellen. Ein Entwicklungsbereich kann von jeder Person verwendet werden, die Zugriff für Mitwirkende auf die Ressourcengruppe des Entwicklungsbereichs hat.

Sie können außerdem einen neuen Entwicklungsbereich erstellen, der aus einem anderen Entwicklungsbereich abgeleitet ist. Wenn Sie einen abgeleiteten Entwicklungsbereich erstellen, wird dem Namespace dieses abgeleiteten Entwicklungsbereichs die Bezeichnung *azds.io/parent-space=NAME-ÜBERGEORDNETER-BEREICH* hinzugefügt. Darüber hinaus werden alle Anwendungen aus dem übergeordneten Entwicklungsbereich für den abgeleiteten Entwicklungsbereich freigegeben. Wenn Sie eine aktualisierte Version einer Anwendung im abgeleiteten Entwicklungsbereich bereitstellen, ist diese Version nur im abgeleiteten Entwicklungsbereich vorhanden, und der übergeordnete Entwicklungsbereich ist nicht betroffen. Sie können mit bis zu drei Ebenen abgeleiteter Entwicklungsbereiche oder *über-übergeordneter* Bereiche arbeiten.

In einem abgeleiteten Entwicklungsbereich werden Anforderungen zwischen dessen eigenen Anwendungen und den Anwendungen, die aus seinem übergeordneten Bereich freigegeben sind, auf intelligente Weise weitergeleitet. Das Weiterleiten (Routing) funktioniert, indem versucht wird, eine Anforderung an eine Anwendung im abgeleiteten Entwicklungsbereich weiterzuleiten, und ggf. auf die freigegebene Anwendung aus dem übergeordneten Entwicklungsbereich zurückgegriffen wird. Für das Routing wird auf die freigegebene Anwendung im über-übergeordneten Bereich zurückgegriffen, wenn die Anwendung im übergeordneten Bereich nicht vorhanden ist.

Beispiel:
* Im Entwicklungsbereich *default* gibt es die Anwendungen *serviceA* und *serviceB*.
* Der Entwicklungsbereich *azureuser* ist aus *default* abgeleitet.
* Eine aktualisierte Version von *serviceA* wird in *azureuser* bereitgestellt.

Wird *azureuser* verwendet, werden alle Anforderungen an *serviceA* an die aktualisierte Version in *azureuser* weitergeleitet. Für eine Anforderung an *serviceB* wird zuerst versucht, sie an die *azureuser*-Version von *serviceB* weiterzuleiten. Da diese nicht vorhanden ist, wird die Anforderung an die *default*-Version von *serviceB* weitergeleitet. Wurde die *azureuser*-Version von *serviceA* entfernt, erfolgt für alle Anforderungen an *serviceA* ein Rückgriff auf die *default*-Version von *serviceA*.

## <a name="next-steps"></a>Nächste Schritte

Einige Beispiele für die Verwendung von Routing für schnelle Iterationen und die schnelle Entwicklung in Azure Dev Spaces finden Sie unter [Funktionsweise des lokalen Prozesses mit Kubernetes][how-it-works-local-process-kubernetes], [Funktionsweise von Remotedebuggen Ihres Codes mit Azure Dev Spaces ][how-it-works-remote-debugging] und [GitHub Actions und Azure Kubernetes Service][pr-flow].

Informationen zu den ersten Schritten bei der Verwendung von Routing mit Azure Dev Spaces für die Teamentwicklung finden Sie im Schnellstart [Teamentwicklung in Azure Dev Spaces][quickstart-team].

[helm-upgrade]: https://helm.sh/docs/intro/using_helm/#helm-upgrade-and-helm-rollback-upgrading-a-release-and-recovering-on-failure
[how-it-works-local-process-kubernetes]: /visualstudio/containers/overview-local-process-kubernetes
[how-it-works-remote-debugging]: how-dev-spaces-works-remote-debugging.md
[pr-flow]: how-to/github-actions.md
[quickstart-team]: quickstart-team-development.md
[troubleshooting]: troubleshooting.md