---
title: Serverlose Container in Azure
description: Mit den Azure Container Instances-Dienst lassen sich isolierte Container in Azure besonders schnell und einfach ausführen, ohne dass Sie dazu virtuelle Computer verwalten oder einen übergeordneten Orchestrator einführen müssen.
ms.topic: overview
ms.date: 04/25/2019
ms.custom: seodec18, mvc
ms.openlocfilehash: 261e5d0159b4201aab0e8aad1e05fa320cc76a14
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2020
ms.locfileid: "86259501"
---
# <a name="what-is-azure-container-instances"></a>Was ist Azure Container Instances?

Container entwickeln sich mehr und mehr zum bevorzugten Instrument für das Packen, Bereitstellen und Verwalten von Cloudanwendungen. Mit Azure Container Instances lassen sich Container in Azure besonders schnell und einfach ausführen, ohne dass Sie dazu virtuelle Computer verwalten oder einen übergeordneten Dienst einführen müssen.

Azure Container Instances ist eine großartige Lösung für jedes Szenario, das für die Verwendung isolierter Container geeignet ist. Hierzu zählen unter anderem einfache Anwendungen, Aufgabenautomatisierung und Erstellungsaufträge. Für Szenarien, die eine umfassende Containerorchestrierung erfordern (etwa für die containerübergreifende Dienstermittlung, automatische Skalierung und koordinierte Anwendungsupgrades), empfehlen wir [Azure Kubernetes Service](../aks/index.yml) (AKS).

## <a name="fast-startup-times"></a>Schneller Start

Container bieten im Vergleich zu virtuellen Computern (VMs) erhebliche Vorteile beim Start. Azure Container Instances kann in Sekundenschnelle Container in Azure starten, ohne virtuelle Computer bereitstellen und verwalten zu müssen.

## <a name="container-access"></a>Containerzugriff

Mit Azure Container Instances können Sie Ihre Containergruppen direkt über eine öffentliche IP-Adresse und einen vollqualifizierten Domainnamen (FQDN) im Internet verfügbar machen. Beim Erstellen einer Containerinstanz können Sie eine benutzerdefinierte DNS-Namensbezeichnung angeben, sodass Ihre Anwendung unter „*customlabel*.*azureregion*.azurecontainer.io“ erreichbar ist.

Azure Container Instances unterstützt auch das Ausführen eines Befehls in einem ausgeführten Container durch Bereitstellen einer interaktiven Shell zur Unterstützung durch Anwendungsentwicklung und Problembehandlung. Der Zugriff erfolgt über HTTPS und verwendet TLS zum Sichern von Clientverbindungen.

> [!IMPORTANT]
> Ab dem 13. Januar 2020 setzt Azure Container Instances voraus, dass alle sicheren Verbindungen von Servern und Anwendungen TLS 1.2 verwenden. Die Unterstützung für TLS 1.0 und 1.1 wird eingestellt.

## <a name="hypervisor-level-security"></a>Sicherheit auf Hypervisor-Ebene

In der Vergangenheit verfügten Container zwar über Anwendungsabhängigkeitsisolierung und Ressourcenkontrolle, galten aber nicht als ausreichend gehärtet, um in einer gefährlichen Umgebung mit mehreren Mandanten verwendet werden zu können. Azure Container Instances gewährleistet, dass Ihre Anwendung in einem Container isoliert ist – genau wie bei einem virtuellen Computer.


## <a name="custom-sizes"></a>Benutzerdefinierte Größen

Container sind in der Regel für die Ausführung einer einzelnen Anwendung optimiert. Die genauen Anforderungen dieser Anwendungen können sich jedoch erheblich voneinander unterscheiden. Azure Container Instances ermöglicht exakte Angaben für CPU-Kerne und Arbeitsspeicher und bietet dadurch eine optimale Auslastung. Dank sekundengenauer Abrechnung können Sie Ihre Ausgaben auf der Grundlage Ihres tatsächlichen Bedarfs präzise optimieren.

Bei rechenintensiven Aufträgen wie maschinelles Lernen kann Azure Container Instances Linux-Container für die Nutzung von NVIDIA Tesla [GPU-Ressourcen](container-instances-gpu.md) (Vorschau) planen.

## <a name="persistent-storage"></a>Permanenter Speicher

Dank der Möglichkeit zur durch Azure Storage gesicherten direkten [Einbindung von Azure Files-Freigaben](./container-instances-volume-azure-files.md) können Sie mit Azure Container Instances den Zustand abrufen und speichern.

## <a name="linux-and-windows-containers"></a>Linux- und Windows-Container

Azure Container Instances kann sowohl Windows- als auch Linux-Container mit der gleichen API planen. Geben Sie einfach den BS-Typ an, wenn Sie Ihre [Containergruppen](container-instances-container-groups.md) erstellen.

Einige Features sind momentan auf Linux-Container beschränkt:

* Mehrere Container pro Containergruppe
* Einbindung von Volumes ([Azure Files](container-instances-volume-azure-files.md), [emptyDir](container-instances-volume-emptydir.md), [GitRepo](container-instances-volume-gitrepo.md), [geheimes Volume](container-instances-volume-secret.md))
* [Ressourcennutzungsmetriken](container-instances-monitor.md) von Azure Monitor
* [VNET-Bereitstellung](container-instances-vnet.md)
* [GPU-Ressourcen](container-instances-gpu.md) (Vorschauversion)

Verwenden Sie für Windows-Containerbereitstellungen Images, die auf allgemeinen [Windows-Basisimages](container-instances-faq.md#what-windows-base-os-images-are-supported) beruhen.

> [!NOTE]
> Die Verwendung von auf Windows Server 2019 basierenden Images in Azure Container Instances befindet sich im Vorschaustadium.

## <a name="co-scheduled-groups"></a>Gemeinsam geplante Gruppen

Azure Container Instances unterstützt die Planung von [Gruppen mit mehreren Containern](container-instances-container-groups.md) mit gemeinsamem Hostcomputer, lokalem Netzwerk, Speicher und Lebenszyklus. Dadurch können Sie Ihren Hauptanwendungscontainer mit anderen unterstützenden Rollencontainern (beispielsweise Protokollierungs-Sidecars) kombinieren.

## <a name="virtual-network-deployment"></a>VNET-Bereitstellung

Dieses Feature von Azure Container Instances ist derzeit in einigen Azure-Regionen für Produktionsworkloads verfügbar und ermöglicht die [Bereitstellung von Containerinstanzen in einem virtuellen Azure-Netzwerk](container-instances-vnet.md). Indem Containerinstanzen in einem Subnetz in Ihrem virtuellen Netzwerk bereitgestellt werden, können diese sicher mit anderen Ressourcen im virtuellen Netzwerk kommunizieren, einschließlich der lokalen Ressourcen (über das [VPN-Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md) oder [ExpressRoute](../expressroute/expressroute-introduction.md)).

## <a name="next-steps"></a>Nächste Schritte

Stellen Sie mit einem einzelnen Befehl einen Container in Azure bereit. Eine entsprechende Anleitung finden Sie in unserem Schnellstarthandbuch:

> [!div class="nextstepaction"]
> [Schnellstartanleitung: Erstellen Ihres ersten Containers in Azure Container Instances](container-instances-quickstart.md)

<!-- LINKS - External -->
[terms-of-use]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/
