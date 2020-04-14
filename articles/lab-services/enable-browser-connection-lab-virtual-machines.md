---
title: Aktivieren der Browserverbindung auf Azure DevTest Labs-VMs | Microsoft-Dokumentation
description: DevTest Labs und Azure Bastion sind jetzt integriert. Als Besitzer des Labs können Sie den Zugriff auf alle Lab-VMs über einen Browser aktivieren.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: tanmayeekamath
manager: femila
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/09/2019
ms.author: takamath
ms.openlocfilehash: 074d512767769fe434cd4a6d4f4e8e6a88f7ed2a
ms.sourcegitcommit: 980c3d827cc0f25b94b1eb93fd3d9041f3593036
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/02/2020
ms.locfileid: "80549117"
---
# <a name="enable-browser-connection-on-lab-virtual-machines"></a>Aktivieren der Browserverbindung auf Lab-VMs 
DevTest Labs und [Azure Bastion](https://docs.microsoft.com/azure/bastion/) sind integriert, sodass Sie über einen Browser eine Verbindung mit Ihren virtuellen Computern herstellen können. Zuerst müssen Sie die Browserverbindung auf Lab-VMs aktivieren.

Als Besitzer eines Labs können Sie den Zugriff auf alle Lab-VMs über einen Browser ermöglichen. Dazu sind kein zusätzlicher Client oder Agent und keine zusätzliche Software erforderlich. Azure Bastion bietet direkt im Azure-Portal sichere und nahtlose RDP- und SSH-Verbindungen mit Ihren virtuellen Computern über TLS. Beim Herstellen einer Verbindung über Azure Bastion benötigen Ihre virtuellen Computer keine öffentliche IP-Adresse. Weitere Informationen finden Sie unter [Was ist Azure Bastion?](../bastion/bastion-overview.md)


Dieser Artikel erläutert, wie Sie die Browserverbindung auf Lab-VMs aktivieren.

## <a name="prerequisites"></a>Voraussetzungen 
Sie können einen Bastionhost im virtuellen Netzwerk Ihres bestehenden Labs bereitstellen **ODER** Ihr Lab mit einem mit Bastion konfigurierten virtuellen Netzwerk verbinden. 

Informationen zum Bereitstellen eines Bastionhosts in einem virtuellen Netzwerk finden Sie unter [Erstellen eines Azure Bastion-Hosts](../bastion/bastion-create-host-portal.md). Wenn Sie den Bastion-Host erstellen, wählen Sie das virtuelle Netzwerk des Labs aus. 

Zunächst müssen Sie ein zweites Subnetz im virtuellen Bastion-Netzwerk erstellen, da in AzureBastionSubnet keine Nicht-Bastion-Ressourcen erstellt werden können. 

## <a name="create-a-second-sub-net-in-the-bastion-virtual-network"></a>Erstellen eines zweiten Subnetzes im virtuellen Bastion-Netzwerk
Sie können in einem Azure Bastion-Subnetz keine Lab-VMs erstellen. Erstellen Sie ein weiteres Subnetz innerhalb des virtuellen Bastion-Netzwerks, wie in der folgenden Abbildung dargestellt:

![Zweites Subnetz in einem virtuellen Azure Bastion-Netzwerk](./media/connect-virtual-machine-through-browser/second-subnet.png)

## <a name="enable-vm-creation-in-the-subnet"></a>Aktivieren der VM-Erstellung im Subnetz
Aktivieren Sie nun die Erstellung von VMs in diesem Subnetz über die folgenden Schritte: 

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Wählen Sie im linken Navigationsmenü die Option **Alle Dienste** aus. 
1. Wählen Sie in der Liste **DevTest Labs** aus. 
1. Wählen Sie in der Liste der Labs Ihr *Lab* aus. 

    > [!NOTE]
    > Azure Bastion ist jetzt in den folgenden Regionen allgemein verfügbar: USA, Westen; USA, Osten; Europa, Westen; USA, Süden-Mitte; Australien, Osten und Japan, Osten. Erstellen Sie also ein Lab in einer dieser Regionen, wenn sich ihr Lab nicht in einer davon befindet. 
    
1. Wählen Sie im Abschnitt **Einstellungen** die Option **Konfiguration und Richtlinien** aus. 
1. Wählen Sie **Virtuelle Netzwerke** aus.
1. Klicken Sie auf der Symbolleiste auf **Hinzufügen**. 
1. Wählen Sie das **virtuelle Netzwerk** aus, in dem der Bastionhost bereitgestellt ist. 
1. Wählen Sie das Subnetz für VMs aus, nicht das zuvor erstellte **AzureBastionSubnet**. Wenn das Subnetz nicht unten in der Liste angezeigt wird, schließen Sie die Seite, und öffnen Sie sie erneut. 

    ![Aktivieren der VM-Erstellung im Subnetz](./media/connect-virtual-machine-through-browser/enable-vm-creation-subnet.png)
1. Wählen Sie in die Option **Beim Erstellen virtueller Computer verwenden** aus. 
1. Wählen Sie auf der Symbolleiste **Speichern** aus. 
1. Wenn Sie über ein altes virtuelles Netzwerk für das Lab verfügen, entfernen Sie es, indem Sie * *...* und **Entfernen** auswählen. 

## <a name="enable-browser-connection"></a>Aktivieren der Browserverbindung 

Sobald Sie über ein mit Bastion konfiguriertes virtuelles Netzwerk im Lab verfügen, können Sie als Besitzer des Labs die Browserverbindung auf Lab-VMs aktivieren.

Gehen Sie wie folgt vor, um die Browserverbindung auf Lab-VMs zu aktivieren:

1. Navigieren Sie im Azure-Portal zu Ihrem *Lab*.
1. Wählen Sie **Konfiguration und Richtlinien** aus.
1. Wählen Sie unter **Einstellungen** die Option **Browserverbindung** aus. Wenn diese Option nicht angezeigt wird, schließen Sie die Seite **Konfigurationsrichtlinien**, und öffnen Sie sie erneut. 

    ![Aktivieren der Browserverbindung](./media/enable-browser-connection-lab-virtual-machines/browser-connect.png)

## <a name="next-steps"></a>Nächste Schritte
Im folgenden Artikel erfahren Sie, wie Sie mithilfe eines Browsers eine Verbindung mit Ihren virtuellen Computern herstellen: [Herstellen einer Verbindung mit Ihren virtuellen Computern über einen Browser](connect-virtual-machine-through-browser.md)
