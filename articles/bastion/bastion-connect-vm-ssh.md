---
title: Herstellen einer Verbindung mit einem virtuellen Linux-Computer unter Verwendung von Azure Bastion
description: In diesem Artikel erfahren Sie, wie Sie über Azure Bastion eine Verbindung mit einem virtuellen Linux-Computer herstellen.
services: bastion
author: cherylmc
ms.service: bastion
ms.topic: how-to
ms.date: 02/24/2020
ms.author: cherylmc
ms.openlocfilehash: f9bc06e58fcbd93fba0c92e91b51503239b5a951
ms.sourcegitcommit: bdd5c76457b0f0504f4f679a316b959dcfabf1ef
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90980770"
---
# <a name="connect-using-ssh-to-a-linux-virtual-machine-using-azure-bastion"></a>Herstellen einer SSH-Verbindung mit einem virtuellen Linux-Computer über Azure Bastion

Dieser Artikel zeigt Ihnen, wie Sie in einem virtuellen Azure-Netzwerk sicher und problemlos eine SSH-Verbindung mit Linux-VMs herstellen können. Sie können sich direkt vom Azure-Portal aus mit einer VM verbinden. Wenn Sie Azure Bastion verwenden, benötigen VMs keinen Client, keinen Agent und auch keine zusätzliche Software. Weitere Informationen über Azure Bastion finden Sie in der [Übersicht](bastion-overview.md).

Sie können Azure Bastion verwenden, um eine SSH-Verbindung mit einem virtuellen Linux-Computer herzustellen. Sie können sowohl Benutzername/Kennwort als auch SSH-Schlüssel für die Authentifizierung verwenden. Sie können sich auch über SSH-Schlüssel mit Ihrer VM verbinden, indem Sie Folgendes verwenden:

* einen privaten Schlüssel, den Sie manuell eingeben
* eine Datei, die den privaten Schlüssel enthält

Der private SSH-Schlüssel muss in einem Format vorliegen, das mit `"-----BEGIN RSA PRIVATE KEY-----"` beginnt und mit `"-----END RSA PRIVATE KEY-----"` endet.

## <a name="before-you-begin"></a>Voraussetzungen

Stellen Sie sicher, dass Sie einen Azure Bastion-Host für das virtuelle Netzwerk eingerichtet haben, in dem sich die VM befindet. Weitere Informationen finden Sie unter [Erstellen eines Azure Bastion-Hosts](bastion-create-host-portal.md). Sobald der Bastion-Dienst provisioniert und in Ihrem virtuellen Netzwerk bereitgestellt wurde, können Sie ihn verwenden, um sich mit jeder VM in diesem virtuellen Netzwerk zu verbinden. 

Wenn Sie die Verbindung über Bastion herstellen, wird davon ausgegangen, dass Sie RDP für die Verbindung mit einer Windows-VM und SSH für die Verbindung mit Linux-VMs verwenden. Informationen zur Verbindung mit einer Windows-VM finden Sie unter [Herstellen einer Verbindung mit einer VM – Windows](bastion-connect-vm-rdp.md).

### <a name="required-roles"></a>Erforderliche Rollen

Zum Herstellen einer Verbindung sind die folgenden Rollen erforderlich:

* Rolle „Leser“ auf dem virtuellen Computer
* Rolle „Leser“ auf dem Netzwerkadapter mit privater IP-Adresse des virtuellen Computers
* Rolle „Leser“ für die Azure Bastion-Ressource

### <a name="ports"></a>Ports

Zum Herstellen einer Verbindung mit dem virtuellen Linux-Computer über SSH müssen die folgenden Ports auf Ihrer VM geöffnet sein:

* Eingehender Port: SSH (22)

## <a name="connect-using-username-and-password"></a><a name="username"></a>Verbinden: Verwenden von Benutzernamen und Kennwort

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com). Navigieren Sie zu dem virtuellen Computer, mit dem Sie eine Verbindung herstellen möchten, klicken Sie dann auf **Verbinden**, und wählen Sie **Bastion** aus der Dropdownliste aus.

   ![Der Screenshot zeigt die Übersicht für einen virtuellen Computer im Azure-Portal bei ausgewähltem „Verbinden“.](./media/bastion-connect-vm-ssh/connect.png)
