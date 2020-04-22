---
title: Netzwerkkonfiguration für Azure Dev Spaces in verschiedenen Netzwerktopologien
services: azure-dev-spaces
ms.date: 03/17/2020
ms.topic: conceptual
description: Beschreibung der Netzwerkanforderungen für die Ausführung von Azure Dev Spaces in Azure Kubernetes Service
keywords: Azure Dev Spaces, Dev Spaces, Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, Container, CNI, kubenet, SDN, Netzwerk
ms.openlocfilehash: 3e344576caf276ae7cb5fe00395c84810a4e7d32
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2020
ms.locfileid: "81262042"
---
# <a name="configure-networking-for-azure-dev-spaces-in-different-network-topologies"></a>Netzwerkkonfiguration für Azure Dev Spaces in verschiedenen Netzwerktopologien

Azure Dev Spaces wird in Azure Kubernetes Service-Clustern (AKS) mit der Standardnetzwerkkonfiguration ausgeführt. Wenn Sie die Netzwerkkonfiguration Ihres AKS-Clusters ändern möchten, um den Cluster z. B. hinter einer Firewall zu platzieren oder Netzwerksicherheitsgruppen oder Netzwerkrichtlinien zu verwenden, müssen Sie zusätzliche Überlegungen zur Ausführung von Azure Dev Spaces berücksichtigen.

![Konfiguration von virtuellen Netzwerken](media/configure-networking/virtual-network-clusters.svg)

## <a name="virtual-network-or-subnet-configurations"></a>Konfiguration des virtuellen Netzwerks oder Subnetzes

In einem AKS-Cluster kann eine abweichende Konfiguration des virtuellen Netzwerks oder des Subnetzes definiert sein, um den eingehenden oder ausgehenden Datenverkehr für den AKS-Cluster einzuschränken. Der Cluster kann sich beispielsweise hinter einer Firewall (z. B. Azure Firewall) befinden. Oder Sie verwenden möglicherweise Netzwerksicherheitsgruppen oder benutzerdefinierte Rollen zum Einschränken des Netzwerkdatenverkehrs. Ein Beispiel für eine Netzwerkkonfiguration finden Sie im [Azure Dev Spaces-Beispielrepository auf GitHub][sample-repo].

Azure Dev Spaces hat bestimmte Anforderungen für *eingehenden und ausgehenden* Netzwerkdatenverkehr sowie für *nur eingehenden*  Datenverkehr. Wenn Sie Azure Dev Spaces in einem AKS-Cluster mit einer Konfiguration eines virtuellen Netzwerks oder eines Subnetzes verwenden, die den Datenverkehr für den AKS-Cluster einschränkt, müssen Sie die folgenden Anforderungen für nur eingehenden sowie für eingehenden und ausgehenden Datenverkehr beachten, damit Azure Dev Spaces ordnungsgemäß ausgeführt wird.

### <a name="ingress-and-egress-network-traffic-requirements"></a>Anforderungen für eingehenden und ausgehenden Netzwerkdatenverkehr

In Azure Dev Spaces ist eingehender und ausgehender Datenverkehr für folgende vollqualifizierte Domänennamen (FQDNs) erforderlich:

| FQDN                       | Port       | Zweck      |
|----------------------------|------------|----------|
| cloudflare.docker.com      | HTTPS: 443 | Pullen von Docker-Images für Azure Dev Spaces |
| gcr.io                     | HTTPS: 443 | Pullen von Helm-Images für Azure Dev Spaces |
| storage.googleapis.com     | HTTPS: 443 | Pullen von Helm-Images für Azure Dev Spaces |
| azds-*.azds.io             | HTTPS: 443 | Kommunikation mit Azure Dev Spaces-Back-End-Diensten für den Azure Dev Spaces-Controller. Der genaue vollqualifizierte Domänenname befindet sich in *dataplaneFqdn* unter `USERPROFILE\.azds\settings.json`. |

