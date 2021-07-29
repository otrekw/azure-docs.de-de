---
title: Die lokale Azure Resource Manager-Instanz auf einem Azure Stack Edge Pro-GPU-Gerät
description: In diesem Artikel finden Sie eine Übersicht über die lokale Azure Resource Manager-Instanz auf Ihrem Azure Stack Edge Gerät.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: overview
ms.date: 06/30/2021
ms.author: alkohli
ms.openlocfilehash: 8d200195540f92c88ae3dc93737ea09a461e6c26
ms.sourcegitcommit: 98308c4b775a049a4a035ccf60c8b163f86f04ca
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/30/2021
ms.locfileid: "113114485"
---
# <a name="what-is-local-azure-resource-manager-on-azure-stack-edge"></a>Was ist eine lokale Azure Resource Manager-Instanz in Azure Stack Edge?

[!INCLUDE [applies-to-GPU-and-pro-r-and-mini-r-skus](../../includes/azure-stack-edge-applies-to-gpu-pro-r-mini-r-sku.md)]

Azure Resource Manager bietet eine Verwaltungsebene, die das Erstellen, Aktualisieren und Löschen von Ressourcen in Ihrem Azure-Abonnement ermöglicht. Azure Stack Edge-Geräte unterstützen dieselben Azure Resource Manager-APIs zum Erstellen, Aktualisieren und Löschen von VMs in einem lokalen Abonnement. Dadurch können Sie das Gerät in Übereinstimmung mit der Cloud verwalten. 

Dieser Artikel bietet eine Übersicht über die lokale Azure Resource Manager-Instanz, die zum Herstellen einer Verbindung mit den lokalen APIs auf Ihren Azure Stack Edge-Geräten verwendet werden kann.

## <a name="about-local-azure-resource-manager"></a>Informationen zur lokalen Azure Resource Manager-Instanz

Die lokale Azure Resource Manager-Instanz stellt eine konsistente Verwaltungsschicht für alle Aufrufe des Azure Stack Edge-Geräts über Resource Manager-Vorlagen bereit. Die Vorteile einer lokalen Azure Resource Manager-Instanz werden in den folgenden Abschnitten erläutert.

#### <a name="consistent-management-layer"></a>Einheitliche Verwaltungsebene

Die lokale Azure Resource Manager-Instanz bietet eine konsistente Verwaltungsebene, um die APIs des Azure Stack Edge-Geräts aufzurufen und Vorgänge auszuführen, z. B. Erstellen, Aktualisieren und Löschen von VMs. 

1. Wenn Sie eine Anforderung von REST-APIs oder SDKs senden, empfängt die lokale Azure Resource Manager-Instanz diese Anforderung auf dem Gerät. 
1. Die lokale Azure Resource Manager-Instanz verwendet den Sicherheitstokendienst (Security Token Service, STS), um die Anforderung zu authentifizieren und zu autorisieren. STS wird für die Erstellung, Validierung, Erneuerung und den Abbruch von Sicherheitstoken verwendet. Der STS erstellt beide Typen von Sicherheitstoken: Zugriffstoken und Aktualisierungstoken. Diese Token werden für die fortlaufende Kommunikation zwischen dem Gerät und den Clients verwendet, die über die lokale Azure Resource Manager-Instanz auf das Gerät zugreifen.
1. Resource Manager sendet dann die Anforderung an die Ressourcenanbieter, die die angeforderte Aktion ausführen.   

    Folgende Ressourcenanbieter sind vorab bei Azure Stack Edge registriert:

    - **Compute:** Mit `Microsoft.Compute` oder dem Computeressourcenanbieter können Sie VMs auf Ihrem Azure Stack Edge-Gerät bereitstellen. Der Computeressourcenanbieter bietet die Möglichkeit, VMs und VM-Erweiterungen zu erstellen. 

    - **Netzwerk:** Mit `Microsoft.Network` oder dem Netzwerkressourcenanbieter können Sie Ressourcen wie Netzwerkschnittstellen und virtuelle Netzwerke erstellen.

    - **Speicher:** `Microsoft.Storage` oder der Speicherressourcenanbieter stellt einen mit Azure konsistenten Blobspeicherdienst und eine Key Vault-Kontoverwaltung mit Funktionen zur Verwaltung und Überwachung von Geheimnissen (z. B. Kennwörter und Zertifikate) bereit.  
    
    - **Datenträger:** Mit `Microsoft.Disks` oder dem Datenträgerressourcenanbieter können Sie verwaltete Datenträger erstellen, die zum Erstellen von VMs verwendet werden können.

    Ressourcen sind verwaltbare Elemente, die über Azure Stack Edge verfügbar sind. Die Ressourcenanbieter sind für die Bereitstellung der Ressourcen verantwortlich. VMs, Speicherkonten und virtuelle Netzwerke sind Beispiele für Ressourcen. Außerdem stellt der Computeressourcenanbieter die VM-Ressource bereit.    

