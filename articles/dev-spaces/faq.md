---
title: Häufig gestellte Fragen zu Azure Dev Spaces
services: azure-dev-spaces
ms.date: 09/25/2019
ms.topic: conceptual
description: Hier finden Sie Antworten auf einige der häufig gestellten Fragen zu Azure Dev Spaces.
keywords: 'Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, Container, Helm, Service Mesh, Service Mesh-Routing, kubectl, k8s '
ms.openlocfilehash: d5ab56edfe4799d51fb7f08642aad9e2ee01db05
ms.sourcegitcommit: 05cdbb71b621c4dcc2ae2d92ca8c20f216ec9bc4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/16/2020
ms.locfileid: "76044973"
---
# <a name="frequently-asked-questions-about-azure-dev-spaces"></a>Häufig gestellte Fragen zu Azure Dev Spaces

Im Folgenden werden einige der häufig gestellten Fragen zu Azure Dev Spaces beantwortet.

## <a name="which-azure-regions-currently-provide-azure-dev-spaces"></a>In welchen Azure-Regionen wird Azure Dev Spaces derzeit zur Verfügung gestellt?

Eine vollständige Liste der verfügbaren Regionen finden Sie unter [Unterstützte Regionen][supported-regions].

## <a name="can-i-use-azure-dev-spaces-without-a-public-ip-address"></a>Kann ich Azure Dev Spaces auch ohne eine öffentliche IP-Adresse verwenden?

Nein, Azure Dev Spaces kann ohne öffentliche IP-Adresse nicht in einem AKS-Cluster bereitgestellt werden. Eine öffentliche IP-Adresse ist bei Azure Dev Spaces [für das Routing erforderlich][dev-spaces-routing].

## <a name="can-i-use-my-own-ingress-with-azure-dev-spaces"></a>Kann ich mit Azure Dev Spaces meinen eigenen Dateneingangscontroller verwenden?

Ja, Sie können neben dem von Azure Dev Spaces erstellten Controller auch einen eigenen konfigurieren. Hierzu können Sie z. B. [traefik][ingress-traefik] verwenden.

## <a name="can-i-use-https-with-azure-dev-spaces"></a>Kann ich HTTPS mit Azure Dev Spaces verwenden?

Ja, Sie können mithilfe von [traefik][ingress-https-traefik] Ihren eigenen Eingangscontroller mit HTTPS konfigurieren.

## <a name="can-i-use-azure-dev-spaces-on-a-cluster-that-uses-cni-rather-than-kubenet"></a>Kann ich Azure Dev Spaces in einem Cluster verwenden, der CNI anstelle von Kubenet verwendet? 

Ja, Sie können Azure Dev Spaces in einem AKS-Cluster verwenden, der für Netzwerke CNI verwendet. Beispielsweise können Sie Azure Dev Spaces in einem AKS-Cluster mit [vorhandenen Windows-Containern][windows-containers] verwenden, der für Netzwerke CNI verwendet. Weitere Informationen zur Verwendung von CNI für Netzwerke mit Azure Dev Spaces finden Sie [hier](configure-networking.md#using-azure-cni).

## <a name="can-i-use-azure-dev-spaces-with-windows-containers"></a>Kann ich Azure Dev Spaces mit Windows-Containern verwenden?

Derzeit ist Azure Dev Spaces nur für die Ausführung auf Linux-Pods und -Knoten vorgesehen. Sie können Azure Dev Spaces aber in einem AKS-Cluster mit [vorhandenen Windows-Containern][windows-containers] ausführen.

## <a name="can-i-use-azure-dev-spaces-on-aks-clusters-with-api-server-authorized-ip-address-ranges-enabled"></a>Kann ich Azure Dev Spaces in AKS-Clustern mit vom API-Server autorisierten IP-Adressbereichen verwenden?

Ja. Sie können Azure Dev Spaces in AKS-Clustern mit [vom API-Server autorisierten IP-Adressbereichen][aks-auth-range] verwenden. Weitere Informationen zur Verwendung eines AKS-Clusters mit vom API-Server autorisierten IP-Adressbereichen, die für Azure Dev Spaces aktiviert sind, finden Sie [hier](configure-networking.md#using-api-server-authorized-ip-ranges).

### <a name="can-i-use-azure-dev-spaces-on-aks-clusters-with-restricted-egress-traffic-for-cluster-nodes"></a>Kann ich Azure Dev Spaces in AKS-Clustern mit eingeschränktem ausgehenden Datenverkehr für Clusterknoten verwenden?

Ja. Sie können Azure Dev Spaces in AKS-Clustern mit [eingeschränktem ausgehenden Datenverkehr für Clusterknoten][aks-restrict-egress-traffic] verwenden, wenn die korrekten FQDNs zugelassen wurden. Weitere Informationen zur Verwendung eines AKS-Clusters mit eingeschränktem ausgehenden Datenverkehr für Clusterknoten, die für Azure Dev Spaces aktiviert sind, finden Sie [hier](configure-networking.md#ingress-and-egress-network-traffic-requirements).

[aks-auth-range]: ../aks/api-server-authorized-ip-ranges.md
[aks-auth-range-create]: ../aks/api-server-authorized-ip-ranges.md#create-an-aks-cluster-with-api-server-authorized-ip-ranges-enabled
[aks-auth-range-ranges]: https://github.com/Azure/dev-spaces/tree/master/public-ips
[aks-auth-range-update]: ../aks/api-server-authorized-ip-ranges.md#update-a-clusters-api-server-authorized-ip-ranges
[aks-restrict-egress-traffic]: ../aks/limit-egress-traffic.md
[dev-spaces-routing]: how-dev-spaces-works.md#how-routing-works
[ingress-traefik]: how-to/ingress-https-traefik.md#configure-a-custom-traefik-ingress-controller
[ingress-https-traefik]: how-to/ingress-https-traefik.md#configure-the-traefik-ingress-controller-to-use-https
[supported-regions]: https://azure.microsoft.com/global-infrastructure/services/?products=kubernetes-service
[windows-containers]: how-to/run-dev-spaces-windows-containers.md