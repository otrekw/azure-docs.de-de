---
title: Herstellen einer Verbindung mit einer Windows-VM mit Azure Bastion | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie, wie Sie eine Verbindung mit einem virtuellen Azure-Computer mit Azure Bastion herstellen.
services: bastion
author: cherylmc
ms.service: bastion
ms.topic: conceptual
ms.date: 02/03/2020
ms.author: cherylmc
ms.openlocfilehash: 7dad6a517341f83f693e1e7e1f7d27e899e00f7e
ms.sourcegitcommit: 4f6a7a2572723b0405a21fea0894d34f9d5b8e12
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/04/2020
ms.locfileid: "76990486"
---
# <a name="connect-to-a-windows-virtual-machine-using-azure-bastion"></a>Herstellen einer Verbindung mit einem virtuellen Windows-Computer mit Azure Bastion

In diesem Artikel erfahren Sie, wie Sie mit Azure Bastion sicher und problemlos RDP auf Ihren virtuellen Windows-Computern in einem virtuellen Azure-Netzwerk durchführen können. Sie können sich direkt vom Azure-Portal aus mit einer VM verbinden. Wenn Sie Azure Bastion verwenden, benötigen VMs keinen Client, keinen Agent und auch keine zusätzliche Software. Weitere Informationen über Azure Bastion finden Sie in der [Übersicht](bastion-overview.md).

## <a name="before-you-begin"></a>Voraussetzungen

Stellen Sie sicher, dass Sie einen Azure Bastion-Host für das virtuelle Netzwerk eingerichtet haben, in dem sich die VM befindet. Weitere Informationen finden Sie unter [Erstellen eines Azure Bastion-Hosts](bastion-create-host-portal.md). Sobald der Bastion-Dienst provisioniert und in Ihrem virtuellen Netzwerk bereitgestellt wurde, können Sie ihn verwenden, um sich mit jeder VM in diesem virtuellen Netzwerk zu verbinden.

In Bastion wird vorausgesetzt, dass Sie RDP für die Verbindung mit virtuellen Windows-Computern und SSH für die Verbindung mit virtuellen Linux-Computern verwenden. Informationen zur Verbindung mit einer Linux-VM finden Sie unter [Herstellen einer Verbindung mit einer VM – Linux](bastion-connect-vm-ssh.md).

### <a name="required-roles"></a>Erforderliche Rollen
Zum Herstellen einer Verbindung sind die folgenden Rollen erforderlich:

* Rolle „Leser“ auf dem virtuellen Computer
* Rolle „Leser“ auf dem Netzwerkadapter mit privater IP-Adresse des virtuellen Computers
* Rolle „Leser“ für die Azure Bastion-Ressource

### <a name="ports"></a>Ports

Zum Herstellen einer Verbindung mit dem virtuellen Windows-Computer über RDP müssen die folgenden Ports auf Ihrer Windows-VM geöffnet sein:

* Eingehende Ports: RDP (3389)

## <a name="rdp"></a>Verbindung über RDP

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com). Navigieren Sie zu dem virtuellen Computer, mit dem Sie eine Verbindung herstellen möchten, und klicken Sie dann auf **Verbinden**. Die VM sollte ein virtueller Windows-Computer sein, wenn Sie eine RDP-Verbindung verwenden.

   ![VM-Verbindung](./media/bastion-connect-vm-rdp/connect.png)
1. Nachdem Sie auf Verbinden geklickt haben, erscheint eine Seitenleiste mit drei Registerkarten – RDP, SSH und Bastion. Wenn Bastion für das virtuelle Netzwerk bereitgestellt wurde, ist die Registerkarte Bastion standardmäßig aktiv. Wenn Sie Bastion nicht für das virtuelle Netzwerk bereitgestellt haben, können Sie auf den Link klicken, um Bastion zu konfigurieren. Konfigurationsanweisungen finden Sie unter [Konfigurieren von Bastion](bastion-create-host-portal.md).

   ![VM-Verbindung](./media/bastion-connect-vm-rdp/bastion.png)
1. Geben Sie auf der Registerkarte „Bastion“ den Benutzernamen und das Kennwort für Ihren virtuellen Computer ein, und klicken Sie dann auf **Verbinden**. Die RDP-Verbindung zu diesem virtuellen Computer über Bastion wird direkt im Azure-Portal (über HTML5) über Port 443 und den Bastion-Dienst geöffnet.

   ![VM-Verbindung](./media/bastion-connect-vm-rdp/443rdp.png)
 
## <a name="next-steps"></a>Nächste Schritte

Lesen Sie die [häufig gestellten Fragen zu Bastion](bastion-faq.md).
