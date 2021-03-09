---
title: Herstellen einer Verbindung mit Ihrem Azure Percept DK über SSH
description: Erfahren Sie, wie Sie mithilfe von PuTTY eine SSH-Verbindung mit Ihrem Azure Percept DK herstellen.
author: elqu20
ms.author: v-elqu
ms.service: azure-percept
ms.topic: how-to
ms.date: 02/03/2021
ms.custom: template-how-to
ms.openlocfilehash: 8dda18271de9b7d65246f0882ee7a68191031c05
ms.sourcegitcommit: 4b7a53cca4197db8166874831b9f93f716e38e30
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "102096614"
---
# <a name="connect-to-your-azure-percept-dk-over-ssh"></a>Herstellen einer Verbindung mit Ihrem Azure Percept DK über SSH

Führen Sie die folgenden Schritte aus, um über [PuTTY](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html) eine SSH-Verbindung mit Ihrem Azure Percept DK einzurichten.

## <a name="prerequisites"></a>Voraussetzungen

- Ein Windows-, Linux- oder OS X-basierter Hostcomputer mit WLAN-Funktion
- Ein SSH-Client
    - Wenn auf dem Hostcomputer Windows ausgeführt wird, ist [PuTTY](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html) ein effektiver SSH-Client und wird in diesem Handbuch verwendet.
    - Wenn auf dem Hostcomputer Linux oder OS X ausgeführt wird, sind SSH-Dienste in diesen Betriebssystemen enthalten und können ohne separate Clientanwendung ausgeführt werden. Weitere Informationen zum Ausführen von SSH-Diensten finden Sie in der Produktdokumentation des Betriebssystems.
- Azure Percept DK
- Einrichten eines SSH-Anmeldekontos während des [Onboardings von Azure Percept DK](./quickstart-percept-dk-set-up.md)

## <a name="initiate-the-ssh-connection"></a>Initiieren der SSH-Verbindung

1. Einschalten Ihres Azure Percept DK (DevKit)

1. Wenn Ihr DevKit Kit bereits über Ethernet oder WLAN mit einem Netzwerk verbunden ist, fahren Sie mit dem nächsten Schritt fort. Andernfalls verbinden Sie Ihren Hostcomputer direkt mit dem WLAN-Zugangspunkt des DevKits, so wie Sie es mit jedem anderen WLAN-Netzwerk tun:
    - **Netzwerkname**: scz-xxxx (wobei „xxxx“ die letzten vier Ziffern der MAC-Netzwerkadresse des DevKits sind)
    - **Kennwort**: Sie finden es auf der Begrüßungskarte, die im Lieferumfang des DevKits enthalten war.

    > [!WARNING]
    > Während Sie eine Verbindung mit dem WLAN-Zugriffspunkt des Azure Percept DK herstellen, verliert Ihr Hostcomputer vorübergehend seine Internetverbindung. Aktive Videokonferenzanrufe, Webstreaming oder andere netzwerkbasierte Erfahrungen werden unterbrochen, bis Schritt 3 der Azure Percept DK-Onboardingerfahrung abgeschlossen ist.

1. Öffnen Sie PuTTY. Geben Sie Folgendes ein, und klicken Sie auf **Öffnen**, um per SSH eine Verbindng mit Ihrem DevKit herzustellen:

    1. Hostname: 10.1.1.1
    1. Port: 22
    1. Verbindungstyp: SSH

    > [!NOTE]
    > Der **Hostname** ist die IP-Adresse Ihres Geräts. Wenn Ihr DevKit mit dem WLAN-Zugangspunkt des DevKits verbunden ist, lautet die IP-Adresse 10.1.1.1. Wenn Ihr DevKit über Ethernet verbunden ist, verwenden Sie die lokale IP-Adresse des Geräts, die Sie auf dem Ethernet-Router oder-Hub finden. Wenn Ihr Gerät über WLAN verbunden ist, müssen Sie die IP-Adresse verwenden, die während des [Onboardings des Azure Percept DK](./quickstart-percept-dk-set-up.md) bereitgestellt wurde.

    :::image type="content" source="./media/how-to-ssh-into-percept-dk/ssh-putty.png" alt-text="Abbildung.":::

1. Melden Sie sich mit dem während des Onboardings erstellten SSH-Benutzernamen und -Kennwort beim PuTTY-Terminal an.

## <a name="next-steps"></a>Nächste Schritte

Nach der erfolgreichen Verbindung mit Ihrem Azure Percept DK über SSH können Sie eine Reihe von Aufgaben durchführen, einschließlich Fehlerbehebung, USB-Updates und Ausführung von DiagTool oder des SoftAP-Tools.