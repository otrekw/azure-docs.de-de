---
title: Beispiele für Azure Resource Manager-Vorlagen
description: Hier finden Sie Beispiele für Azure Resource Manager-Vorlagen zum Bereitstellen von Azure Container Instances in verschiedenen Konfigurationen.
ms.topic: article
ms.date: 03/07/2019
ms.openlocfilehash: 312cc2e7a59387a0a0b9cb36e7363f80f5a4c44b
ms.sourcegitcommit: 91fdedcb190c0753180be8dc7db4b1d6da9854a1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/17/2021
ms.locfileid: "112289043"
---
# <a name="azure-resource-manager-templates-for-azure-container-instances"></a>Azure Resource Manager-Vorlagen für Azure Container Instances

Die folgenden Beispielvorlagen stellen Containerinstanzen in verschiedenen Konfigurationen bereit.

Informationen zu Bereitstellungsoptionen finden Sie im Abschnitt [Bereitstellung](#deployment). Wenn Sie eigene Vorlagen erstellen möchten, nutzen Sie die ausführlichen Informationen zu Vorlagenformat und verfügbaren Eigenschaften in der [Resource Manager-Vorlagenreferenz][ref] von Azure Container Instances.

## <a name="sample-templates"></a>Beispielvorlagen

| Vorlage | BESCHREIBUNG |
|-|-|
| **Anwendungen** ||
| [WordPress][app-wp] | Erstellt eine WordPress-Website und ihre MySQL-Datenbank in einer Containergruppe. Der Inhalt der WordPress-Website und MySQL-Datenbank werden persistent in einer Azure Files-Freigabe gespeichert. Erstellt auch ein Anwendungsgateway, um öffentlichen Netzwerkzugriff auf WordPress zu ermöglichen. |
| [MS NAV mit SQL Server und IIS][app-nav] | Stellt einen einzelnen Windows-Container mit einer vollständig ausgestatteten eigenständigen Dynamics NAV / Dynamics 365 Business Central-Umgebung bereit. |
| **Volumes** ||
| [emptyDir][vol-emptydir] | Stellt zwei Linux-Container bereit, die ein emptyDir-Volume gemeinsam nutzen. |
| [gitRepo][vol-gitrepo] | Stellt einen Linux-Container bereit, der ein GitHub-Repository klont und als Volume einbindet. |
| [secret][vol-secret] | Stellt einen Linux-Container mit einem PFX-Zertifikat als geheimes Volume eingebunden bereit. |
| **Netzwerk** ||
| [Container mit verfügbar gemachtem UDP][net-udp] | Stellt einen Windows- oder Linux-Container bereit, der einen UDP-Port verfügbar macht. |
| [Linux-Container mit öffentlicher IP-Adresse][net-publicip] | Stellt einen einzelnen Linux-Container bereit, auf den über eine öffentliche IP-Adresse zugegriffen werden kann. |
| [Bereitstellen einer Containergruppe mit einem virtuellen Netzwerk][net-vnet] | Stellt ein neues virtuelles Netzwerk, Subnetz, Netzwerkprofil und eine neue Containergruppe bereit. |
| **Azure-Ressourcen** ||
| [Erstellen eines Azure Storage-Kontos und einer Files-Freigabe][az-files] | Verwendet die Azure CLI in einer Containerinstanz zum Erstellen eines Speicherkontos und einer Azure Files-Freigabe.

## <a name="deployment"></a>Bereitstellung

Sie können zwischen verschiedenen Optionen für das Bereitstellen von Ressourcen mit Resource Manager-Vorlagen wählen:

[Azure-Befehlszeilenschnittstelle][deploy-cli]

[Azure PowerShell][deploy-powershell]

[Azure portal][deploy-portal]

[REST-API][deploy-rest]

<!-- LINKS - External -->
[app-nav]: https://github.com/Azure/azure-quickstart-templates/tree/master/demos/aci-dynamicsnav
[app-wp]: https://github.com/Azure/azure-quickstart-templates/tree/master/application-workloads/wordpress/aci-wordpress
[az-files]: https://github.com/Azure/azure-quickstart-templates/tree/master/quickstarts/microsoft.containerinstance/aci-storage-file-share
[net-publicip]: https://github.com/Azure/azure-quickstart-templates/tree/master/quickstarts/microsoft.containerinstance/aci-linuxcontainer-public-ip
[net-udp]: https://github.com/Azure/azure-quickstart-templates/tree/master/quickstarts/microsoft.containerinstance/aci-udp
[net-vnet]: https://github.com/Azure/azure-quickstart-templates/tree/master/quickstarts/microsoft.containerinstance/aci-vnet
[repo]: https://github.com/Azure/azure-quickstart-templates
[vol-emptydir]: https://github.com/Azure/azure-quickstart-templates/tree/master/quickstarts/microsoft.containerinstance/aci-linuxcontainer-volume-emptydir
[vol-gitrepo]: https://github.com/Azure/azure-quickstart-templates/tree/master/quickstarts/microsoft.containerinstance/aci-linuxcontainer-volume-gitrepo
[vol-secret]: https://github.com/Azure/azure-quickstart-templates/tree/master/quickstarts/microsoft.containerinstance/aci-linuxcontainer-volume-secret

<!-- LINKS - Internal -->
[deploy-cli]: ../azure-resource-manager/templates/deploy-cli.md
[deploy-portal]: ../azure-resource-manager/templates/deploy-portal.md
[deploy-powershell]: ../azure-resource-manager/templates/deploy-powershell.md
[deploy-rest]: ../azure-resource-manager/templates/deploy-rest.md
[ref]: /azure/templates/microsoft.containerinstance/containergroups
