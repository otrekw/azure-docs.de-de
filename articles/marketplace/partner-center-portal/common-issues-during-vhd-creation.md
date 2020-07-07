---
title: Allgemeine Probleme bei der VHD-Erstellung (FAQ)
description: Enthält häufig gestellte Fragen zu häufigen Problemen beim Erstellen einer virtuellen Festplatte (VHD).
author: emuench
ms.author: mingshen
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: guide
ms.date: 04/09/2020
ms.openlocfilehash: e96118e3c570faaaffb4cc7ef64e6d2d0e075e47
ms.sourcegitcommit: 845a55e6c391c79d2c1585ac1625ea7dc953ea89
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2020
ms.locfileid: "85954175"
---
# <a name="common-issues-during-vhd-creation"></a>Allgemeine Probleme bei der VHD-Erstellung

In diesem Artikel mit häufig gestellten Fragen (FAQs) erhalten Sie Informationen zu Problemen, die auftreten können, wenn Sie eine virtuelle Festplatte (VHD) für Ihr Angebot für virtuelle Azure-Computer erstellen.

## <a name="how-do-i-create-a-vm-from-the-azure-portal-using-a-vhd-in-premium-storage"></a>Wie erstelle ich eine VM über das Azure-Portal, indem ich eine VHD in Premium-Speicher verwende?

Der Azure Marketplace verfügt derzeit nicht über die Unterstützung für die Erstellung von VM-Angeboten aus Images, die sich im verwalteten Speicher oder in Azure Storage Premium befinden. Weitere Informationen finden Sie unter [Azure Managed Disks – Übersicht](../../virtual-machines/windows/managed-disks-overview.md).

## <a name="can-i-use-generation-2-vms-for-offers"></a>Kann ich virtuelle Computer der 2. Generation für Angebote verwenden?

