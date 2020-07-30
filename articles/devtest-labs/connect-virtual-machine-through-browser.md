---
title: Herstellen einer Verbindung mit Ihren virtuellen Computern über einen Browser – Azure | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie über einen Browser eine Verbindung mit Ihren virtuellen Computern herstellen.
ms.topic: article
ms.date: 06/26/2020
ms.openlocfilehash: 352fc5fd3ff53a00d9f62966ecf21417ad898706
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/28/2020
ms.locfileid: "87288053"
---
# <a name="connect-to-your-virtual-machines-through-a-browser"></a>Herstellen einer Verbindung mit Ihren virtuellen Computern über einen Browser 

DevTest Labs und [Azure Bastion](../bastion/index.yml) sind integriert, sodass Sie über einen Browser eine Verbindung mit Ihren virtuellen Computern herstellen können. Informationen zum Aktivieren dieses Features in DevTest Labs finden Sie unter [Aktivieren der Browserverbindung auf Lab-VMs](enable-browser-connection-lab-virtual-machines.md).

Nach dem Aktivieren der *Browserverbindung* können Benutzer des Labs über einen Browser auf virtuelle Computer zugreifen.  

## <a name="create-a-lab-virtual-machine"></a>Erstellen einer Lab-VM

Sie müssen zuerst den virtuellen Labcomputer in einem VNET erstellen, für das Bastion konfiguriert ist. Wählen Sie das zweite **Subnetz** aus, das Sie erstellt haben, nicht AzureBastionSubnet. Sie können ein virtuelles Netzwerk während der Erstellung eines virtuellen Computers auswählen, indem Sie zur Registerkarte **Erweiterte Einstellungen** navigieren.

![Erstellen eines virtuellen Computers](./media/connect-virtual-machine-through-browser/create-virtual-machine.png)

## <a name="launch-virtual-machine-in-a-browser"></a>Starten des virtuellen Computers in einem Browser

Nachdem der virtuelle Computer erstellt wurde, können Sie ihn in einem Browser starten. Klicken Sie dazu auf die Schaltfläche *Browser verbinden*, und geben Sie Ihren Benutzernamen und Ihr Kennwort für die VM ein.  

![In einem Browser starten](./media/connect-virtual-machine-through-browser/browser-connect.png)

## <a name="next-steps"></a>Nächste Schritte

[Hinzufügen eines virtuellen Computers zu einem Lab in Azure DevTest Labs](devtest-lab-add-vm.md)
