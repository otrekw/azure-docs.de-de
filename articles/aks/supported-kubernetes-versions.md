---
title: Unterstützte Kubernetes-Versionen in Azure Kubernetes Service
description: Grundlegendes zur Richtlinie zur Unterstützung der Kubernetes-Version und zum Lebenszyklus von Clustern in Azure Kubernetes Service (AKS)
services: container-service
author: sauryadas
ms.topic: article
ms.date: 12/09/2019
ms.author: saudas
ms.openlocfilehash: 77d64391888957a1697a5823a1485413686682d1
ms.sourcegitcommit: 99ac4a0150898ce9d3c6905cbd8b3a5537dd097e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/25/2020
ms.locfileid: "77593443"
---
# <a name="supported-kubernetes-versions-in-azure-kubernetes-service-aks"></a>Unterstützte Kubernetes-Versionen in Azure Kubernetes Service (AKS)

Die Kubernetes-Community veröffentlicht etwa alle drei Monate Nebenversionen. Diese Releases enthalten neue Features und Verbesserungen. Patchreleases kommen häufiger vor (manchmal wöchentlich) und sind nur für wichtige Fehlerbehebungen in einer Nebenversion gedacht. Diese Patchreleases beinhalten Behebungen von Sicherheitsrisiken oder größerer Fehler, die viele Kunden und Produkte betreffen, die in einer auf Kubernetes basierenden Produktionsumgebung ausgeführt werden.

AKS zielt darauf ab, neue Kubernetes-Versionen je nach Stabilität des Upstreamrelease innerhalb von 30 Tagen zu zertifizieren und zu veröffentlichen.

## <a name="kubernetes-versions"></a>Kubernetes-Versionen