Aktualisieren Sie die Firewall- oder Sicherheitskonfiguration, um Netzwerkdatenverkehr zu und von allen oben genannten vollqualifizierten Domänennamen zuzulassen. Wenn Sie z. B. eine Firewall verwenden, um Ihr Netzwerk zu schützen, müssen die oben genannten vollqualifizierten Domänennamen in der Anwendungsregel der Firewall hinzugefügt werden, um Datenverkehr zu und von diesen Domänen zuzulassen.

### <a name="ingress-only-network-traffic-requirements"></a>Anforderungen für nur eingehenden Netzwerkdatenverkehr

Azure Dev Spaces ermöglicht das Routing auf Kubernetes-Namespaceebene sowie öffentlichen Zugriff auf Dienste, die einen eigenen vollqualifizierten Domänennamen verwenden. Damit beide Funktionen ausgeführt werden, müssen Sie die Firewall- oder Netzwerkkonfiguration aktualisieren, um den öffentlichen Zugriff auf die externe IP-Adresse des Azure Dev Spaces-Eingangscontrollers im Cluster zu ermöglichen. Alternativ können Sie einen [internen Lastenausgleich][aks-internal-lb] einrichten und eine NAT-Regel in der Firewall hinzufügen, um die öffentliche IP-Adresse der Firewall in die IP-Adresse des internen Lastenausgleichsmoduls umzuwandeln. Außerdem können Sie [traefik][traefik-ingress] oder [NGINX][nginx-ingress] verwenden, um einen benutzerdefinierten Eingangscontroller zu erstellen.

## <a name="aks-cluster-network-requirements"></a>Netzwerkanforderungen für den AKS-Cluster

In AKS können Sie über [Netzwerkrichtlinien][aks-network-policies] den eingehenden und ausgehenden Datenverkehr zwischen Pods in einem Cluster sowie den ausgehenden Datenverkehr von einem Pod steuern. Azure Dev Spaces hat bestimmte Anforderungen für *eingehenden und ausgehenden* Netzwerkdatenverkehr sowie für *nur eingehenden*  Datenverkehr. Wenn Sie Azure Dev Spaces in einem AKS-Cluster mit AKS-Netzwerkrichtlinien verwenden, müssen Sie die folgenden Anforderungen für nur eingehenden sowie für eingehenden und ausgehenden Datenverkehr beachten, damit Azure Dev Spaces ordnungsgemäß ausgeführt wird.

### <a name="ingress-and-egress-network-traffic-requirements"></a>Anforderungen für eingehenden und ausgehenden Netzwerkdatenverkehr

Azure Dev Spaces ermöglicht zum Debuggen die direkte Kommunikation mit einem Pod in einem Entwicklungsbereich in Ihrem Cluster. Damit diese Funktion ausgeführt wird, müssen Sie eine Netzwerkrichtlinie hinzufügen, die die eingehende und ausgehende Kommunikation für die IP-Adressen der Azure Dev Spaces-Infrastruktur zulässt, die [je nach Region variieren][dev-spaces-ip-auth-range-regions].

### <a name="ingress-only-network-traffic-requirements"></a>Anforderungen für nur eingehenden Netzwerkdatenverkehr

Azure Dev Spaces ermöglicht das Routing zwischen Pods in unterschiedlichen Namespaces. Beispielsweise können Namespaces mit aktiviertem Azure Dev Spaces eine Über- und Unterordnungsbeziehung aufweisen, sodass Netzwerkdatenverkehr zwischen Pods in den übergeordneten und untergeordneten Namespaces weitergeleitet werden kann. Azure Dev Spaces stellt auch Dienstendpunkte mit eigenem FQDN bereit. Weitere Informationen zum Konfigurieren verschiedener Optionen zum Bereitstellen von Diensten sowie zu den Auswirkungen auf das Routing auf Namespaceebene finden Sie unter [Verwenden verschiedener Endpunktoptionen][endpoint-options].

