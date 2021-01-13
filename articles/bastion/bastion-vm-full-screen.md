---
title: 'Azure Bastion: Anzeigen einer virtuellen Computersitzung: Vollbild'
description: Erfahren Sie, wie Sie die Ansicht des virtuellen Computers in Ihrem Browser für eine RDP- oder SSH-Verbindung in Azure Bastion in den Vollbildmodus und zurück ändern können.
services: bastion
author: cherylmc
ms.service: bastion
ms.topic: how-to
ms.date: 02/03/2020
ms.author: cherylmc
ms.openlocfilehash: 7db85c5e1d5f67c30adf08969c51bb4c57838fb5
ms.sourcegitcommit: a92fbc09b859941ed64128db6ff72b7a7bcec6ab
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2020
ms.locfileid: "92079138"
---
# <a name="change-to-full-screen-view-for-a-vm-session-azure-bastion"></a>Wechsel zur Vollbildansicht für eine VM-Sitzung: Azure Bastion

Dieser Artikel hilft Ihnen, die Ansicht der VM in Ihrem Browser in den Vollbildmodus zu setzen und wieder zurück. Bevor Sie mit einem virtuellen Computer arbeiten, stellen Sie sicher, dass Sie die Schritte zum [Erstellen eines Azure Bastion-Hosts (Vorschauversion)](./tutorial-create-host-portal.md) befolgt haben. Stellen Sie dann entweder mit [RDP](bastion-connect-vm-rdp.md) oder [SSH](bastion-connect-vm-ssh.md) eine Verbindung mit der VM her, mit der Sie arbeiten möchten.

## <a name="launch-the-clipboard-tool"></a>Starten des Zwischenablagetools

Starten Sie während der Remotesitzung die Zwischenablagenzugriffs-Toolpalette von Bastion durch Auswahl von zwei Pfeilen, die sich in der Mitte der Sitzung links befinden.

![Tools](./media/bastion-vm-manage/left.png)

## <a name="select-full-screen"></a>Auswählen des Vollbilds

Wählen Sie die **Vollbild**-Schaltfläche aus, um die Sitzung auf eine Vollbild-Benutzeroberfläche umzuschalten. Sobald Sie umschalten, wird die Sitzung mit Vollbild erneut initialisiert.

![Vollbild](./media/bastion-vm-manage/full-screen.png)
 
## <a name="next-steps"></a>Nächste Schritte

Lesen Sie die [häufig gestellten Fragen zu Bastion](bastion-faq.md).
Lernen Sie das [Kopieren und Einfügen](bastion-vm-copy-paste.md) in eine und aus einer Azure-VM kennen.
