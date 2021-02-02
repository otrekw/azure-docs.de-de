---
title: Grundlegendes zu Azure Cloud Services (erweiterter Support)
description: Enthält eine Beschreibung der untergeordneten Elemente des NetworkConfiguration-Elements der Dienstkonfigurationsdatei, mit der die Virtual Network- und DNS-Werte angegeben werden.
ms.topic: article
ms.service: cloud-services-extended-support
author: gachandw
ms.author: gachandw
ms.reviewer: mimckitt
ms.date: 10/13/2020
ms.custom: ''
ms.openlocfilehash: 969f60cd92e8c1cbe93f1646cccd08c942ad9923
ms.sourcegitcommit: 3c3ec8cd21f2b0671bcd2230fc22e4b4adb11ce7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/25/2021
ms.locfileid: "98762821"
---
# <a name="about-azure-cloud-services-extended-support"></a>Grundlegendes zu Azure Cloud Services (erweiterter Support)

> [!IMPORTANT]
> Cloud Services (erweiterter Support) befindet sich derzeit in der öffentlichen Vorschauphase.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Cloud Services (erweiterter Support) ist ein neues Bereitstellungsmodell für das  [Azure Cloud Services](https://azure.microsoft.com/services/cloud-services/)-Produkt. Es basiert auf  [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/management/overview) und befindet sich derzeit in der öffentlichen Vorschauphase. Die Hauptvorteile von Cloud Services (erweiterter Support) sind die regionale Resilienz sowie die Featureparität mit Azure Cloud Services bei der Bereitstellung mit Azure Service Manager. Darüber hinaus sind einige ARM-Funktionen vorhanden, z. B. rollenbasierte Zugriffssteuerung (RBAC), Tags, Richtlinien und Unterstützung von Bereitstellungsvorlagen.  

Aufgrund dieser Änderung wird das auf Azure Service Manager basierende Bereitstellungsmodell für Cloud Services in [Cloud Services (klassisch)](../cloud-services/cloud-services-choose-me.md) umbenannt. Sie haben weiterhin die Möglichkeit, Ihre Web- und Cloudanwendungen und -dienste zu entwickeln und schnell bereitzustellen. Sie können Ihre Infrastruktur für Clouddienste bedarfsabhängig skalieren und sicherstellen, dass die Leistung Ihrer Anwendungen ausreicht, während gleichzeitig die Kosten reduziert werden.  

## <a name="what-does-not-change"></a>Nicht von der Änderung betroffene Bereiche 
- Sie erstellen den Code, definieren die Konfigurationen und führen die Bereitstellung in Azure durch. Azure richtet die Compute-Umgebung ein, führt Ihren Code aus und übernimmt dann die Überwachung und Verwaltung für Sie.
- Bei Cloud Services (erweiterter Support) werden auch zwei Arten von Rollen unterstützt: [Web und Worker](../cloud-services/cloud-services-choose-me.md). Es wurden keine Änderungen am Entwurf, an der Architektur oder den Komponenten von Web- und Workerrollen vorgenommen. 
- Die drei Komponenten eines Clouddiensts, also die Dienstdefinition (.csdef), die Dienstkonfiguration (.cscfg) und das Dienstpaket (.cspkg), sind weiterhin ohne Änderung des [Formats](cloud-services-model-and-package.md) vorhanden. 
- Es müssen keine Änderungen am Runtimecode vorgenommen werden, da die Datenebene identisch ist und nur die Steuerungsebene geändert wurde. 
- Ausrichtung der Releases von Azure-Gastbetriebssystemen und der zugehörigen Updates auf Cloud Services (klassisch)
- Keine Änderung des zugrunde liegenden Updateprozesses in Bezug auf Upgradedomänen, Durchführung des Upgrades, Rollback und zulässige Dienständerungen während eines Updates

## <a name="changes-in-deployment-model"></a>Änderungen am Bereitstellungsmodell

Für die Bereitstellung von Cloud Services (erweiterter Support) sind nur geringfügige Änderungen an der Dienstkonfiguration (.cscfg) und der Dienstdefinition (.csdef) erforderlich. Es müssen keine Änderungen am Runtimecode vorgenommen werden. Es ist aber erforderlich, die Bereitstellungsskripts zu aktualisieren, um die neuen Azure Resource Manager-basierten APIs aufzurufen. 

:::image type="content" source="media/overview-image-1.png" alt-text="Abbildung: Konfiguration von Cloud Service (klassisch) mit hinzugefügtem Vorlagenabschnitt":::

Hauptunterschiede zwischen Cloud Services (klassisch) und Cloud Services (erweiterter Support) in Bezug auf die Bereitstellung: 

- Für Azure Resource Manager-Bereitstellungen werden [ARM-Vorlagen](https://docs.microsoft.com/azure/azure-resource-manager/templates/overview) verwendet. Hierbei handelt es sich um eine JSON-Datei (JavaScript Object Notation), in der die Infrastruktur und die Konfiguration für Ihr Projekt definiert sind. Für die Vorlage wird deklarative Syntax verwendet. Hiermit können Sie angeben, was Sie bereitstellen möchten, ohne dass Sie die Folge der Programmierbefehle für die Erstellung schreiben müssen. Die Datei für die Dienstkonfiguration und die Dienstdefinition muss beim Bereitstellen von Cloud Services (erweiterter Support) jeweils mit der [ARM-Vorlage](https://docs.microsoft.com/azure/azure-resource-manager/templates/overview) konsistent sein. Dies kann erreicht werden, indem entweder die [manuelle Erstellung der ARM-Vorlage](deploy-template.md) gewählt wird oder [PowerShell](deploy-powershell.md), das [Portal](deploy-portal.md) und [Visual Studio](deploy-visual-studio.md) verwendet werden.  

- Kunden müssen [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/general/overview) nutzen, um [Zertifikate in Cloud Services (erweiterter Support) zu verwalten](certificates-and-key-vault.md). Mit Azure Key Vault können Sie die Anmeldeinformationen von Anwendungen, z. B. Geheimnisse, Schlüssel und Zertifikate, in einem zentralen und geschützten Cloudrepository sicher speichern und verwalten. Ihre Anwendungen können sich zur Laufzeit bei Key Vault authentifizieren, um die Anmeldeinformationen abzurufen. 

- Alle Ressourcen, die über [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/templates/overview) bereitgestellt werden, müssen sich in einem virtuellen Netzwerk befinden. Virtuelle Netzwerke und Subnetze werden in Azure Resource Manager erstellt, indem vorhandene Azure Resource Manager-APIs verwendet werden. Beim Bereitstellen von Cloud Services (erweiterter Support) muss im Abschnitt „NetworkConfiguration“ der CSCFG-Datei dann darauf verwiesen werden.   

- Bei jeder Instanz von Cloud Services (erweiterter Support) handelt es sich um eine unabhängige Bereitstellung. Für Cloud Services (erweiterter Support) werden für einen Clouddienst nicht mehrere Slots unterstützt.  
    - Die Funktion für den VIP-Austausch (VIP Swap<sup>*</sup>) kann genutzt werden, um zwischen zwei Instanzen von Cloud Services (erweiterter Support) zu wechseln. Um eine neue Version eines Clouddiensts zu testen und zu stagen, stellen Sie einen Clouddienst (erweiterter Support) bereit und geben dafür an, dass der VIP-Austausch mit einem anderen Clouddienst vom Typ „Erweiterter Support“ möglich ist.  

- Die DNS-Bezeichnung (Domain Name Service) ist für Cloud Services (erweiterter Support) optional. In Azure Resource Manager ist die DNS-Bezeichnung eine Eigenschaft der öffentlichen IP-Ressource, die dem Clouddienst zugeordnet ist. 


<sup>*</sup> Während der öffentlichen Vorschauphase ist die Funktion für den VIP-Austausch für Cloud Services (erweiterter Support) nicht verfügbar.  

## <a name="migration-to-azure-resource-manager"></a>Migration zu Azure Resource Manager

In Cloud Services (erweiterter Support) gibt es zwei Möglichkeiten, wie Sie die Migration von [Azure Service Manager](https://docs.microsoft.com/powershell/azure/servicemanagement/overview?view=azuresmps-4.0.0&preserve-view=true ) zu [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/management/overview) durchführen können. 
1) Kunden stellen Clouddienste direkt in Azure Resource Manager bereit und löschen anschließend den alten Clouddienst in Azure Service Manager. 
2) Bei der direkten Migration wird das Verfahren unterstützt, bei dem Cloud Services (klassisch) ohne bzw. nur mit minimaler Ausfallzeit zu Cloud Services (erweiterter Support) migriert wird. 

### <a name="additional-migration-options"></a>Zusätzliche Migrationsoptionen

Bei der Evaluierung von Plänen für die Migration von Cloud Services (klassisch) zu Cloud Services (erweiterter Support) kann es auch ratsam sein, weitere Azure-Dienste zu evaluieren, z. B.: [Virtual Machine Scale Sets](https://docs.microsoft.com/azure/virtual-machine-scale-sets/overview), [App Service](https://docs.microsoft.com/azure/app-service/overview), [Azure Kubernetes Service](https://docs.microsoft.com/azure/aks/intro-kubernetes) und [Azure Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-overview). Diese Dienste verfügen weiterhin über zusätzliche Funktionen, während für Cloud Services (erweiterter Support) weitestgehend die Featureparität mit Cloud Services (klassisch) beibehalten wird. 

Je nach Anwendung ist der Aufwand für die Umstellung auf Azure Resource Manager für Cloud Services (erweiterter Support) unter Umständen deutlich geringer als bei anderen Optionen. Falls Ihre Anwendung nicht weiterentwickelt wird, ist Cloud Services (erweiterter Support) eine gute Option, weil Sie einen kurzen Migrationspfad nutzen können. Wenn Ihre Anwendung dagegen ständig weiterentwickelt wird und moderne Features benötigt, sollten Sie andere Azure-Dienste erkunden, die Ihre derzeitigen und zukünftigen Anforderungen ggf. besser erfüllen. 

## <a name="next-steps"></a>Nächste Schritte
- Informieren Sie sich über die [Bereitstellungsvoraussetzungen](deploy-prerequisite.md) für Cloud Services (erweiterter Support).
- Stellen Sie eine Instanz von Cloud Services (erweiterter Support) über das [Azure-Portal](deploy-portal.md), mit [PowerShell](deploy-powershell.md), einer [Vorlage](deploy-template.md) oder [Visual Studio](deploy-visual-studio.md) bereit.
- Sehen Sie sich die [häufig gestellten Fragen](faq.md) zu Cloud Services (erweiterter Support) an.