Nein. Es werden nur VHDs der 1. Generation unterstützt. Wir arbeiten aber derzeit mit dem Microsoft Azure Platform Team zusammen an der Unterstützung für VMs der 2. Generation. Weitere Informationen finden Sie unter [Sollte ich einen virtuellen Computer der Generation 1 oder 2 in Hyper-V erstellen?](https://docs.microsoft.com/windows-server/virtualization/hyper-v/plan/should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v).

## <a name="how-do-i-change-the-name-of-the-host"></a>Wie ändere ich den Namen des Hosts?

Das ist nicht möglich. Nachdem ein virtueller Computer erstellt wurde, können Benutzer den Hostnamen nicht mehr aktualisieren (gilt auch für Besitzer).

## <a name="how-do-i-reset-the-remote-desktop-service-or-its-sign-in-password"></a>Wie kann ich den Remotedesktopdienst bzw. das zugehörige Kennwort für die Anmeldung zurücksetzen?

In diesen Artikeln wird beschrieben, wie Sie das Zurücksetzen des Remotedesktopdiensts für Windows- und Linux-basierte VMs durchführen:

* [Zurücksetzen des Remotedesktopdiensts oder seines Anmeldekennworts in einer Windows-VM](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-reset-rdp/)
* [Zurücksetzen eines Kennworts oder eines SSH-Schlüssels für einen virtuellen Linux-Computer, Beheben von SSH-Konfigurationsproblemen und Überprüfen der Datenträgerkonsistenz mithilfe der VMAccess-Erweiterung](https://azure.microsoft.com/documentation/articles/virtual-machines-linux-classic-reset-access/)

## <a name="how-do-i-generate-new-ssh-certificates"></a>Wie kann ich neue SSH-Zertifikate generieren?

Die Generierung von Zertifikaten ist unter [Azure-VM-Imagezertifizierung](https://aks.ms/CertifyVMimage) beschrieben.

## <a name="how-do-i-configure-a-virtual-private-network-vpn-to-work-with-my-vms"></a>Wie kann ich ein virtuelles privates Netzwerk (VPN) für VMs konfigurieren?

Bei Verwendung des Azure Resource Manager-Bereitstellungsmodells haben Sie drei Optionen:

* [Erstellen eines routenbasierten VPN-Gateways mit dem Azure-Portal](../../vpn-gateway/create-routebased-vpn-gateway-portal.md)
* [Erstellen eines routenbasierten VPN-Gateways mit Azure PowerShell](../../vpn-gateway/create-routebased-vpn-gateway-powershell.md)
* [Erstellen eines routenbasierten VPN-Gateways mithilfe von CLI](../../vpn-gateway/create-routebased-vpn-gateway-cli.md)

## <a name="what-are-microsoft-support-policies-for-running-microsoft-server-software-on-azure-based-vms"></a>Was sind Microsoft-Supportrichtlinien für die Ausführung von Microsoft-Serversoftware auf Azure-basierten VMs?

Ausführliche Informationen finden Sie unter [Microsoft-Server-Software-Support für virtuelle Maschinen von Microsoft Azure](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines).

## <a name="do-virtual-machines-have-unique-identifiers-associated-with-them"></a>Werden virtuellen Computern eindeutige Bezeichner zugeordnet?

Ja. Dies ist der Fall, wenn sie unter Azure gehostet werden. Azure weist für jede neu erstellte VM-Ressource einen eindeutigen Bezeichner zu, der als [eindeutige ID des virtuellen Azure-Computers](https://blogs.msdn.microsoft.com/wasimbloch/2016/10/20/azure-virtual-machine-unique-id/) bezeichnet wird. Weitere Informationen finden Sie im Artikel zur eindeutigen ID für virtuelle Azure-Computer. Sie können diesen Bezeichner auch über die [Listen-API](https://docs.microsoft.com/rest/api/compute/virtualmachines/list) abrufen.

## <a name="in-a-vm-how-do-i-manage-the-custom-script-extension-in-the-startup-task"></a>Wie kann ich auf einem virtuellen Computer die benutzerdefinierte Skripterweiterung in der Startaufgabe verwalten?

Informationen dazu, wie Sie die benutzerdefinierte Skripterweiterung mit dem Azure PowerShell-Modul verwenden, zu Azure Resource Manager-Vorlagen und zu Problembehandlungsschritten für Windows-Systeme finden Sie unter [CustomScript-Erweiterung für Windows](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-extensions-customscript/).

## <a name="are-32-bit-applications-or-services-supported-in-azure-marketplace"></a>Werden 32-Bit-Anwendungen oder -Dienste für Azure Marketplace unterstützt?

Im Allgemeinen nicht. Bei allen unterstützten Betriebssystemen und Standarddiensten für Azure-VMs handelt es sich um 64-Bit-Versionen. Die meisten 64-Bit-Betriebssysteme unterstützen aus Gründen der Abwärtskompatibilität zwar 32-Bit-Versionen von Anwendungen, aber die Nutzung von 32-Bit-Anwendungen für Ihre VM-Lösung wird nicht unterstützt, und wir raten daher dringend von der Nutzung ab. Erstellen Sie Ihre Anwendung als 64-Bit-Projekt neu.

Weitere Informationen und Beispiele finden Sie in diesen Artikeln:

* [Running 32-bit applications](https://docs.microsoft.com/windows/desktop/WinProg64/running-32-bit-applications) (Ausführen von 32-Bit-Anwendungen)
* [Unterstützung für 32-Bit-Betriebssysteme auf virtuellen Azure-Computern](https://support.microsoft.com/help/4021388/support-for-32-bit-operating-systems-in-azure-virtual-machines)
* [Microsoft-Server-Software-Support für virtuelle Maschinen von Microsoft Azure](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines)

## <a name="error-vhd-is-already-registered-with-image-repository-as-the-resource"></a>Error: Die virtuelle Festplatte ist im Imagerepository bereits als Ressource registriert.

Bei jeder versuchten Erstellung eines Images aus virtuellen Festplatten erhalte ich in Azure PowerShell eine Fehlermeldung mit dem Hinweis, dass die virtuelle Festplatte im Imagerepository bereits als Ressource registriert ist. Ich habe vorher weder ein Image erstellt, noch habe ich ein Image mit diesem Namen in Azure gefunden. Wie löse ich dieses Problem?

Dieses Problem tritt normalerweise auf, wenn Sie eine VM aus einer VHD erstellt haben, die mit einer Sperre versehen ist. Stellen Sie sicher, dass über diese virtuelle Festplatte kein virtueller Computer zugeordnet wird, und wiederholen Sie anschließend den Vorgang. Öffnen Sie ein Supportticket, falls dieses Problem weiterhin besteht. Weitere Informationen finden Sie unter [Support für das Programm „Kommerzieller Marketplace“ im Partner Center](support.md).