## <a name="using-azure-cni"></a>Verwenden von Azure CNI

AKS-Cluster sind standardmäßig für die Verwendung von [kubenet][aks-kubenet] für Netzwerke konfiguriert, das mit Azure Dev Spaces verwendet werden kann. Sie können Ihren AKS-Cluster auch so konfigurieren, dass [Azure CNI (Container Networking Interface)][aks-cni] verwendet wird. Wenn Sie Azure Dev Spaces mit Azure CNI in Ihrem AKS-Cluster verwenden möchten, lassen Sie für den Adressraum des virtuellen Netzwerks und des Subnetzes bis zu 10 private IP-Adressen für Pods zu, die von Azure Dev Spaces bereitgestellt werden. Weitere Informationen zum Zulassen von privaten IP-Adressen finden Sie in der [Dokumentation zu Azure CNI in AKS][aks-cni-ip-planning].

## <a name="using-api-server-authorized-ip-ranges"></a>Verwenden von vom API-Server autorisierten IP-Adressbereichen

In AKS-Clustern können Sie zusätzliche Sicherheitseinstellungen konfigurieren, die einschränken, welche IP-Adressen mit Ihren Clustern interagieren können, z. B. durch Verwenden von virtuellen Netzwerken oder durch [Schützen des Zugriffs auf den API-Server mithilfe von autorisierten IP-Adressbereichen][aks-ip-auth-ranges]. Um bei Verwendung dieser zusätzlichen Sicherheitseinstellungen beim [Erstellen][aks-ip-auth-range-create] Ihres Clusters Azure Dev Spaces verwenden zu können, müssen Sie [zusätzliche Bereiche auf Grundlage Ihrer Region zulassen][dev-spaces-ip-auth-range-regions]. Sie können auch einen bereits vorhandenen Cluster [aktualisieren][aks-ip-auth-range-update], um diese zusätzlichen Bereiche zuzulassen. Außerdem müssen Sie die IP-Adressen aller Entwicklungscomputer zulassen, die zum Debuggen eine Verbindung mit dem AKS-Cluster herstellen, um eine Verbindung mit dem API-Server herzustellen.

## <a name="using-aks-private-clusters"></a>Verwenden von privaten AKS-Clustern

Derzeit wird Azure Dev Spaces nicht mit [privaten AKS-Clustern][aks-private-clusters] unterstützt.

## <a name="using-different-endpoint-options"></a>Verwenden verschiedener Endpunktoptionen

Azure Dev Spaces bietet die Möglichkeit, Endpunkte für Ihre bereitzustellen, die unter AKS ausgeführt werden. Wenn Sie Azure Dev Spaces für Ihren Cluster aktivieren, stehen Ihnen die folgenden Optionen zur Verfügung, um den Endpunkttyp für Ihren Cluster zu konfigurieren:

* Ein *öffentlicher* Endpunkt (Standardeinstellung) stellt einen Eingangscontroller mit einer öffentlichen IP-Adresse bereit. Die öffentliche IP-Adresse wird im DNS des Clusters registriert und ermöglicht öffentlichen Zugriff auf ihre Dienste mithilfe einer URL. Sie können diese URL mit `azds list-uris` anzeigen.
* Ein *privater* Endpunkt stellt einen Eingangscontroller mit einer privaten IP-Adresse bereit. Mit einer privaten IP-Adresse kann nur innerhalb des virtuellen Netzwerks des Clusters auf den Load Balancer für den Cluster zugegriffen werden. Die private IP-Adresse des Load Balancers wird im DNS des Clusters registriert, sodass auf Dienste im virtuellen Netzwerk des Clusters mithilfe einer URL zugegriffen werden kann. Sie können diese URL mit `azds list-uris` anzeigen.
* Wenn Sie *Keine* als Endpunktoption festlegen, wird kein Eingangscontroller bereitgestellt. Wenn kein Eingangscontroller bereitgestellt wird, funktionieren die [Azure Dev Spaces-Routingfunktionen][dev-spaces-routing] nicht. Optional können Sie eine eigene Eingangscontrollerlösung mit [traefik][traefik-ingress] oder [NGINX][nginx-ingress] implementieren, damit die Routingfunktionen wieder funktionieren.

