---
title: Herstellen einer Verbindung mit einer Windows-VM mit Azure Bastion
description: In diesem Artikel erfahren Sie, wie Sie eine Verbindung mit einem virtuellen Azure-Computer mit Azure Bastion herstellen.
services: bastion
author: cherylmc
ms.service: bastion
ms.topic: how-to
ms.date: 02/24/2020
ms.author: cherylmc
ms.openlocfilehash: 79eb09a005f62846fc2f7e3e7b493d5e366edabc
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "84744322"
---
# <a name="connect-to-a-windows-virtual-machine-using-azure-bastion"></a>Herstellen einer Verbindung mit einem virtuellen Windows-Computer mit Azure Bastion

Mit Azure Bastion können Sie eine sichere und problemlose Verbindung mit Ihren virtuellen Computern über SSL direkt im Azure-Portal herstellen. Wenn Sie Azure Bastion verwenden, benötigen Ihre virtuellen Computer keinen Client, keinen Agent und auch keine zusätzliche Software. In diesem Artikel erfahren Sie, wie Sie eine Verbindung mit Ihren Windows-VMs herstellen können. Informationen zum Herstellen einer Verbindung mit einer Linux-VM finden Sie unter [Herstellen einer Verbindung mit einer VM mit Azure Bastion – Linux](bastion-connect-vm-ssh.md).

Azure Bastion bietet sichere Verbindungen mit allen virtuellen Computern in dem virtuellen Netzwerk, in dem der Dienst bereitgestellt wird. Durch die Verwendung von Azure Bastion wird verhindert, dass Ihre virtuellen Computer RDP- und SSH-Ports öffentlich verfügbar machen. Gleichzeitig wir weiterhin der sichere Zugriff per RDP/SSH ermöglicht. Weitere Informationen finden Sie in der [Übersicht](bastion-overview.md).

## <a name="before-you-begin"></a>Voraussetzungen

Stellen Sie sicher, dass Sie einen Azure Bastion-Host für das virtuelle Netzwerk eingerichtet haben, in dem sich die VM befindet. Sobald der Bastion-Dienst provisioniert und in Ihrem virtuellen Netzwerk bereitgestellt wurde, können Sie ihn verwenden, um sich mit jeder VM im virtuellen Netzwerk zu verbinden. Informationen zum Einrichten eines Azure Bastion-Hosts finden Sie unter [Erstellen eines Azure Bastion-Hosts](bastion-create-host-portal.md).

### <a name="required-roles"></a>Erforderliche Rollen

Zum Herstellen einer Verbindung sind die folgenden Rollen erforderlich:

* Rolle „Leser“ auf dem virtuellen Computer
* Rolle „Leser“ auf dem Netzwerkadapter mit privater IP-Adresse des virtuellen Computers
* Rolle „Leser“ für die Azure Bastion-Ressource

### <a name="ports"></a>Ports

Zum Herstellen einer Verbindung mit dem virtuellen Windows-Computer müssen die folgenden Ports auf Ihrer Windows-VM geöffnet sein:

* Eingehende Ports: RDP (3389)

## <a name="connect"></a><a name="rdp"></a>Verbinden

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com). Navigieren Sie zu dem virtuellen Computer, mit dem Sie eine Verbindung herstellen möchten, klicken Sie dann auf **Verbinden**, und wählen Sie **Bastion** aus der Dropdownliste aus.

   ![VM-Verbindung](./media/bastion-connect-vm-rdp/connect.png)
1. Nachdem Sie auf Bastion geklickt haben, wird eine Seitenleiste mit drei Registerkarten angezeigt – RDP, SSH und Bastion. Wenn Bastion für das virtuelle Netzwerk bereitgestellt wurde, ist die Registerkarte Bastion standardmäßig aktiv. Wenn Sie Bastion nicht für das virtuelle Netzwerk bereitgestellt haben, können Sie auf den Link klicken, um Bastion zu konfigurieren. Konfigurationsanweisungen finden Sie unter [Konfigurieren von Bastion](bastion-create-host-portal.md).

   ![Registerkarte „Bastion“](./media/bastion-connect-vm-rdp/bastion.png)
1. Geben Sie auf der Registerkarte „Bastion“ den Benutzernamen und das Kennwort für Ihren virtuellen Computer ein, und klicken Sie dann auf **Verbinden**. Die RDP-Verbindung zu diesem virtuellen Computer über Bastion wird direkt im Azure-Portal (über HTML5) über Port 443 und den Bastion-Dienst geöffnet.

   ![RDP-Verbindung](./media/bastion-connect-vm-rdp/443rdp.png)
 
## <a name="next-steps"></a>Nächste Schritte

Lesen Sie die [häufig gestellten Fragen zu Bastion](bastion-faq.md).
