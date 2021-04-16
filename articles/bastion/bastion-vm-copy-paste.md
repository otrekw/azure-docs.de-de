---
title: 'Kopieren und Einfügen bei virtuellen Computern: Azure Bastion'
description: In diesem Artikel lernen Sie das Kopieren und Einfügen bei einer Azure-VM mit Bastion kennen.
services: bastion
author: cherylmc
ms.service: bastion
ms.topic: how-to
ms.date: 03/22/2021
ms.author: cherylmc
ms.openlocfilehash: 4b0c2b734366f9a74a9b007ab9450ab4b4f51feb
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "104800430"
---
# <a name="copy-and-paste-to-a-virtual-machine-azure-bastion"></a>Kopieren und Einfügen bei virtuellen Computern: Azure Bastion

Dieser Artikel hilft Ihnen, Text mit Azure Bastion aus VMs zu kopieren und in VMs einzufügen. Bevor Sie mit einem virtuellen Computer arbeiten, stellen Sie sicher, dass Sie die Schritte zum [Erstellen eines Azure Bastion-Hosts (Vorschauversion)](./tutorial-create-host-portal.md) befolgt haben. Stellen Sie dann entweder mit [RDP](bastion-connect-vm-rdp.md) oder [SSH](bastion-connect-vm-ssh.md) eine Verbindung mit der VM her, mit der Sie arbeiten möchten.

Für Browser, die den erweiterten Zugriff auf die Zwischenablage-API unterstützen, können Sie Text zwischen Ihrem lokalen Gerät und der Remotesitzung genauso kopieren und einfügen wie zwischen Anwendungen und Ihrem lokalen Gerät. Für andere Browser können Sie die Zwischenablagenzugriffs-Toolpalette von Bastion verwenden.

>[!NOTE]
>Derzeit wird nur das Kopieren/Einfügen von Text unterstützt.
>

   ![Zulassen der Zwischenablage](./media/bastion-vm-manage/allow.png)

Nur das Kopieren/Einfügen von Text wird unterstützt. Zum direkten Kopieren und Einfügen fordert Ihr Browser Sie möglicherweise auf, den Zugriff auf die Zwischenablage zuzulassen, wenn die Bastion-Sitzung initialisiert wird. **Erlauben** Sie den Zugriff der Webseite auf die Zwischenablage. Wenn Sie mit einem Mac arbeiten, lautet die Tastenkombination **UMSCHALT-STRG-V**.

## <a name="copy-to-a-remote-session"></a><a name="to"></a>Kopieren in eine Remotesitzung

Führen Sie die folgenden Schritte aus, nachdem Sie im [Azure-Portal](https://portal.azure.com) eine Verbindung mit dem virtuellen Computer hergestellt haben:

1. Kopieren Sie den Text/Inhalt vom lokalen Gerät in die lokale Zwischenablage.
1. Starten Sie während der Remotesitzung die Zwischenablagenzugriffs-Toolpalette von Bastion durch Auswahl von zwei Pfeilen. Die Pfeile befinden sich in der Mitte der Sitzung links.

   ![Screenshot mit hervorgehobenen Pfeilen zum Starten der Toolpalette auf der linken Seite des Fensters](./media/bastion-vm-manage/left.png)

   ![Der Screenshot zeigt eine Zwischenablage für in Bastion kopierten Text.](./media/bastion-vm-manage/clipboard.png)
1. Normalerweise wird der kopierte Text automatisch in der Bastion-Palette zum Kopieren/Einfügen angezeigt. Wenn der Text nicht angezeigt wird, fügen Sie den Text in den Textbereich auf der Palette ein.
1. Sobald der Text im Textbereich ist, können Sie ihn in der Remotesitzung einfügen.

   ![Screenshot mit hervorgehobener Schaltfläche zum Kopieren/Einfügen und einer in der Remotesitzung eingefügten Beispieltextzeichenfolge](./media/bastion-vm-manage/local.png)

## <a name="copy-from-a-remote-session"></a><a name="from"></a>Kopieren aus einer Remotesitzung

Führen Sie die folgenden Schritte aus, nachdem Sie im [Azure-Portal](https://portal.azure.com) eine Verbindung mit dem virtuellen Computer hergestellt haben:

1. Kopieren Sie den Text/Inhalt von der Remotesitzung in die Remotezwischenablage (mit STRG+C).

   ![Toolpalette](./media/bastion-vm-manage/remote.png)
1. Starten Sie während der Remotesitzung die Zwischenablagenzugriffs-Toolpalette von Bastion durch Auswahl von zwei Pfeilen. Die Pfeile befinden sich in der Mitte der Sitzung links.

   ![Zwischenablage](./media/bastion-vm-manage/clipboard2.png)
1. Normalerweise wird der kopierte Text automatisch in der Bastion-Palette zum Kopieren/Einfügen angezeigt. Wenn der Text nicht angezeigt wird, fügen Sie den Text in den Textbereich auf der Palette ein.
1. Sobald der Text im Textbereich ist, können Sie ihn auf dem lokalen Gerät einfügen.

   ![Einfügen](./media/bastion-vm-manage/local2.png)
 
## <a name="next-steps"></a>Nächste Schritte

Lesen Sie die [häufig gestellten Fragen zu Bastion](bastion-faq.md).
