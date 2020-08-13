---
title: Erweitern eines vorhandenen Hostpools um neue Sitzungshosts – Azure
description: Hier finden Sie Informationen dazu, wie Sie einen vorhandenen Hostpool in Windows Virtual Desktop um neue Sitzungshosts erweitern.
author: Heidilohr
ms.topic: how-to
ms.date: 04/30/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: 339fe25a5a75ca8bc5cc0af7c4d96651e90c7eb6
ms.sourcegitcommit: 98854e3bd1ab04ce42816cae1892ed0caeedf461
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/07/2020
ms.locfileid: "88002448"
---
# <a name="expand-an-existing-host-pool-with-new-session-hosts"></a>Erweitern eines vorhandenen Hostpools um neue Sitzungshosts

>[!IMPORTANT]
>Dieser Inhalt gilt für Windows Virtual Desktop mit Windows Virtual Desktop-Objekten für Azure Resource Manager. Wenn Sie Windows Virtual Desktop (klassisch) ohne Azure Resource Manager-Objekte verwenden, finden Sie weitere Informationen in [diesem Artikel](./virtual-desktop-fall-2019/expand-existing-host-pool-2019.md).

Wenn Sie die Nutzung in Ihrem Hostpool erhöhen, müssen Sie den vorhandenen Pool möglicherweise um neue Sitzungshosts erweitern, um die neue Last zu verarbeiten.

In diesem Artikel wird beschrieben, wie Sie einen vorhandenen Hostpool um neue Sitzungshosts erweitern.

## <a name="what-you-need-to-expand-the-host-pool"></a>Voraussetzungen für die Erweiterung des Hostpools

Bevor Sie beginnen, stellen Sie sicher, dass der Hostpool und die Sitzungshost-VMs mit einer der folgenden Methoden erstellt wurden:

- [Azure portal](./create-host-pools-azure-marketplace.md)
- [Erstellen eines Hostpools mit PowerShell](./create-host-pools-powershell.md)

Sie benötigen außerdem folgende Informationen aus dem Vorgang der ersten Erstellung des Hostpools und der Sitzungshost-VMs:

- Größe, Image und Namenspräfix der VM
- Administratoranmeldeinformationen des Domänenbeitritts
- Name des virtuellen Netzwerks und des Subnetzes

## <a name="add-virtual-machines-with-the-azure-portal"></a>Hinzufügen von virtuellen Computern mit dem Azure-Portal

So erweitern Sie den Hostpool durch Hinzufügen von virtuellen Computern:

1. Melden Sie sich beim Azure-Portal an.

2. Suchen Sie nach **Windows Virtual Desktop**, und wählen Sie diese Option aus.

3. Wählen Sie im Menü auf der linken Seite des Bildschirms **Hostpools**aus, und wählen Sie dann den Namen des Hostpools aus, dem Sie virtuelle Computer hinzufügen möchten.

4. Wählen Sie im Menü auf der linken Seite des Bildschirms die Option **Sitzungshosts** aus.

5. Wählen Sie **+Hinzufügen** aus, um mit dem Erstellen des Hostpools zu beginnen.

6. Ignorieren Sie die Registerkarte mit den Grundeinstellungen, und wählen Sie stattdessen die Registerkarte **VM-Details** aus. Hier können Sie die Details des virtuellen Computers (VM) anzeigen und bearbeiten, den Sie dem Hostpool hinzufügen möchten.

7. Wählen Sie die Ressourcengruppe aus, unter der Sie die VMs erstellen möchten, und wählen Sie dann die Region aus. Sie können die aktuelle Region auswählen, die Sie verwenden, oder eine neue Region.

8. Geben Sie die Anzahl der Sitzungshosts, die Sie zu Ihrem Hostpool hinzufügen möchten, in **Anzahl von VMs** ein. Wenn Sie z. B. Ihren Hostpool um fünf Hosts erweitern möchten, geben Sie **5** ein.

    >[!NOTE]
    >Sie können die Größe oder das Image der VMs nicht bearbeiten, da es wichtig ist, sicherzustellen, dass alle VMs im Hostpool die gleiche Größe besitzen.

9. Wählen Sie als **Informationen zum virtuellen Netzwerk** das virtuelle Netzwerk und das Subnetz aus, mit dem die virtuellen Computer verknüpft werden sollen. Sie können dasselbe virtuelle Netzwerk auswählen, das zurzeit bereits von Ihren vorhandenen Computern verwendet wird, oder ein anderes virtuelles Netzwerk auswählen, das besser für die Region geeignet ist, die Sie in Schritt 7 ausgewählt haben.

10. Geben Sie als **Administratorkonto** den Active Directory-Domänenbenutzernamen und das Kennwort ein, der bzw. das dem ausgewählten virtuellen Netzwerk zugeordnet ist. Diese Anmeldeinformationen werden verwendet, um die virtuellen Computer mit dem virtuellen Netzwerk zu verknüpfen.

      >[!NOTE]
      >Stellen Sie sicher, dass Ihre Administratornamen den hier angegebenen Informationen entsprechen. Vergewissern Sie sich außerdem, dass keine mehrstufige Authentifizierung (MFA) für das Konto aktiviert ist.

11. Wählen Sie die Registerkarte **Tag** aus, wenn Sie über Tags verfügen, mit denen Sie die virtuellen Computer gruppieren möchten. Überspringen Sie diese Registerkarte andernfalls.

12. Wählen Sie die Registerkarte **Überprüfen und erstellen** aus. Überprüfen Sie Ihre Auswahl, und wählen Sie **Erstellen** aus, wenn alles in Ordnung ist.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie Ihren vorhandenen Hostpool erweitert haben, können Sie sich bei einem Windows Virtual Desktop-Client anmelden, um die Einstellungen im Rahmen einer Benutzersitzung zu testen. Sie können jeden der folgenden Clients verwenden, um eine Verbindung mit einer Sitzung herzustellen:

- [Herstellen einer Verbindung mit dem Windows-Desktopclient](./connect-windows-7-10.md)
- [Herstellen einer Verbindung mit dem Webclient](./connect-web.md)
- [Herstellen einer Verbindung mit dem Android-Client](./connect-android.md)
- [Herstellen einer Verbindung mit dem macOS-Client](./connect-macos.md)
- [Herstellen einer Verbindung mit dem iOS-Client](./connect-ios.md)
