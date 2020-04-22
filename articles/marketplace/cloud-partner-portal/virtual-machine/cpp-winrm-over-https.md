---
title: Windows-Remoteverwaltung über HTTPS für Azure | Azure Marketplace
description: Erläutert, wie ein in Azure gehosteter Windows-basierter virtueller Computer so konfiguriert wird, dass er mit Remotezugriff über PowerShell verwaltet werden kann.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 11/26/2018
ms.author: dsindona
ms.openlocfilehash: b2a4bb107309894a7180e0a4585cdba6f04d1bee
ms.sourcegitcommit: 530e2d56fc3b91c520d3714a7fe4e8e0b75480c8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/14/2020
ms.locfileid: "81273034"
---
# <a name="windows-remote-management-over-https"></a>Windows-Remoteverwaltung über HTTPS

> [!IMPORTANT]
> Ab dem 13. April 2020 beginnen wir mit der Umstellung der Verwaltung Ihrer Azure Virtual Machines-Angebote auf Partner Center. Nach der Migration erstellen und verwalten Sie Ihre Angebote im Partner Center. Folgen Sie den Anweisungen unter [Erstellen eines Azure Virtual Machines-Angebots](https://aka.ms/CreateAzureVMoffer), um Ihre migrierten Angebote zu verwalten.

In diesem Abschnitt wird erläutert, wie ein in Azure gehosteter Windows-basierter virtueller Computer so konfiguriert wird, dass er mit Remotezugriff über PowerShell verwaltet und bereitgestellt werden kann.  Um PowerShell-Remoting zu aktivieren, muss der virtuelle Zielcomputer einen HTTPS-Endpunkt für Windows-Remoteverwaltung (WinRM) verfügbar machen.  Weitere Informationen über PowerShell-Remoting finden Sie unter [Ausführen von Remotebefehlen](https://docs.microsoft.com/powershell/scripting/learn/remoting/running-remote-commands).  Weitere Informationen über WinRM finden Sie unter [Windows-Remoteverwaltung](https://docs.microsoft.com/windows/desktop/WinRM/portal).

Wenn Sie einen virtuellen Computer mit einer der „klassischen“ Azure-Methoden erstellt haben – entweder über das Azure Service Manager-Portal oder die veraltete [Azure Dienstverwaltungs-API](https://docs.microsoft.com/previous-versions/azure/ee460799(v=azure.100)) –, wurde er automatisch mit einem WinRM-Endpunkt konfiguriert.  Wenn Sie einen virtuellen Computer jedoch über eine der folgenden „modernen“ Azure-Methoden erstellen, wird der virtuelle Computer *nicht* für WinRM über HTTPS konfiguriert.

- Über das [Azure-Portal](https://portal.azure.com/), in der Regel aus einem genehmigten Basisimage, wie dies im Abschnitt [Erstellen einer Azure-kompatiblen VHD](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/virtual-machine/cpp-create-vhd) beschrieben ist
- [Verwenden der Azure Resource Manager-Vorlagen](https://docs.microsoft.com/azure/virtual-machines/windows/ps-template)
- Über Azure PowerShell oder über die Azure-Befehlszeilenschnittstelle.  Beispiele finden Sie unter [Schnellstart: Erstellen eines virtuellen Windows-Computers mit PowerShell in Azure](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-powershell) und [Schnellstart: Erstellen eines virtuellen Linux-Computers mit Azure CLI](https://docs.microsoft.com/azure/virtual-machines/linux/quick-create-cli).

Dieser WinRM-Endpunkt ist auch erforderlich, um das Zertifizierungs-Toolkit für das Onboarding des virtuellen Computers auszuführen, wie dies unter [Zertifizieren Ihres VM-Images](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/virtual-machine/cpp-certify-vm) beschrieben ist.

Im Gegensatz dazu werden virtuelle Linux-Computer in der Regel remote verwaltet, indem entweder die [Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/cli/azure) oder Linux-Befehle aus einer SSH-Konsole verwendet werden.  Darüber werden in Azure mehrere alternative Methoden unterstützt, um [Skripts in Ihrer Linux-VM auszuführen](https://docs.microsoft.com/azure/virtual-machines/linux/run-scripts-in-vm).  Für komplexere Szenarien gibt es eine Reihe von Automatisierungs- und Integrationslösungen, die für Windows- oder Linux-basierte virtuelle Computer verfügbar sind.


## <a name="configure-and-deploy-with-winrm"></a>Konfigurieren und Bereitstellen mit Windows-Remoteverwaltung (WinRM) Firewall

Der WinRM-Endpunkt für einen Windows-basierten virtuellen Computer kann bei dessen Entwicklung in zwei verschiedenen Phasen konfiguriert werden:

- Während der Erstellung: während der Bereitstellung eines virtuellen Computers auf einer vorhandenen virtuellen Festplatte.  Dies ist die bevorzugte Vorgehensweise für neue Angebote.  Diese Vorgehensweise erfordert die Erstellung eines Azure-Zertifikats, Verwenden von bereitgestellten Azure Resource Manager-Vorlagen und Ausführen von angepassten PowerShell-Skripts.
- Nach der Bereitstellung: auf einem vorhandenen virtuellen Computer, der in Azure gehostet wird.  Verwenden Sie diese Vorgehensweise, wenn Sie bereits eine in Azure bereitgestellte VM-Lösung haben und Windows-Remoteverwaltung für diese Lösung aktivieren müssen.  Diese Vorgehensweise erfordert manuelle Änderungen im Azure-Portal und das Ausführen eines Skripts auf dem virtuellen Zielcomputer.


## <a name="next-steps"></a>Nächste Schritte
Wenn Sie einen neuen virtuellen Computer erstellen, können Sie WinRM während des [Bereitstellens eines virtuellen Computers auf Basis der VHDs](./cpp-deploy-vm-vhd.md) aktivieren.  Andernfalls kann WinRM auf einem vorhandenen virtuellen Computer aktiviert werden.
