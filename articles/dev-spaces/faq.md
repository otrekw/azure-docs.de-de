---
title: Häufig gestellte Fragen zu Azure Dev Spaces
services: azure-dev-spaces
ms.date: 09/25/2019
ms.topic: conceptual
description: Hier finden Sie Antworten auf einige der häufig gestellten Fragen zu Azure Dev Spaces.
keywords: 'Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, Container, Helm, Service Mesh, Service Mesh-Routing, kubectl, k8s '
ms.openlocfilehash: 2baab0812061bec7dcf08d35056804313d873889
ms.sourcegitcommit: 8cf199fbb3d7f36478a54700740eb2e9edb823e8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/25/2019
ms.locfileid: "74482308"
---
# <a name="frequently-asked-questions-about-azure-dev-spaces"></a>Häufig gestellte Fragen zu Azure Dev Spaces

Im Folgenden werden einige der häufig gestellten Fragen zu Azure Dev Spaces beantwortet.

## <a name="which-azure-regions-currently-provide-azure-dev-spaces"></a>In welchen Azure-Regionen wird Azure Dev Spaces derzeit zur Verfügung gestellt?

Eine vollständige Liste der verfügbaren Regionen finden Sie unter [Unterstützte Regionen und Konfigurationen][supported-regions].

## <a name="can-i-use-azure-dev-spaces-without-a-public-ip-address"></a>Kann ich Azure Dev Spaces auch ohne eine öffentliche IP-Adresse verwenden?

Nein, Azure Dev Spaces kann ohne öffentliche IP-Adresse nicht in einem AKS-Cluster bereitgestellt werden. Eine öffentliche IP-Adresse ist bei Azure Dev Spaces [für das Routing erforderlich][dev-spaces-routing].

## <a name="can-i-use-my-own-ingress-with-azure-dev-spaces"></a>Kann ich mit Azure Dev Spaces meinen eigenen Dateneingangscontroller verwenden?

Ja, Sie können neben dem von Azure Dev Spaces erstellten Controller auch einen eigenen konfigurieren. Hierzu können Sie z. B. [traefik][ingress-traefik] verwenden.

## <a name="can-i-use-https-with-azure-dev-spaces"></a>Kann ich HTTPS mit Azure Dev Spaces verwenden?

Ja, Sie können mithilfe von [traefik][ingress-https-traefik] Ihren eigenen Eingangscontroller mit HTTPS konfigurieren.

## <a name="can-i-use-azure-dev-spaces-on-a-cluster-that-uses-cni-rather-than-kubenet"></a>Kann ich Azure Dev Spaces in einem Cluster verwenden, der CNI anstelle von Kubenet verwendet? 

Ja, Sie können Azure Dev Spaces in einem AKS-Cluster verwenden, der für Netzwerke CNI verwendet. Beispielsweise können Sie Azure Dev Spaces in einem AKS-Cluster mit [vorhandenen Windows-Containern][windows-containers] verwenden, der für Netzwerke CNI verwendet.

## <a name="can-i-use-azure-dev-spaces-with-windows-containers"></a>Kann ich Azure Dev Spaces mit Windows-Containern verwenden?

Derzeit ist Azure Dev Spaces nur für die Ausführung auf Linux-Pods und -Knoten vorgesehen. Sie können Azure Dev Spaces aber in einem AKS-Cluster mit [vorhandenen Windows-Containern][windows-containers] ausführen.

## <a name="can-i-use-azure-dev-spaces-on-aks-clusters-with-api-server-authorized-ip-address-ranges-enabled"></a>Kann ich Azure Dev Spaces in AKS-Clustern mit vom API-Server autorisierten IP-Adressbereichen verwenden?

Ja. Sie können Azure Dev Spaces in AKS-Clustern mit [vom API-Server autorisierten IP-Adressbereichen][aks-auth-range] verwenden. Beim [Erstellen][aks-auth-range-create] des Clusters müssen Sie [zusätzliche Bereiche auf der Grundlage Ihrer Region][aks-auth-range-ranges] lassen. Sie können auch einen bereits vorhandenen Cluster [aktualisieren][aks-auth-range-update], um diese zusätzlichen Bereiche zuzulassen.

### <a name="can-i-use-azure-dev-spaces-on-aks-clusters-with-restricted-egress-traffic-for-cluster-nodes"></a>Kann ich Azure Dev Spaces in AKS-Clustern mit eingeschränktem ausgehenden Datenverkehr für Clusterknoten verwenden?

Ja. Sie können Azure Dev Spaces in AKS-Clustern mit [eingeschränktem ausgehenden Datenverkehr für Clusterknoten][aks-restrict-egress-traffic] verwenden, sobald die folgenden FQDNs zugelassen wurden:

| FQDN                                    | Port      | Zweck      |
|-----------------------------------------|-----------|----------|
| cloudflare.docker.com | HTTPS: 443 | Pullen von Linux Alpine und anderen Azure Dev Spaces-Images |
| gcr.io | HTTP:443 | Pullen von Helm-/Tiller-Images |
| storage.googleapis.com | HTTP:443 | Pullen von Helm-/Tiller-Images |
| azds-<guid>.<location>.azds.io | HTTPS: 443 | Kommunizieren mit Azure Dev Spaces-Back-End-Diensten für Ihren Controller. Den genauen FQDN finden Sie in „dataplaneFqdn“ unter „%USERPROFILE%\.azds\settings.json“. |


[aks-auth-range]: ../aks/api-server-authorized-ip-ranges.md
[aks-auth-range-create]: ../aks/api-server-authorized-ip-ranges.md#create-an-aks-cluster-with-api-server-authorized-ip-ranges-enabled
[aks-auth-range-ranges]: https://github.com/Azure/dev-spaces/tree/master/public-ips
[aks-auth-range-update]: ../aks/api-server-authorized-ip-ranges.md#update-a-clusters-api-server-authorized-ip-ranges
[aks-restrict-egress-traffic]: ../aks/limit-egress-traffic.md
[dev-spaces-routing]: how-dev-spaces-works.md#how-routing-works
[ingress-traefik]: how-to/ingress-https-traefik.md#configure-a-custom-traefik-ingress-controller
[ingress-https-traefik]: how-to/ingress-https-traefik.md#configure-the-traefik-ingress-controller-to-use-https
[supported-regions]: about.md#supported-regions-and-configurations
[windows-containers]: how-to/run-dev-spaces-windows-containers.md