1. Nachdem Sie auf Bastion geklickt haben, wird eine Seitenleiste mit drei Registerkarten angezeigt – RDP, SSH und Bastion. Wenn Bastion für das virtuelle Netzwerk bereitgestellt wurde, ist die Registerkarte Bastion standardmäßig aktiv. Wenn Sie Bastion nicht für das virtuelle Netzwerk bereitgestellt haben, lesen Sie bitte den Abschnitt zum [Konfigurieren von Bastion](bastion-create-host-portal.md).

   ![Der Screenshot zeigt das Dialogfeld zum Herstellen einer Verbindung mit einem virtuellem Computer mit ausgewählter Option „BASTION“.](./media/bastion-connect-vm-ssh/bastion.png)
1. Geben Sie den Benutzernamen und das Kennwort für die SSH-Verbindung mit dem virtuellen Computer ein.
1. Klicken Sie auf die Schaltfläche **Verbinden**, nachdem Sie den Schlüssel eingegeben haben.

## <a name="connect-manually-enter-a-private-key"></a><a name="privatekey"></a>Verbinden: Manuelle Eingabe eines privaten Schlüssels

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com). Navigieren Sie zu dem virtuellen Computer, mit dem Sie eine Verbindung herstellen möchten, klicken Sie dann auf **Verbinden**, und wählen Sie **Bastion** aus der Dropdownliste aus.

   ![Der Screenshot zeigt die Übersicht für einen virtuellen Computer im Azure-Portal bei ausgewähltem „Verbinden“.](./media/bastion-connect-vm-ssh/connect.png)
1. Nachdem Sie auf Bastion geklickt haben, wird eine Seitenleiste mit drei Registerkarten angezeigt – RDP, SSH und Bastion. Wenn Bastion für das virtuelle Netzwerk bereitgestellt wurde, ist die Registerkarte Bastion standardmäßig aktiv. Wenn Sie Bastion nicht für das virtuelle Netzwerk bereitgestellt haben, lesen Sie bitte den Abschnitt zum [Konfigurieren von Bastion](bastion-create-host-portal.md).

   ![Der Screenshot zeigt das Dialogfeld zum Herstellen einer Verbindung mit einem virtuellem Computer mit ausgewählter Option „BASTION“.](./media/bastion-connect-vm-ssh/bastion.png)
1. Geben Sie den Benutzernamen ein, und wählen Sie **Privater SSH-Schlüssel** aus.
1. Geben Sie Ihren privaten Schlüssel in das Textfeld **Privater SSH-Schlüssel** ein (bzw. fügen Sie ihn direkt ein).
1. Klicken Sie auf die Schaltfläche **Verbinden**, nachdem Sie den Schlüssel eingegeben haben.

## <a name="connect-using-a-private-key-file"></a><a name="ssh"></a>Verbinden: Verwenden einer Datei mit einem privaten Schlüssel

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com). Navigieren Sie zu dem virtuellen Computer, mit dem Sie eine Verbindung herstellen möchten, klicken Sie dann auf **Verbinden**, und wählen Sie **Bastion** aus der Dropdownliste aus.

   ![Der Screenshot zeigt die Übersicht für einen virtuellen Computer im Azure-Portal bei ausgewähltem „Verbinden“.](./media/bastion-connect-vm-ssh/connect.png)
1. Nachdem Sie auf Bastion geklickt haben, wird eine Seitenleiste mit drei Registerkarten angezeigt – RDP, SSH und Bastion. Wenn Bastion für das virtuelle Netzwerk bereitgestellt wurde, ist die Registerkarte Bastion standardmäßig aktiv. Wenn Sie Bastion nicht für das virtuelle Netzwerk bereitgestellt haben, lesen Sie bitte den Abschnitt zum [Konfigurieren von Bastion](bastion-create-host-portal.md).

   ![Der Screenshot zeigt das Dialogfeld zum Herstellen einer Verbindung mit einem virtuellem Computer mit ausgewählter Option „BASTION“.](./media/bastion-connect-vm-ssh/bastion.png)
1. Geben Sie den Benutzernamen ein, und wählen Sie **Privater SSH-Schlüssel aus lokaler Datei** aus.
1. Klicken Sie auf die Schaltfläche **Durchsuchen** (das Ordnersymbol in der lokalen Datei).
1. Suchen Sie nach der Datei, und klicken Sie dann **Öffnen**.
1. Klicken Sie auf **Verbinden**, um die Verbindung mit dem virtuellen Computer herzustellen. Sobald Sie auf „Verbinden“ klicken, wird SSH zu diesem virtuellen Computer direkt im Azure-Portal geöffnet. Die Verbindung erfolgt über HTML5 mit Port 443 im Bastion-Dienst und über die private IP-Adresse Ihres virtuellen Computers.

## <a name="next-steps"></a>Nächste Schritte

Lesen Sie die [häufig gestellten Fragen zu Bastion](bastion-faq.md).
