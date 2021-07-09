---
author: alexeyo26
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 03/19/2021
ms.author: alexeyo
ms.openlocfilehash: db8ae4bba6717cb1c0f27befd0897b60c2760ed2
ms.sourcegitcommit: 6ed3928efe4734513bad388737dd6d27c4c602fd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/07/2021
ms.locfileid: "110059150"
---
## <a name="private-endpoints-and-vnet-service-endpoints"></a>Private Endpunkte und VNet-Dienstendpunkte

Azure bietet private Endpunkte und VNet-Dienstendpunkte für das Tunneln von Datenverkehr mithilfe des [privaten Azure-Backbone-Netzwerks](https://azure.microsoft.com/global-infrastructure/global-network/). Diese Endpunkttypen ähneln sich in ihrem Zweck und den Technologien, auf denen sie basieren. Es gibt jedoch Unterschiede zwischen den beiden Technologien, und wir empfehlen, sich vor dem Entwurf Ihres Netzwerks über die Vor- und Nachteile der beiden Technologien zu informieren.

Dies sind einige Punkte, die Sie bei der Auswahl berücksichtigen sollten:
- Beide Technologien stellen sicher, dass der Datenverkehr zwischen dem VNet und der Speech-Ressource *nicht* über das öffentliche Internet erfolgt.
- Ein privater Endpunkt stellt eine dedizierte private IP-Adresse für Ihre Speech-Ressource bereit. Auf diese IP-Adresse kann nur innerhalb eines bestimmten VNets und Subnetzes zugegriffen werden. Sie haben die volle Kontrolle über den Zugriff auf diese IP-Adresse innerhalb Ihrer Netzwerkinfrastruktur.
- VNet-Dienstendpunkte stellen *keine* dedizierte private IP-Adresse für die Speech-Ressource bereit, sondern kapseln stattdessen alle Pakete, die an die Speech-Ressource gesendet werden, und übermitteln sie direkt über das Azure-Backbone-Netzwerk.
- Beide Technologien unterstützen lokale Szenarien. Standardmäßig sind bei Verwendung von VNet-Dienstendpunkten Azure-Dienstressourcen, die für virtuelle Netzwerke gesichert sind, nicht von lokalen Netzwerken aus erreichbar, [aber dies kann eingerichtet werden](../../../virtual-network/virtual-network-service-endpoints-overview.md#secure-azure-service-access-from-on-premises).
- VNet-Dienstpunkte werden häufig verwendet, wenn Sie den Zugriff für Ihre Speech-Ressource basierend auf den VNets, aus denen der Datenverkehr stammt, einschränken möchten.
- Bei Cognitive Services erzwingt die Aktivierung des VNet-Dienstendpunkts, dass der Datenverkehr für **alle** Cognitive Services-Ressourcen über das private Backbone-Netzwerk geleitet wird. Dies erfordert eine explizite Konfiguration für den Netzwerkzugriff (weitere Informationen finden Sie [hier](../speech-service-vnet-service-endpoint.md#configure-vnets-and-the-speech-resource-networking-settings)). Private Endpunkte besitzen diese Einschränkung nicht und bieten mehr Flexibilität für Ihre Netzwerkkonfiguration. Sie können auf eine Ressource über das private Backbone und auf eine andere über das öffentliche Internet zugreifen, indem Sie dasselbe Subnetz desselben VNets verwenden.
- Für private Endpunkte entstehen [zusätzliche Kosten](https://azure.microsoft.com/pricing/details/private-link). VNet-Dienstendpunkte sind kostenlos.
- Private Endpunkte erfordern eine [zusätzliche DNS-Konfiguration](../speech-services-private-link.md#turn-on-private-endpoints).
- Eine Speech-Ressource kann gleichzeitig mit privaten Endpunkten und VNet-Dienstendpunkten arbeiten.

Es wird empfohlen, beide Endpunkttypen zu testen, bevor Sie sich für einen Entwurf für Ihre Produktionsumgebung entscheiden. 

- [Private Link und privater Endpunkt: Dokumentation](../../../private-link/private-link-overview.md)
- [VNet-Dienstendpunkt: Dokumentation](../../../virtual-network/virtual-network-service-endpoints-overview.md)