Kubernetes verwendet als Standardversionierungsschema die [semantische Versionierung](https://semver.org/). Für jede Kubernetes-Version wird daher das folgende Nummerierungsschema verwendet:

```
[major].[minor].[patch]

Example:
  1.12.14
  1.12.15
```

Für jede Zahl in der Version gilt, dass allgemeine Kompatibilität mit der vorherigen Version besteht:

* Die Hauptversionen ändern sich, wenn nicht kompatible API-Änderungen vorgenommen werden oder möglicherweise keine Abwärtskompatibilität mehr gewährleistet wird.
* Nebenversionen ändern sich, wenn Änderungen an der Funktionalität vorgenommen werden, die zu den anderen Nebenreleases abwärtskompatibel sind.
* Patchversionen ändern sich, wenn abwärtskompatible Fehlerbehebungen vorgenommen werden.

Benutzer sollten nach Möglichkeit immer das neueste Patchrelease der aktuell verwendeten Nebenversion nutzen. Wenn Ihr Produktionscluster beispielsweise die Version *1.12.14* verwendet und *1.12.15* die neueste verfügbare Patchversion für die Nebenversion *1.12* ist, sollten Sie ein Upgrade auf Version *1.12.15* durchführen, sobald Sie sicher sind, dass Ihr Cluster vollständig gepatcht ist und unterstützt wird.

## <a name="kubernetes-version-support-policy"></a>Richtlinie zur Unterstützung der Kubernetes-Version

AKS unterstützt drei Nebenversionen von Kubernetes:

* Die aktuelle Nebenversion, die in AKS veröffentlicht wird (N).
* Die beiden vorherigen Nebenversionen. Jede unterstützte Nebenversion unterstützt ebenfalls zwei stabile Patches.

Dies wird als „N-2“ bezeichnet: („N“ für das neueste Release – 2 für die Anzahl der Nebenversionen).

Wenn AKS beispielsweise die Version *1.15.a* einführt, werden die folgenden Versionen unterstützt:

Neue Nebenversion    |    Liste mit unterstützten Versionen
-----------------    |    ----------------------
1.15.a               |    1.15.a, 1.15.b, 1.14.c, 1.14.d, 1.13.e, 1.13.f

Dabei steht der Buchstabe für die jeweiligen Patchversionen.

Informationen zu Benachrichtigungen, die Versionsänderungen und damit verbundene Auswirkungen betreffen, finden Sie im Abschnitt „Benachrichtigungen“ weiter unten.

Wenn eine neue Nebenversion eingeführt wird, werden die älteste Nebenversion und die unterstützten Patchreleases nicht mehr unterstützt und entfernt. Angenommen, die aktuelle Liste mit den unterstützten Versionen sieht wie folgt aus:

```
1.15.a
1.15.b
1.14.c
1.14.d
1.13.e
1.13.f
```

AKS veröffentlicht 1.16. *. Dies bedeutet, dass Version 1.13.* (alle 1.13-Versionen) entfernt wird und aus dem Support herausfällt.

> [!NOTE]
> Wenn Kunden eine nicht unterstützte Kubernetes-Version verwenden, werden sie aufgefordert, ein Upgrade vorzunehmen, sobald sie Support für den Cluster anfordern. Die [AKS-Supportrichtlinien](https://docs.microsoft.com/azure/aks/support-policies) gelten nicht für Cluster, auf denen nicht unterstützte Kubernetes-Releases ausgeführt werden.

Zusätzlich zur Unterstützung der oben genannten Nebenversionen unterstützt AKS auch die beiden neuesten **Patchreleases** für eine bestimmte Nebenversion. Angenommen, folgende Versionen werden unterstützt:

```
Current Supported Version List
------------------------------
1.15.2, 1.15.1, 1.14.5, 1.14.4
```

Wenn Upstream-Kubernetes die Patchversionen 1.15.3 und 1.14.6 veröffentlicht und AKS diese ebenfalls veröffentlicht, werden die ältesten Patchversionen nicht mehr unterstützt und entfernt. Die Liste mit den unterstützten Versionen ändert sich dann wie folgt:

```
New Supported Version List
----------------------
1.15.*3*, 1.15.*2*, 1.14.*6*, 1.14.*5*
```

### <a name="communications"></a>Benachrichtigungen

* Bei neuen **Nebenversionen** von Kubernetes:
  * Alle Benutzer werden öffentlich über die neue Version benachrichtigt. Außerdem wird mitgeteilt, welche Version entfernt wird.
  * Wenn eine neue Patchversion veröffentlicht wird, wird gleichzeitig die älteste Patchversion entfernt.
  * Kunden haben ab dem Datum der öffentlichen Benachrichtigung **30 Tage** Zeit, ein Upgrade auf ein unterstütztes Release der Nebenversion durchzuführen.
* Bei neuen **Patchversionen** von Kubernetes:
  * Alle Benutzer werden über die neue Patchversion benachrichtigt. Außerdem werden sie aufgefordert, ein Upgrade auf das neueste Patchrelease vorzunehmen.
  * Benutzern verbleiben **30 Tage** für das Upgrade auf eine neuere, unterstützte Patchversion, bevor die älteste entfernt wird.

In AKS ist eine „veröffentlichte Version“ als allgemein verfügbare Version definiert, die in allen SLO/QoS-Messungen (Quality of Service/Service Level Objective) aktiviert und in allen Regionen verfügbar ist. AKS unterstützt möglicherweise auch Vorschauversionen, die explizit bezeichnet werden und den Nutzungsbedingungen für Vorschauversionen unterliegen.

#### <a name="notification-channels-for-aks-changes"></a>Benachrichtigungskanäle für AKS-Änderungen

Für AKS werden regelmäßige Dienstupdates veröffentlicht, in denen neue Kubernetes-Versionen, Dienständerungen und Komponentenupdates zusammengefasst sind, die für den Dienst auf [GitHub](https://github.com/Azure/AKS/releases) veröffentlicht wurden.

Diese Änderungen werden im Rahmen der regulären Wartung, die als Teil des verwalteten Diensts angeboten wird, an alle Kunden weitergegeben. Einige Änderungen erfordern explizite Upgrades, während für andere keine Maßnahmen erforderlich sind.

Benachrichtigungen werden außerdem an den folgenden Stellen veröffentlicht:

* in den [AKS-Versionshinweisen](https://aka.ms/aks/releasenotes)
* Benachrichtigungen im Azure-Portal
* im [Azure-Updatekanal][azure-update-channel]

### <a name="supported-versions-policy-exceptions"></a>Richtlinienausnahmen für unterstützte Versionen

AKS behält sich das Recht vor, ohne Vorankündigung neue oder bestehende Versionen hinzuzufügen oder zu entfernen, wenn in diesen kritische, die Produktion einschränkende Fehler oder Sicherheitsprobleme gefunden werden.

Bestimmte Patchreleases können übersprungen oder Rollouts je nach Schweregrad des Fehlers oder Sicherheitsproblems beschleunigt werden.

### <a name="azure-portal-and-cli-default-versions"></a>Standardversionen für das Azure-Portal und die Azure-CLI

Wenn Sie einen AKS-Cluster im Portal oder mit der Azure-Befehlszeilenschnittstelle bereitstellen, wird der Cluster standardmäßig auf die Nebenversion N-1 und den neuesten Patch festgelegt. Wenn AKS beispielsweise *1.15.a*, *1.15.b*, *1.14.c*, *1.14.d*, *1.13.e* und *1.13.f* unterstützt, ist die standardmäßig ausgewählte Version *1.14.c*.

AKS verwendet standardmäßig Version N-1, um Kunden eine bekannte, stabile und gepatchte Version bereitstellen zu können.

## <a name="list-currently-supported-versions"></a>Auflisten der derzeit unterstützten Versionen

Um herauszufinden, welche Versionen für Ihr Abonnement und Ihre Region derzeit verfügbar sind, verwenden Sie den Befehl [az aks get-versions][az-aks-get-versions]. Im folgenden Beispiel sind die verfügbaren Kubernetes-Versionen für die Region *EastUS* aufgelistet:

```azurecli-interactive
az aks get-versions --location eastus --output table
```

## <a name="faq"></a>Häufig gestellte Fragen

**Was geschieht, wenn ein Kunde ein Upgrade eines Kubernetes-Clusters mit einer Nebenversion durchführt, die nicht unterstützt wird?**

Wenn Sie die Version *N-3* verwenden, erhalten Sie keinen Support mehr und werden zu einem Upgrade aufgefordert. Wenn das Upgrade von Version N-3 auf N-2 erfolgreich ist, können Sie den Support entsprechend den Supportrichtlinien wieder nutzen. Beispiel:

- Wenn die älteste unterstützte AKS-Version *1.13.a* ist und Sie *1.12.b* oder eine ältere Version verwenden, steht Ihnen kein Support zur Verfügung.
- Wenn das Upgrade von *1.12.b* auf *1.13.a* oder höher erfolgreich ist, befinden Sie sich wieder im Rahmen unserer Supportrichtlinien.

Upgrades auf ältere Versionen als das unterstützte Fenster von *N-2* werden nicht unterstützt. In solchen Fällen wird Kunden empfohlen, neue AKS-Cluster zu erstellen und ihre Workloads unter Versionen innerhalb des unterstützten Fensters erneut bereitzustellen.

**Was bedeutet „nicht unterstützt“?**

„Nicht unterstützt“ bedeutet, dass die verwendete Version nicht in der Liste mit den unterstützten Versionen enthalten ist und Sie aufgefordert werden, für den Cluster ein Upgrade auf eine unterstützte Version vorzunehmen, sobald Sie Support anfordern. Des Weiteren stellt AKS für Cluster, die nicht in der Liste der unterstützten Versionen aufgeführt sind, weder Laufzeitgarantien noch sonstige Garantien bereit.

**Was geschieht, wenn ein Kunde einen Kubernetes-Cluster mit einer Nebenversion skaliert, die nicht unterstützt wird?**

Bei Nebenversionen, die nicht von AKS unterstützt werden, sollte das horizontale Hoch- oder Herunterskalieren weiterhin funktionieren, es wird jedoch dringend empfohlen, ein Upgrade durchzuführen, um wieder Support für den Cluster zu erhalten.

**Kann ein Kunde für immer eine Kubernetes-Version verwenden?**

Ja. Wenn der Cluster jedoch keine der von AKS unterstützten Versionen verwendet, gelten die AKS-Supportrichtlinien nicht für den Cluster. Azure führt kein automatisches Upgrade Ihres Clusters durch und löscht ihn nicht.

**Welche Version wird von der Steuerungsebene unterstützt, wenn der Knotenpool keine der unterstützten AKS-Versionen verwendet?**

Die Steuerungsebene muss sich innerhalb eines Fensters von Versionen aller Knotenpools befinden. Ausführliche Informationen zum Aktualisieren der Steuerungsebene oder der Knotenpools finden Sie in der Dokumentation zum [Aktualisieren von Knotenpools](use-multiple-node-pools.md#upgrade-a-cluster-control-plane-with-multiple-node-pools).

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Upgrade Ihres Clusters finden Sie unter [Durchführen eines Upgrades für einen Azure Kubernetes Service-Cluster (AKS)][aks-upgrade].

<!-- LINKS - External -->
[aks-engine]: https://github.com/Azure/aks-engine
[azure-update-channel]: https://azure.microsoft.com/updates/?product=kubernetes-service

<!-- LINKS - Internal -->
[aks-upgrade]: upgrade-cluster.md
[az-aks-get-versions]: /cli/azure/aks#az-aks-get-versions