Da alle Anforderungen von der gleichen API verarbeitet werden, stehen in allen Tools konsistente Ergebnisse und Funktionen zur Verfügung.

Die folgende Abbildung zeigt den Mechanismus zur Verarbeitung aller API-Anforderungen und welche Rolle die lokale Azure Resource Manager-Instanz bei der Bereitstellung einer konsistenten Verwaltungsschicht für die Verarbeitung dieser Anforderungen spielt.

![Abbildung von Azure Resource Manager](media/azure-stack-edge-gpu-connect-resource-manager/edge-device-flow.svg)


#### <a name="use-of-resource-manager-templates"></a>Verwenden von Resource Manager-Vorlagen

Ein weiterer wichtiger Vorteil von Azure Resource Manager besteht darin, dass Sie Resource Manager-Vorlagen verwenden können. Dies sind JSON-Dateien (JavaScript Object Notation) in einer deklarativen Syntax, mit denen die Ressourcen konsistent und wiederholt bereitgestellt werden können. Bei der deklarativen Syntax können Sie beispielsweise „Here is what I intend to create“ (Dies möchte ich erstellen) eingeben, ohne dafür die Folge der Programmierbefehle für die Erstellung schreiben zu müssen. Sie können diese deklarativen Syntaxvorlagen z. B. verwenden, um VMs auf Ihren Azure Stack Edge-Geräten bereitzustellen. Ausführliche Informationen finden Sie unter [Bereitstellen von VMs auf Ihrem Azure Stack Edge Pro-GPU-Gerät über Vorlagen](azure-stack-edge-gpu-deploy-virtual-machine-templates.md).

## <a name="connect-to-the-local-azure-resource-manager"></a>Herstellen einer Verbindung mit der lokalen Azure Resource Manager-Instanz auf Ihrem Gerät

Um VMs, Freigaben oder Speicherkonten auf Ihrem Azure Stack Edge-Gerät zu erstellen, müssen Sie die entsprechenden Ressourcen erstellen. Für VMs benötigen Sie z. B. Ressourcen wie Netzwerkschnittstelle, Betriebssystem und Datenträger auf der VM, die von den Netzwerk-, Datenträger- und Speicherressourcenanbietern bereitgestellt werden. 

Um die Erstellung von Ressourcen von den Ressourcenanbietern anzufordern, müssen Sie zunächst eine Verbindung mit der lokalen Azure Resource Manager-Instanz herstellen. Ausführliche Schritte finden Sie unter [Herstellen einer Verbindung mit Azure Resource Manager auf Ihrem Azure Stack Edge-Gerät](azure-stack-edge-gpu-connect-resource-manager.md).

Wenn Sie zum ersten Mal eine Verbindung mit Azure Resource Manager herstellen, müssen Sie auch Ihr Kennwort zurücksetzen. Ausführliche Schritte finden Sie unter [Zurücksetzen Ihres Azure Resource Manager-Kennworts](azure-stack-edge-gpu-set-azure-resource-manager-password.md).


## <a name="azure-resource-manager-endpoints"></a>Azure Resource Manager-Endpunkte

Die lokale Azure Resource Manager-Instanz und die STS-Dienste werden auf Ihrem Gerät ausgeführt und sind über bestimmte Endpunkten erreichbar. In der folgenden Tabelle sind die verschiedenen Endpunkte, die von diesen Diensten auf dem Gerät verfügbar gemacht werden, die unterstützten Protokolle und die Ports für den Zugriff auf diese Endpunkte zusammengefasst. 

| # | Endpunkt | Unterstützte Protokolle | Verwendeter Port | Syntaxelemente |
| --- | --- | --- | --- | --- |
| 1. | Azure Resource Manager | https | 443 | Herstellen einer Verbindung mit Azure Resource Manager zur Automatisierung |
| 2. | Sicherheitstokendienst | https | 443 | Authentifizieren über Zugriffs- und Aktualisierungstoken |


## <a name="next-steps"></a>Nächste Schritte

[Herstellen einer Verbindung mit der lokalen Azure Resource Manager-Instanz auf Ihrem Azure Stack Edge Pro-GPU-Gerät](azure-stack-edge-gpu-deploy-virtual-machine-powershell.md)
