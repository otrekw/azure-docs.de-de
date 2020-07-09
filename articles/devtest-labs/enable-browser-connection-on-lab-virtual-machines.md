---
title: Aktivieren der Browserverbindung auf Azure DevTest Labs-VMs | Microsoft-Dokumentation
description: DevTest Labs und Azure Bastion sind jetzt integriert. Als Besitzer des Labs können Sie den Zugriff auf alle Lab-VMs über einen Browser aktivieren.
ms.topic: article
ms.date: 06/26/2020
ms.openlocfilehash: 9b901d36907c8d2bbae866c38bbf1c2554e0f1da
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85482937"
---
# <a name="enable-browser-connection-on-lab-virtual-machines"></a>Aktivieren der Browserverbindung auf Lab-VMs 

DevTest Labs und [Azure Bastion](https://docs.microsoft.com/azure/bastion/) sind integriert, sodass Sie über einen Browser eine Verbindung mit Ihren virtuellen Computern herstellen können. Zuerst müssen Sie die Browserverbindung auf Lab-VMs aktivieren.

Als Besitzer eines Labs können Sie den Zugriff auf alle Lab-VMs über einen Browser ermöglichen. Dazu sind kein zusätzlicher Client oder Agent und keine zusätzliche Software erforderlich. Azure Bastion bietet direkt im Azure-Portal sichere und nahtlose RDP- und SSH-Verbindungen mit Ihren virtuellen Computern über TLS. Beim Herstellen einer Verbindung über Azure Bastion benötigen Ihre virtuellen Computer keine öffentliche IP-Adresse. Weitere Informationen finden Sie unter [Was ist Azure Bastion?](../bastion/bastion-overview.md)

> [!NOTE]
> Das Aktivieren der Browserverbindung auf Lab-VMs ist in der Vorschauphase.

Dieser Artikel erläutert, wie Sie die Browserverbindung auf Lab-VMs aktivieren.

## <a name="prerequisites"></a>Voraussetzungen 
Sie können entweder einen Bastion-Host im virtuellen Netzwerk Ihres bestehenden Labs bereitstellen **(ODER)** Ihr Lab mit einem von Bastion konfigurierten VNet verbinden. 

Informationen zum Bereitstellen eines Bastion-Hosts in einem VNet finden Sie unter [Erstellen eines Azure Bastion-Hosts (Vorschau)](../bastion/bastion-create-host-portal.md). Wenn Sie den Bastion-Host erstellen, wählen Sie das virtuelle Netzwerk des Labs aus. 

Um zu erfahren, wie Sie Ihr Lab mit einem von Bastion konfigurierten VNet verbinden können, siehe [Konfigurieren eines virtuellen Netzwerks in Azure DevTest Labs](devtest-lab-configure-vnet.md). Wählen Sie das VNet aus, in dem der Bastion-Host und das **AzureBastionSubnet** bereitgestellt sind. Ausführliche Erläuterung der Schritte: 

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Wählen Sie im linken Navigationsmenü die Option **Alle Dienste** aus. 
1. Wählen Sie in der Liste **DevTest Labs** aus. 
1. Wählen Sie in der Liste der Labs Ihr *Lab* aus. 

    > [!NOTE]
    > Azure Bastion befindet sich derzeit in der Vorschauphase. Die Lösung ist auf folgende Regionen beschränkt: USA, Westen; USA, Osten; Europa, Westen; USA, Süden-Mitte; Australien, Osten und Japan, Osten. Erstellen Sie also ein Lab in einer dieser Regionen, wenn sich ihr Lab nicht in einer davon befindet. 
1. Wählen Sie im Abschnitt **Einstellungen** die Option **Konfiguration und Richtlinien** aus. 
1. Wählen Sie **Virtuelle Netzwerke** aus.
1. Klicken Sie auf der Symbolleiste auf **Hinzufügen**. 
1. Wählen Sie das **VNet** aus, in dem der Bastion-Host bereitgestellt ist. 
1. Wählen Sie das Subnetz aus: **AzureBastionSubnet**. 

    ![Subnet](./media/enable-browser-connection-lab-virtual-machines/subnet.png)
1. Wählen Sie in die Option **Beim Erstellen virtueller Computer verwenden** aus. 
1. Wählen Sie auf der Symbolleiste **Speichern** aus. 
1. Wenn Sie ein altes VNet für das Lab haben, entfernen Sie es, indem Sie * *...* und **Entfernen** auswählen. 

## <a name="enable-browser-connection"></a>Aktivieren der Browserverbindung 

Sobald Sie ein mit Bastion konfiguriertes VNet im Lab haben, können Sie als Besitzer des Labs die Browserverbindung auf Lab-VMs aktivieren.

Gehen Sie wie folgt vor, um die Browserverbindung auf Lab-VMs zu aktivieren:

1. Navigieren Sie im Azure-Portal zu Ihrem *Lab*.
1. Wählen Sie **Konfiguration und Richtlinien** aus.
1. Wählen Sie unter **Einstellungen** die Option **Browser verbinden (Vorschau)** aus.

![Aktivieren der Browserverbindung](./media/enable-browser-connection-lab-virtual-machines/browser-connect.png)

## <a name="next-steps"></a>Nächste Schritte
Im folgenden Artikel erfahren Sie, wie Sie mithilfe eines Browsers eine Verbindung mit Ihren virtuellen Computern herstellen: [Herstellen einer Verbindung mit Ihren virtuellen Computern über einen Browser](connect-virtual-machine-through-browser.md)