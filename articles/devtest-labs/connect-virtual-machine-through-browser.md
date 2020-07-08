---
title: Herstellen einer Verbindung mit Ihren virtuellen Computern über einen Browser – Azure | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie über einen Browser eine Verbindung mit Ihren virtuellen Computern herstellen.
ms.topic: article
ms.date: 06/26/2020
ms.openlocfilehash: b1e50a15673199a4bf95956ae66e6b58d6d41d0e
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85483736"
---
# <a name="connect-to-your-virtual-machines-through-a-browser"></a>Herstellen einer Verbindung mit Ihren virtuellen Computern über einen Browser 

DevTest Labs und [Azure Bastion](https://docs.microsoft.com/azure/bastion/) sind integriert, sodass Sie über einen Browser eine Verbindung mit Ihren virtuellen Computern herstellen können. Informationen zum Aktivieren dieses Features in DevTest Labs finden Sie unter [Aktivieren der Browserverbindung auf Lab-VMs](enable-browser-connection-lab-virtual-machines.md).

Nach dem Aktivieren der *Browserverbindung* können Benutzer des Labs über einen Browser auf virtuelle Computer zugreifen.  

## <a name="create-a-lab-virtual-machine"></a>Erstellen einer Lab-VM

Sie müssen zuerst den virtuellen Labcomputer in einem VNET erstellen, für das Bastion konfiguriert ist. Wählen Sie das zweite **Subnetz** aus, das Sie erstellt haben, nicht AzureBastionSubnet. Sie können ein virtuelles Netzwerk während der Erstellung eines virtuellen Computers auswählen, indem Sie zur Registerkarte **Erweiterte Einstellungen** navigieren.

![Erstellen eines virtuellen Computers](./media/connect-virtual-machine-through-browser/create-virtual-machine.png)

## <a name="launch-virtual-machine-in-a-browser"></a>Starten des virtuellen Computers in einem Browser

Nachdem der virtuelle Computer erstellt wurde, können Sie ihn in einem Browser starten. Klicken Sie dazu auf die Schaltfläche *Browser verbinden*, und geben Sie Ihren Benutzernamen und Ihr Kennwort für die VM ein.  

![In einem Browser starten](./media/connect-virtual-machine-through-browser/browser-connect.png)

## <a name="next-steps"></a>Nächste Schritte

[Hinzufügen eines virtuellen Computers zu einem Lab in Azure DevTest Labs](devtest-lab-add-vm.md)