Verwenden Sie zum Konfigurieren der Endpunktoption *-e* oder *--endpoint*, wenn Sie Azure Dev Spaces auf Ihrem Cluster aktivieren. Beispiel:

> [!NOTE]
> Die Endpunktoption setzt voraus, dass Sie Version 2.2.0 oder höher der Azure CLI ausführen. Führen Sie `az --version` aus, um die Version zu ermitteln. Informationen zum Durchführen einer Installation oder eines Upgrades finden Sie bei Bedarf unter [Installieren der Azure CLI][azure-cli-install].

```azurecli
az aks use-dev-spaces -g MyResourceGroup -n MyAKS -e private
```

## <a name="client-requirements"></a>Clientanforderungen

In Azure Dev Spaces werden zum Debuggen clientseitige Tools, z. B. die CLI-Erweiterung für Azure Dev Spaces, die Visual Studio Code-Erweiterung und die Visual Studio-Erweiterung, für die Kommunikation mit dem AKS-Cluster verwendet. Zum Verwenden von clientseitigen Tools in Azure Dev Spaces müssen Sie den Datenverkehr von den Entwicklungscomputern zur Domäne *azds-\*.azds.io* zulassen. Den genauen vollqualifizierten Domänennamen finden Sie in *dataplaneFqdn* unter `USERPROFILE\.azds\settings.json`. Wenn Sie [vom API-Server autorisierte IP-Adressbereiche][auth-range-section] verwenden, müssen Sie außerdem die IP-Adressen aller Entwicklungscomputer zulassen, die zum Debuggen eine Verbindung mit dem AKS-Cluster herstellen, um eine Verbindung mit dem API-Server herzustellen.

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich darüber, wie Azure Dev Spaces Sie bei der Entwicklung komplexerer containerübergreifender Anwendungen unterstützt und wie Sie die gemeinsame Entwicklung vereinfachen können, indem Sie in verschiedenen Bereichen mit verschiedenen Versionen oder Branches Ihres Codes arbeiten.

> [!div class="nextstepaction"]
> [Schnellstart: Entwicklung im Team unter Kubernetes: Azure Dev Spaces][team-quickstart]

[aks-cni]: ../aks/configure-azure-cni.md
[aks-cni-ip-planning]: ../aks/configure-azure-cni.md#plan-ip-addressing-for-your-cluster
[aks-kubenet]: ../aks/configure-kubenet.md
[aks-internal-lb]: ../aks/internal-lb.md
[aks-ip-auth-ranges]: ../aks/api-server-authorized-ip-ranges.md
[aks-ip-auth-range-create]: ../aks/api-server-authorized-ip-ranges.md#create-an-aks-cluster-with-api-server-authorized-ip-ranges-enabled
[aks-ip-auth-range-update]: ../aks/api-server-authorized-ip-ranges.md#update-a-clusters-api-server-authorized-ip-ranges
[aks-network-policies]: ../aks/use-network-policies.md
[aks-private-clusters]: ../aks/private-clusters.md
[auth-range-section]: #using-api-server-authorized-ip-ranges
[azure-cli-install]: /cli/azure/install-azure-cli
[dev-spaces-ip-auth-range-regions]: https://github.com/Azure/dev-spaces/tree/master/public-ips
[dev-spaces-routing]: how-dev-spaces-works-routing.md
[endpoint-options]: #using-different-endpoint-options
[traefik-ingress]: how-to/ingress-https-traefik.md
[nginx-ingress]: how-to/ingress-https-nginx.md
[sample-repo]: https://github.com/Azure/dev-spaces/tree/master/advanced%20networking
[team-quickstart]: quickstart-team-development.md