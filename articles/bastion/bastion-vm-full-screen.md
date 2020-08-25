---
title: 'Azure Bastion: Anzeigen einer virtuellen Computersitzung: Vollbild'
description: Erfahren Sie, wie Sie die Ansicht des virtuellen Computers in Ihrem Browser für eine RDP- oder SSH-Verbindung in Azure Bastion in den Vollbildmodus und zurück ändern können.
services: bastion
author: charwen
ms.service: bastion
ms.topic: how-to
ms.date: 02/03/2020
ms.author: charwen
ms.openlocfilehash: fa795d7b066733aba54f8612bbabecf264a11c70
ms.sourcegitcommit: 2bab7c1cd1792ec389a488c6190e4d90f8ca503b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2020
ms.locfileid: "88270290"
---
# <a name="change-to-full-screen-view-for-a-vm-session-azure-bastion"></a>Wechsel zur Vollbildansicht für eine VM-Sitzung: Azure Bastion

Dieser Artikel hilft Ihnen, die Ansicht der VM in Ihrem Browser in den Vollbildmodus zu setzen und wieder zurück. Bevor Sie mit einem virtuellen Computer arbeiten, stellen Sie sicher, dass Sie die Schritte zum [Erstellen eines Azure Bastion-Hosts (Vorschauversion)](bastion-create-host-portal.md) befolgt haben. Stellen Sie dann entweder mit [RDP](bastion-connect-vm-rdp.md) oder [SSH](bastion-connect-vm-ssh.md) eine Verbindung mit der VM her, mit der Sie arbeiten möchten.

## <a name="launch-the-clipboard-tool"></a>Starten des Zwischenablagetools

Starten Sie während der Remotesitzung die Zwischenablagenzugriffs-Toolpalette von Bastion durch Auswahl von zwei Pfeilen, die sich in der Mitte der Sitzung links befinden.

![Tools](./media/bastion-vm-manage/left.png)

## <a name="select-full-screen"></a>Auswählen des Vollbilds

Wählen Sie die **Vollbild**-Schaltfläche aus, um die Sitzung auf eine Vollbild-Benutzeroberfläche umzuschalten. Sobald Sie umschalten, wird die Sitzung mit Vollbild erneut initialisiert.

![Vollbild](./media/bastion-vm-manage/full-screen.png)
 
## <a name="next-steps"></a>Nächste Schritte

Lesen Sie die [häufig gestellten Fragen zu Bastion](bastion-faq.md).
Lernen Sie das [Kopieren und Einfügen](bastion-vm-copy-paste.md) in eine und aus einer Azure-VM kennen.
